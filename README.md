# Assignment 3: Graph Algorithms for a Campus Festival

**Course:** Design and Analysis of Algorithms

[English assignment](ASSIGNMENT_EN.md) · [Условие на русском](ASSIGNMENT_RU.md)

Implement DSU, Kruskal, Prim, Kosaraju SCC, DFS topological ordering and DAG shortest paths in **one** language. Both assignment versions contain the full requirements, JSON formats, examples and assessment rules. The deadline is announced in Moodle.

## Getting started

1. Select **Use this template → Create a new repository**. Give the instructor access if your repository is private.
2. Set `language` in [submission.json](submission.json) to `Java`, `Python`, `Cpp` or `CSharp`.
3. Complete the six methods in the selected file and add the route-reconstruction helper:

   | Language | Algorithm file |
   |---|---|
   | Java | [Java/source/Algorithms.java](Java/source/Algorithms.java) |
   | Python | [Python/source/algorithms.py](Python/source/algorithms.py) |
   | C++ | [Cpp/source/algorithms.hpp](Cpp/source/algorithms.hpp) |
   | C# | [CSharp/source/Algorithms.cs](CSharp/source/Algorithms.cs) |

4. Run **Actions → Public checks** in your copy. The adapters already read JSON and dispatch requests; the algorithm bodies contain intentional TODOs. Setup can pass before the algorithms are implemented. Algorithm checks will fail until the implementations are correct.
5. Add your own tests and results under [tests/student/](tests/student/README.md). Complete [ANALYSIS.md](ANALYSIS.md) in English, including helper calls and your Kruskal–Prim measurements. Public checks do not automatically run arbitrary student test files.
6. Push your completed work and submit the exact commit URL in Moodle Online text, then click **Submit assignment**. The practical rubric is in the assignment; public checks provide feedback, not the final grade.

## Local checks

Install Python 3.12+ for the checking tools and the runtime for your chosen language: JDK 17+, Python 3.12+, a C++17 compiler (`g++`), or the .NET 8 SDK.

Run from the repository root:

```sh
python tools/run.py --setup
python tools/run.py --check
python tools/run.py --input examples/dsu.jsonl
```

The last command runs a supplied example; other inputs are under [examples/](examples/). To run your own file, use `python tools/run.py --input tests/student/my_cases.jsonl`. The launcher prints actual results; compare them with independently justified expected results. Compilation has a 60-second timeout; each checked or custom input batch has a 20-second timeout. Use `--language Python` (or another supported value) to override `submission.json` for one run. On Windows, `py -3` can replace `python`.

The 36 public cases include input/output, algorithm examples, boundary cases and deep chains of 2,000 vertices. The checker also repeats the complete batch twice in one process. It accepts different valid forests, component labels, topological orders and tied shortest routes. Test the full published limits yourself as well.

In this instructor template, Actions checks the testing tools and launch of all four languages. In student copies, it also runs the algorithm checks. A green run on the unchanged instructor template confirms the framework only.

## Начало работы

1. Создайте свой репозиторий через **Use this template → Create a new repository**. Предоставьте преподавателю доступ, если он приватный.
2. Выберите один язык в `submission.json`: `Java`, `Python`, `Cpp` или `CSharp`.
3. Реализуйте шесть методов в соответствующем файле из таблицы и добавьте функцию восстановления маршрута. Ввод/вывод JSON уже подготовлен.
4. Запустите **Actions → Public checks** в своей копии или используйте локальные команды выше. До реализации TODO ошибки алгоритмических проверок ожидаемы.
5. Добавьте собственные тесты с ожидаемыми и фактическими результатами в `tests/student/`. Заполните `ANALYSIS.md` на английском: объясните алгоритмы, покажите вызовы восстановления пути и измерения Краскала–Прима. Свои JSONL-файлы запускайте явно через `--input`.
6. Сдайте URL конкретного коммита в Moodle Online text и нажмите **Submit assignment**. Сроки и продления определяются Moodle. Полное условие и правила оценивания приведены в [русской версии задания](ASSIGNMENT_RU.md).
