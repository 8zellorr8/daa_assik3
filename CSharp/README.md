# C# starter / Заготовка C#

Implement the six methods and a route helper in [source/Algorithms.cs](source/Algorithms.cs). Inputs are prepared: `n` and `source` are `int`; `operations` and `edges` are `int[][]`. Row `edges[i]` has original ID `i`. [Input.cs](source/Input.cs) provides conversion; [Program.cs](source/Program.cs) handles JSON Lines. You do not need to modify these support files or [App.csproj](source/App.csproj).

Return a `JsonObject` with the fields from the assignment. For example, after computing DSU states:

```csharp
JsonObject result = new JsonObject();
result["states"] = System.Text.Json.JsonSerializer.SerializeToNode(states);
return result;
```

`SerializeToNode` converts your array or list into JSON. Use `long` for totals and distances; `long?[]` can contain both distances and `null`. Individual input weights fit `int`, but totals may not.

For deep DFS, retain a vertex and next-neighbour position in each explicit frame. A spanning forest processes all components and isolates. Reconstruct a path using original parent edge IDs and reverse the vertex sequence. Implement these operations yourself.

Run `python tools/run.py --language CSharp --check` from the repository root, or select CSharp in `submission.json` and use **Actions → Public checks**. Local checks require Python 3.12+ and the .NET 8 SDK. TODO errors are expected until implementation. See [the assignment](../ASSIGNMENT_EN.md).

## Ваша реализация

Реализуйте шесть методов и функцию восстановления пути в `source/Algorithms.cs`. Входы уже представлены как `int` и `int[][]`; исходный ID строки `edges[i]` равен `i`. Служебные `Input.cs`, `Program.cs` и `App.csproj` подготовлены.

Возвращайте `JsonObject` с полями из условия. `JsonSerializer.SerializeToNode`, как в примере, преобразует массив или список в JSON. Для сумм и расстояний используйте `long`, для расстояний с `null` подходит `long?[]`.

Для глубокого DFS сохраняйте вершину и позицию продолжения в явном стеке. Лес должен учитывать все компоненты; путь восстанавливайте по исходным ID родительских рёбер и разворачивайте. Эти действия реализуйте самостоятельно.

Проверка из корня: `python tools/run.py --language CSharp --check`. Локально нужны Python 3.12+ и **SDK .NET 8**, а не только runtime. Либо выберите CSharp в `submission.json` и отправьте код для проверки Actions. Полное условие — [на русском](../ASSIGNMENT_RU.md).
