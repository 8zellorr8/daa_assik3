# Java starter / Заготовка Java

## Your implementation

Implement the six methods in [source/Algorithms.java](source/Algorithms.java) and add your route-reconstruction helper. Each method already receives its input as ordinary variables:

- `n`: vertex count (`int`).
- `operations[i]`: endpoints of DSU operation `i` (`int[]`).
- `edges[i]`: endpoints and, for weighted tasks, weight of input edge `i` (`int[]`). Its original ID is `i`.
- `source`: source vertex for DAG shortest paths (`int`).

The supplied [Input.java](source/Input.java) converts JSON values to these variables. [Main.java](source/Main.java) reads requests and calls methods; [Json.java](source/Json.java) parses and writes JSON. You do not need to study or modify these support files.

Return the fields specified in the assignment. For example, after your DSU implementation has built `states`, package it as:

```java
Map<String, Object> result = new java.util.LinkedHashMap<>();
result.put("states", states);
return result;
```

The JSON writer supports lists and Java arrays. Keep totals and distances in `long`; a legal individual weight fits `int`, but a sum may not. If using an array containing both distances and `null`, use `Long[]`, since `long[]` cannot store `null`.

For deep DFS, an explicit stack represents unfinished calls. A frame stores a vertex and its next-neighbour position, so traversal can resume and record a vertex only when it finishes. For a spanning forest, process every connected component, including isolates: a graph with `c` components needs `n-c` forest edges. To reconstruct a route, follow each selected parent edge to its tail, stop at the source and reverse the collected vertices; handle an unreachable target separately. Implement these operations yourself.

Run checks from the repository root with `python tools/run.py --check`, or push to your copy and inspect **Actions → Public checks**. Local checks require JDK 17+ and Python 3.12+. TODO failures are expected before implementation. See [the assignment](../ASSIGNMENT_EN.md) for all requirements.

## Ваша реализация

Реализуйте шесть методов в [source/Algorithms.java](source/Algorithms.java) и добавьте свою функцию восстановления пути. Данные уже преобразованы в обычные переменные: `n` и `source` имеют тип `int`, `operations` и `edges` — `int[][]`. Строка `edges[i]` хранит концы ребра и его вес для взвешенных задач; исходный ID ребра равен `i`.

`Input.java` преобразует входные данные, `Main.java` читает запросы и вызывает методы, `Json.java` обрабатывает JSON. Изучать или менять эти служебные файлы не требуется. Готовые алгоритмы в них отсутствуют.

Возвращайте поля из условия через `Map<String, Object>`, как в примере выше. Для суммарной стоимости и расстояний используйте `long`: отдельный вес помещается в `int`, сумма может не поместиться. Массив расстояний с `null` можно хранить в `Long[]`; примитивный `long[]` не хранит `null`.

При глубоком DFS явный стек хранит незавершённые вызовы: вершину и позицию следующего соседа. Это позволяет продолжать обход и фиксировать завершение вершины. Для остовного леса обрабатывайте все компоненты, включая изоляты; требуется `n-c` рёбер. При восстановлении пути переходите по выбранному родительскому ребру к его началу, дойдите до источника и разверните список; недостижимую цель обработайте отдельно. Эти действия реализуйте самостоятельно.

Из корня репозитория запускайте `python tools/run.py --check` либо отправляйте код в свою копию и смотрите **Actions → Public checks**. Для локальной проверки нужны JDK 17+ и Python 3.12+. До заполнения TODO ошибки ожидаемы. Полные требования приведены в [русском условии](../ASSIGNMENT_RU.md).
