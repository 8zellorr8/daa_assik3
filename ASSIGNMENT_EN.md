# Assignment 3: Graph Algorithms for a Campus Festival

**Course:** Design and Analysis of Algorithms  
**Practical work:** 50 points  
**Language:** Java 17+, Python 3.12+, C++17 or C# (.NET 8)  
**Deadline:** as announced in Moodle

## 1. Objective

A campus festival requires cable connections between venues, an order for preparation tasks and delivery routes. Model cable connections as undirected edges and dependencies and delivery routes as directed edges. Each task receives its own graph; do not combine graphs from different requests.

Implement six methods: `dsu`, `kruskal`, `prim`, `scc`, `topo` and `dag`. Use the algorithms specified below. Implement the graph algorithms yourself. Standard collections, sorting, priority queues and JSON input/output utilities are permitted.

## 2. Program requirements

Choose one programming language. Your program must read UTF-8 JSON Lines from standard input and write one JSON result object per input line to standard output. Do not print prompts, timing information or debugging messages to standard output. Several requests may be processed in one execution. Initialise the algorithm state separately for every request.

Vertices are numbered from `0` to `n-1`. An edge ID is its original zero-based position in the input `edges` array. Preserve IDs when sorting or building adjacency lists. Use signed 64-bit arithmetic for weights, total costs and finite distances. Python integers satisfy this requirement. Use JSON `null` for an unreachable distance.

All limits are inclusive. Let `n` be the number of vertices, `m` the number of edges and `q` the number of union operations.

| Method | Vertices | Edges or operations | Edge weights |
|---|---|---|---|
| `dsu` | `0 <= n <= 100000` | `0 <= q <= 250000` | Not used |
| `kruskal`, `prim` | `0 <= n <= 100000` | `0 <= m <= 250000` | `-1000000000 <= w <= 1000000000` |
| `scc`, `topo` | `0 <= n <= 100000` | `0 <= m <= 250000` | Not used |
| `dag` | `1 <= n <= 100000` | `0 <= m <= 250000` | `-1000000000 <= w <= 1000000000` |

Every edge endpoint and operation argument is a valid vertex. For `n=0`, the edge or operation list is empty. DAG requests contain a valid `source`, with `0 <= source < n`. Inputs satisfy these requirements; malformed JSON and out-of-range inputs are not assessed. Field names and task names are case-sensitive.

Your implementation must handle deep graphs without call-stack failure. Include returned results when stating memory complexity. Output object field order is unrestricted. Where several answers are valid, any answer satisfying the stated conditions is accepted.

For an execution check, also support `{"task":"ping"}` with the response `{"status":"ready"}`. This checks input/output only and is not a seventh graph algorithm.

## 3. Connectivity: disjoint-set union

Venues are connected by cables added one at a time. Implement `find` and `union` with path compression and union by size or rank. Initially every vertex forms a separate component.

**Input:** `task="dsu"`, `n`, and `operations`, an array of pairs `[u,v]` representing undirected connections.

**Output:** `states`, containing `[components,largest]` after each operation. Here `components` is the number of connected components and `largest` is the number of vertices in the largest component. Do not include an initial state before the first operation.

Repeated unions and self-unions are valid and must not change the partition. Initially `largest=1` when `n>0`; for `n=0`, `largest=0` and `states=[]`. If you use union by rank, maintain component sizes separately: rank is not size.

Example input:

```json
{"task":"dsu","n":4,"operations":[[0,1],[1,2],[0,2],[3,3]]}
```

Output:

```json
{"states":[[3,2],[2,3],[2,3],[2,3]]}
```

**Complexity:** `O(n + q α(n))` amortised time and `O(n+q)` memory, including the states. `α(n)` denotes the inverse Ackermann function; `O((n+q) α(n))` is also an acceptable upper bound.

## 4. Minimum-cost cable network: Kruskal and Prim

Find a minimum spanning forest of an undirected weighted graph. Connect every connected component at minimum total cost without cycles. Count isolated vertices as components.

Write two separate implementations:

- `kruskal`: sort edges by weight and use DSU. Accept an edge only when `find(u) != find(v)`.
- `prim`: use adjacency lists and a priority queue. When one component is complete, restart from an unvisited vertex until all vertices are processed.

**Input:** `task="kruskal"` or `task="prim"`, `n`, and `edges`, an array of `[u,v,weight]`.

**Output:** `cost`, `edge_ids` and `components`. Return the sum of the selected edge weights, their original input IDs and the number of connected components in the input graph.

Parallel edges, equal weights, negative weights and self-loops are allowed. Do not select self-loops or repeat an edge ID. For `c` connected components, a spanning forest contains exactly `n-c` edges. For `n=0`, return `cost=0`, `edge_ids=[]`, `components=0`. Different optimal forests and different orders of selected IDs are accepted. Kruskal and Prim must agree on optimum cost and component count; their selected IDs may differ.

Example input:

```json
{"task":"kruskal","n":4,"edges":[[0,1,4],[1,2,2],[0,2,3]]}
```

One accepted output:

```json
{"cost":5,"edge_ids":[1,2],"components":2}
```

Vertex `3` is isolated. Replacing the task name with `prim` asks the other implementation to solve the same graph.

**Complexity:** Kruskal takes `O(n + m log(m+1))` time. Prim takes `O(n + m log(m+1))` with a lazy heap, or `O((n+m) log(n+1))` with an indexed heap. Both use `O(n+m)` memory.

## 5. Circular dependencies: strongly connected components

An edge `u -> v` represents a directed dependency. Find the maximal groups of mutually reachable vertices using **Kosaraju's algorithm**: two DFS passes, a transposed graph and decreasing first-pass finish order for the second pass. You may start on either graph, provided the second pass uses its transpose.

**Input:** `task="scc"`, `n`, and directed `edges` of the form `[u,v]`.

**Output:** `component`, with one nonnegative integer label per vertex, and `condensation`, an array of `[from_label,to_label]` pairs. Equal labels must identify exactly one SCC. Labels need not be consecutive or follow a particular order.

Include every cross-component directed pair exactly once. Exclude internal edges and duplicate pairs. Isolated vertices form separate SCCs. Duplicate input edges and self-loops are allowed. For `n=0`, both output arrays are empty. Condensation edge order is unrestricted.

Example input:

```json
{"task":"scc","n":4,"edges":[[0,1],[1,0],[1,2],[0,2],[2,3]]}
```

One accepted output:

```json
{"component":[5,5,7,9],"condensation":[[5,7],[7,9]]}
```

The condensation is acyclic. It summarises component dependencies; it does not make the original tasks inside a cyclic SCC schedulable.

**Complexity:** `O(n+m)` expected time with hash-based condensation deduplication and `O(n+m)` memory.

## 6. Preparation schedule: DFS topological ordering

Determine whether all preparation tasks can be ordered. An edge `u -> v` means that `u` must finish before `v` starts.

Implement DFS with three states: unvisited, active and finished. An edge to an active vertex indicates a directed cycle. An edge to a finished vertex alone does not indicate a cycle. Check every component and reverse DFS finish order when no cycle exists.

**Input:** `task="topo"`, `n`, and directed `edges` of the form `[u,v]`.

**Output:** `acyclic`, a Boolean, and `order`. For an acyclic graph, return `acyclic=true` and every vertex exactly once, with each edge going forward in the order. For any directed cycle, return `acyclic=false` and `order=[]`. A self-loop is a cycle. For `n=0`, return `acyclic=true` and `order=[]`.

Example input:

```json
{"task":"topo","n":4,"edges":[[0,2],[1,2],[2,3]]}
```

One accepted output:

```json
{"acyclic":true,"order":[1,0,2,3]}
```

**Complexity:** `O(n+m)` time and memory. An explicit DFS stack must retain the progress of each suspended traversal so that neighbours are not repeatedly rescanned.

## 7. Delivery routes: DAG shortest paths

Find shortest paths from a given source in a directed acyclic graph. Negative edge weights are allowed. Check the whole graph for cycles, then relax outgoing edges from reachable vertices in topological order.

**Input:** `task="dag"`, `n`, directed `edges` of the form `[u,v,weight]`, and `source`.

**Output:** `acyclic`, `distance` and `parent_edge`.

- If any component contains a directed cycle, including one unreachable from the source, return `acyclic=false`, `distance=[]`, `parent_edge=[]`.
- Otherwise return `acyclic=true` and arrays of length `n`.
- The source has `distance=0` and `parent_edge=-1`.
- An unreachable vertex has `distance=null` and `parent_edge=-1`.
- Every other vertex has its shortest distance and the original ID of an incoming edge on a shortest source path. The edge must end at that vertex; following parent edges must reach the source.

Never relax from an unreachable vertex. Do not use `-1` as an unreachable distance: it may be a valid finite cost. `parent_edge` stores an edge ID, not a predecessor vertex. Parallel edges and tied shortest routes may produce different accepted parent arrays. A self-loop makes the graph cyclic.

Example input:

```json
{"task":"dag","n":5,"source":0,"edges":[[0,1,3],[0,2,8],[1,2,-7],[2,3,4]]}
```

One accepted output:

```json
{"acyclic":true,"distance":[0,3,-4,0,null],"parent_edge":[-1,0,2,3,-1]}
```

Add a helper that reconstructs a vertex sequence from the selected `parent_edge` IDs. A reachable route must run from source to target; the route to the source is `[source]`. Choose and document a no-route result for an unreachable target, for example an empty list or a null value. This choice concerns the helper only; JSON `distance` must still use `null` for unreachable vertices. Call the helper in your own tests for a reachable target, the source and an unreachable target. Also test tied or parallel edges. In the example, the route to vertex `3` is `[0,1,2,3]`, with cost `0`.

The helper is part of your code, not a separate JSON task. **Complexity:** `O(n+m)` time and memory for DAG shortest paths; `O(L)` time and output space to reconstruct a route containing `L` vertices.

## 8. Tests and analysis

Store your test inputs, independently justified expected results, actual outputs and execution commands under `tests/student/`. Cover the applicable cases: empty graphs, isolates, disconnected components, repeated and self-unions, loops, parallel edges, equal optima, negative weights, totals beyond 32 bits, unreachable vertices, cycles outside source reachability and deep chains. Include consecutive requests of different tasks and sizes in one process, then repeat them to check state reset. Test large graphs within the published limits.

For non-unique answers, verify meaning rather than matching one reference list. Check forest feasibility and optimum cost, SCC equivalence and exact condensation, topological precedence, and shortest distances with valid parent paths. Where applicable, demonstrate that your checking accepts an alternative correct answer and rejects an invalid witness.

Write `ANALYSIS.md` in English. For each of the six methods, explain the main steps, maintained state or invariant, correctness, relevant boundary cases and time/memory complexity. Explain state reset and handling of deep DFS. Include the actual helper calls and results, selected parent edge IDs, and route direction and weight checks.

Compare Kruskal and Prim on the **same graph instances at three sizes**. Check correct costs and valid forests before timing. For each size, report `n`, `m`, the generator and seed or stored input, at least three measurements per method, and their medians. Give the commands, runtime version, hardware or runner, timer and any warm-up. Use the same timing boundary for both methods: state whether graph construction, parsing and input/output are included. Exclude compilation. Explain the observations; a few measurements do not prove an asymptotic bound.

## 9. Repository and submission

Create your solution repository using **Use this template → Create a new repository**. Set `submission.json` with exactly one selected language value: `Java`, `Python`, `Cpp` or `CSharp`. For example:

```json
{"language":"Java"}
```

The entry points and JSON adapters are supplied. Complete the six method bodies in `Algorithms.java`, `algorithms.py`, `algorithms.hpp` or `Algorithms.cs` in the selected source directory. Add your route helper there or in an additional source file in the same directory. Keep the input/output interface below.

| Language | Entry point |
|---|---|
| Java | `Java/source/Main.java`, class `Main`, without a package declaration |
| Python | `Python/source/main.py` |
| C++ | `Cpp/source/main.cpp` |
| C# | `CSharp/source/Program.cs` and `App.csproj`, targeting `net8.0` and producing `App.dll` |

Include the source files, `submission.json`, `tests/student/`, English `ANALYSIS.md` and a solution README with exact build, run and test commands. The program must be runnable from source in the selected environment without downloading dependencies. Include any required JSON utility source in your repository.

Push the completed work and submit the **URL of the exact GitHub commit** in Moodle Online text. Do not submit only a branch or repository URL. Complete the submission using **Submit assignment** and check its status. If the repository is private, give the instructor access. Any replacement submitted before the deadline must identify the updated commit. The linked commit is the version assessed.

Follow the deadline and any approved individual extension shown in Moodle. This document does not change the Moodle dates. The template provides input/output code, algorithm stubs and public checks. Run **Actions → Public checks** after pushing your work, or use the commands in the repository README locally. Public algorithm checks must pass before submission. Link the exact commit in Moodle; an Actions URL may be included as additional evidence. Passing public examples does not establish correctness on every permitted input. In the unchanged instructor template, Actions checks the testing tools and launch of all four languages; student copies also run algorithm examples, which fail until the TODO bodies are implemented.

## 10. Assessment

Practical work is assessed by independent automated tests out of **50 points**. The separate Moodle quiz is worth **50 points**; together they form Assignment 3 out of 100, with the existing 20% contribution to the second attestation.

The practical test groups have these maximum scores. A dash means that the method has no group of that type.

| Method | Core | Edge cases | Witness | Scale | Total |
|---|---:|---:|---:|---:|---:|
| DSU | 4 | 2 | — | 2 | 8 |
| Kruskal | 4 | — | 2 | 1 | 7 |
| Prim | 4 | — | 2 | 1 | 7 |
| Kosaraju SCC | 5 | — | 3 | 2 | 10 |
| DFS topological ordering | 4 | 2 | — | 2 | 8 |
| DAG shortest paths | 5 | — | 3 | 2 | 10 |
| **Total** | **26** | **4** | **10** | **10** | **50** |

Core groups check results and optimum values. Edge-case groups check boundary cases. Witness groups check selected forest edges, condensation or parent paths. Scale groups check correct results on large inputs. Each of the 18 groups earns its maximum multiplied by the fraction of its tests passed, rounded half-up to 0.01. Sum the group scores, up to 50. Accept other valid optimal witnesses.

All six specified methods, the helper, analysis, own tests and benchmark remain required learning work. They have no separate manual points or discretionary deductions. Automated result tests alone do not establish the internal algorithm, authorship or truth of timing measurements. Instructor feedback or an explanation of the work does not alter the automatic score.

The instructor execution limits are 60 seconds for compilation and 20 seconds per requirement-group batch on one CPU, with 768 MiB container memory, Java heap up to 512 MiB, 128 processes and 256 MiB tmpfs. UTF-8 standard output is limited to 8 MiB per batch and each source file to 2 MiB.

The quiz contains 30 single-answer questions. Each correct answer earns one raw point; a wrong or blank answer earns zero. The quiz grade is `raw / 30 × 50`, calculated once. There are no oral bonuses or oral score recovery. Confirmed test or question/key defects are corrected consistently for all affected original submissions or attempts. Infrastructure failures require investigation and rerun; they are not recorded as a student zero.

## 11. Academic integrity

Follow the course syllabus. In `ANALYSIS.md`, identify any external sources and AI tools, state their contribution and describe your own verification. If none were used, say so. You remain responsible for correctness, originality and quality. Do not claim tests or measurements that you did not perform.
