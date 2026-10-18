# Analysis of my implementation

Write this report in English. Replace every `REPLACE_WITH` with your own explanation or recorded evidence. Refer to the assignment for the full requirements.

## Algorithms and correctness

For each method, explain the maintained state or invariant, main steps, correctness, relevant boundary cases, and time and memory complexity including output.

- DSU, path compression and union by size/rank: REPLACE_WITH
- Kruskal and safe edge selection: REPLACE_WITH
- Prim, the priority queue and all connected components: REPLACE_WITH
- Kosaraju, the two DFS passes and condensation: REPLACE_WITH
- DFS topological ordering and cycle detection: REPLACE_WITH
- DAG shortest paths and parent edge IDs: REPLACE_WITH

Explain how every request starts with fresh state and how deep DFS avoids call-stack failure.

REPLACE_WITH

## Tests

Link the inputs, independently justified expected answers, actual results and commands under `tests/student/`. Explain the mistakes your cases detect. Include boundary cases, large/deep graphs and repeated mixed requests in one process. Describe how you accept alternative correct witnesses and reject incorrect ones where applicable.

REPLACE_WITH

## Route reconstruction

Show the actual helper calls and results for a reachable target, the source and an unreachable target. State your no-route representation. Include a tied/parallel-edge example. List the selected parent edge IDs and check route direction and total weight.

REPLACE_WITH

## Kruskal versus Prim

Use the same graph instances, environment and timing boundary. Check correct costs and valid forests before timing. Exclude compilation. Record the generator and seed or stored inputs, commands, runtime and hardware, timer, warm-up, and whether graph construction, parsing and input/output are included.

REPLACE_WITH

| Size | n | m | Kruskal measurements (ms) | Median (ms) | Prim measurements (ms) | Median (ms) |
|---|---:|---:|---|---:|---|---:|
| Small | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH |
| Medium | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH |
| Large | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH | REPLACE_WITH |

At each size, provide at least three measurements per method. Explain observations and experimental limitations separately from the asymptotic argument.

REPLACE_WITH

## Sources and assistance

Identify external sources and AI tools, their contribution, your changes and your own verification. If none were used, say so. Do not claim tests or measurements you did not perform.

REPLACE_WITH
