# C++ starter / Заготовка C++

Implement the six methods and a route helper in [source/algorithms.hpp](source/algorithms.hpp). Inputs are prepared: `n` and `source` are `int`; `operations` and `edges` are `vector<vector<int>>`. Row `edges[i]` has original ID `i`. Individual weights fit `int`; totals and distances require `long long`.

[input.hpp](source/input.hpp) provides input conversion. [main.cpp](source/main.cpp) reads requests and calls methods; [json.hpp](source/json.hpp) handles JSON. You do not need to study or modify these support files. `J::O` stores an output object, `J::A` stores a JSON array, and `J()` represents JSON `null`.

After computing your DSU states, package them using these provided types:

```cpp
// states is a J::A filled by your implementation.
return J::O{{"states", states}};
// A single state row can be written as J::A{components, largest}.
```

For deep DFS, retain a vertex and next-neighbour position in each explicit frame. Process all components for a spanning forest. Reconstruct paths from original parent edge IDs and reverse the collected vertices. Implement these operations yourself.

Run `python tools/run.py --language Cpp --check` from the repository root, or select Cpp in `submission.json` and use **Actions → Public checks**. Local checks need Python 3.12+ and `g++` with C++17 support. TODO errors are expected until implementation. See [the assignment](../ASSIGNMENT_EN.md).

## Работа студента

Реализуйте шесть методов и функцию восстановления пути в `source/algorithms.hpp`. `n` и `source` имеют тип `int`, `operations` и `edges` — `vector<vector<int>>`. Исходный ID строки `edges[i]` равен `i`. Веса помещаются в `int`, но для сумм и расстояний нужен `long long`.

`input.hpp`, `main.cpp` и `json.hpp` — готовый служебный код. Для результата используйте `J::O` (объект), `J::A` (массив) и `J()` (JSON `null`), как в примере. Разбирать устройство парсера не требуется.

При глубоком DFS храните вершину и позицию следующего соседа в явном стеке. В лес включайте все компоненты; путь восстанавливайте по исходным ID рёбер и разворачивайте. Эти действия реализуйте самостоятельно.

Проверка из корня: `python tools/run.py --language Cpp --check`. Локально нужны Python 3.12+ и компилятор `g++` с поддержкой C++17. Либо выберите Cpp в `submission.json` и отправьте код для проверки Actions. Полное условие — [на русском](../ASSIGNMENT_RU.md).
