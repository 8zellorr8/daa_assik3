# Python starter / Заготовка Python

Implement the six methods and a route helper in [source/algorithms.py](source/algorithms.py). Inputs are already ordinary Python values: `n` and `source` are integers; `operations` and `edges` are lists of lists. Row `edges[i]` has original edge ID `i`. No extra input helper or numeric casts are needed.

[source/main.py](source/main.py) is provided: it reads JSON, calls your method and writes JSON. You do not need to modify it. Return a dictionary with the fields from the assignment, for example `return {"states": states}` after your DSU code has computed `states`. Use `None` for an unreachable distance; the writer converts it to JSON `null`. Python integers can store the required totals.

For deep DFS, keep explicit frames with a vertex and next-neighbour position; do not rely on increasing the recursion limit. A spanning forest processes all components and isolates. Reconstruct a path using original parent edge IDs, then reverse the vertex sequence. Implement these operations yourself.

From the repository root run `python tools/run.py --language Python --check`, or select Python in `submission.json` and push to **Actions → Public checks**. Python 3.12+ is required locally. TODO errors are expected until implementation. See [the assignment](../ASSIGNMENT_EN.md).

## Работа студента

Реализуйте шесть методов и функцию восстановления пути в `source/algorithms.py`. `n` и `source` уже являются целыми числами, `operations` и `edges` — списками списков. Исходный ID строки `edges[i]` равен `i`. Приведения типов и дополнительный разбор JSON не нужны; служебный `main.py` готов.

Возвращайте словарь с полями из условия, например `{"states": states}` после вычисления состояний DSU. Недостижимое расстояние храните как `None`: оно преобразуется в JSON `null`. Для глубокого DFS используйте явный стек с позицией продолжения; лес должен учитывать все компоненты, путь — восстанавливаться по исходным ID рёбер. Эти действия реализуйте самостоятельно.

Проверка из корня: `python tools/run.py --language Python --check`. Либо выберите Python в `submission.json` и отправьте код: Actions запустится автоматически. Локально нужен Python 3.12+. Полное условие — [на русском](../ASSIGNMENT_RU.md).
