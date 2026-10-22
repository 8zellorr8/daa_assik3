# Kruskal and Prim benchmark

Measured at 2026-10-22T14:38:13.718506+00:00 (UTC). These are actual local measurements.

## Reproduce

Run from the repository root:

```sh
python tests/benchmark.py --validate-only
python tests/benchmark.py
```

The runner compiles all Java sources plus `tests/Benchmark.java` with
`javac --release 17 -d .build/benchmark`, then executes
`java -Xms256m -Xmx384m -cp .build/benchmark Benchmark`.
The validation command adds `--validate-only` to the Java invocation.
Exact argument arrays and raw nanoseconds are saved in `tests/benchmark-results.json`.
The validation-only result is saved in `tests/benchmark-validation.json`.

## Environment and measurement boundary

- CPU: 11th Gen Intel(R) Core(TM) i5-11400H @ 2.70GHz; 12 logical processors.
- Physical memory: 15.73 GiB; Java maximum heap: 384 MiB.
- OS: Windows-11-10.0.26100-SP0; Java sees Windows 11 10.0 (amd64).
- Runtime: Java HotSpot(TM) 64-Bit Server VM, Java 25.0.1+8-LTS-27.
- Compiler: javac 25.0.1; `--release 17` enforces Java 17 source/API compatibility.
- Timer: `System.nanoTime()`, elapsed nanoseconds converted to milliseconds below.
- Warm-up: 3 untimed calls per algorithm at each size, after one untimed correctness call per algorithm.
- Samples: 5 calls per algorithm per size in the same JVM; the first algorithm alternates by repetition.
- Both algorithms receive the exact same preconstructed `int[][]` object at each size. Full input equality is checked before and after timing to detect mutations.
- Timing starts immediately before the typed algorithm call and stops after its result map is returned. It includes sorting, adjacency creation, all algorithm allocations, and returned result construction.
- Compilation, JVM startup, graph generation, input copying, parsing, correctness checks, checksum consumption, and all input/output are excluded. No explicit garbage collection is requested.
- Every returned edge ID and scalar is consumed into a volatile checksum after the timer stops, preventing results from being discarded as unused.

## Generator and independent correctness argument

`tests/Benchmark.java` uses `java.util.Random` with base seed 20261022; each graph seed is `base + 31*n + m`.
Vertices `0..n-2` form three contiguous nonempty components; vertex `n-1` is isolated.
Within each component, each vertex after the first receives an edge to a uniformly selected earlier vertex, creating a tree.
These planted tree edges have weights in `[-1000000000, -999900001]`.
Extra edges stay inside one component and have weights in `[0, 1000000000]`; endpoints may repeat and approximately 5% are deliberate self-loops.
The generator shuffles all edges with the same seeded RNG. Original IDs are their positions after this shuffle.

The optimum is computed directly by summing the planted tree weights, independently of either tested algorithm.
Removing any planted tree edge splits its tree into two sets. That edge is the only planted edge crossing this cut; all other crossing edges have nonnegative weights.
It is therefore the unique cheapest edge across its cut and must belong to every minimum spanning tree. All planted edges together form a spanning forest, proving the expected optimum.

Before timing, the checker validates each method separately: selected IDs are in range and distinct, no selected edge is a loop, selected weights sum to the returned cost, and this cost equals the independently proved optimum.
Independent breadth-first traversals label input and selected-edge components. The returned component count must match the input; every input edge must stay inside a selected component; exactly `n-components` edges must be selected.
Together, spanning every input component and this edge count prove acyclicity. This checks the actual witness, rather than merely comparing the two algorithm costs.

## Actual results

All six pre-timing forest and optimum checks passed. Every graph has four components, including the isolate.

| n | m | Seed | Optimum cost | Kruskal median (ms) | Prim median (ms) |
|---:|---:|---:|---:|---:|---:|
| 1000 | 4000 | 20296022 | -995949415446 | 1.260 | 1.401 |
| 10000 | 40000 | 20611022 | -9995504392833 | 12.214 | 12.909 |
| 100000 | 250000 | 23611022 | -99991006962082 | 117.339 | 142.012 |

All individual timings below are milliseconds in execution-repetition order, rounded to three decimals. Medians are computed from unrounded nanoseconds.

| n | Method | Sample 1 | Sample 2 | Sample 3 | Sample 4 | Sample 5 |
|---:|---|---:|---:|---:|---:|---:|
| 1000 | Kruskal | 1.184 | 1.267 | 1.152 | 1.260 | 1.480 |
| 1000 | Prim | 1.394 | 1.401 | 1.583 | 1.505 | 1.377 |
| 10000 | Kruskal | 32.692 | 12.214 | 15.059 | 11.128 | 11.796 |
| 10000 | Prim | 14.108 | 12.922 | 12.812 | 12.828 | 12.909 |
| 100000 | Kruskal | 126.427 | 117.339 | 143.841 | 116.714 | 116.969 |
| 100000 | Prim | 141.651 | 138.648 | 144.849 | 143.503 | 142.012 |

## Observations

- At n=1000, m=4000, Kruskal had the smaller median; the ratio between the larger and smaller median was 1.11.
- At n=10000, m=40000, Kruskal had the smaller median; the ratio between the larger and smaller median was 1.06.
- At n=100000, m=250000, Kruskal had the smaller median; the ratio between the larger and smaller median was 1.21.

These observations describe this run and generator only. Consult the individual samples to see variation; a median reduces the influence of one slow sample but does not eliminate runtime noise.

## Asymptotic argument and limitations

Kruskal sorts `m` edges, then performs near-constant amortized DSU operations, giving `O(n + m log(m+1))` time and `O(n+m)` memory.
Prim builds adjacency lists and processes a lazy priority queue, giving `O(n + m log(m+1))` time and `O(n+m)` memory.
These bounds follow from the implementations, not from fitting three measurements.

The experiment uses three sparse graphs from one deliberately structured distribution, with planted negative optimum edges and four components. It does not characterize dense graphs, arbitrary weight distributions, or every graph shape.
There is one JVM process, five samples per point, and three warm-ups per method per size. JIT compilation, garbage collection, scheduling, CPU frequency changes, cache effects, and background activity can still influence the timings.
No CPU affinity or operating-system isolation was configured. The two algorithms can allocate different amounts of memory even though the measurement boundary is identical.
The local runtime is the version shown above, not Java 17; compilation targets Java 17 but the measurements describe the installed runtime. These data support a reproducible local comparison, not a universal performance claim or proof of complexity.
