# Assignment 3: Graph Algorithms for a Campus Festival

[English assignment](ASSIGNMENT_EN.md) · [Условие на русском](ASSIGNMENT_RU.md)

Implement DSU, Kruskal, Prim, Kosaraju SCC, DFS topological ordering and DAG shortest paths in **one** language. The linked specifications contain all tasks, limits and assessment rules. The deadline is announced in Moodle.

## Getting started

1. Select **Use this template → Create a new repository**.
2. Set `language` in [submission.json](submission.json) to `Java`, `Python`, `Cpp` or `CSharp`. Java is selected by default.
3. Complete the six method bodies in the selected file and add your route helper:

   | Language | Algorithm file |
   |---|---|
   | Java | [Java/source/Algorithms.java](Java/source/Algorithms.java) |
   | Python | [Python/source/algorithms.py](Python/source/algorithms.py) |
   | C++ | [Cpp/source/algorithms.hpp](Cpp/source/algorithms.hpp) |
   | C# | [CSharp/source/Algorithms.cs](CSharp/source/Algorithms.cs) |

4. Push your code. **Actions → Public checks** runs automatically in your copy. TODO bodies fail algorithm checks until implemented.
5. Add your tests and results under [tests/student/](tests/student/README.md), complete English [ANALYSIS.md](ANALYSIS.md), and submit the exact commit URL in Moodle. Click **Submit assignment**. Give the instructor access to a private repository.

[Java starter notes / Пояснения к Java](Java/README.md): input helpers, result format and the role of supplied files.

## Local checks

Install **Python 3.12+** for the checking tools and your selected runtime: **JDK 17+** (Java 17-compatible code), Python 3.12+, `g++` with C++17 support, or the **.NET 8 SDK**. Python runs the checker even when your solution is Java; GitHub Actions installs the tools for you.

From the repository root:

```sh
python tools/run.py --setup
python tools/run.py --check
python tools/run.py --input examples/dsu.jsonl
```

`--setup` checks compilation and launch; `--check` runs 36 public cases (ping and 35 algorithm cases) and repeats the full batch twice in one process. `--input` runs a supplied [example](examples/) or your own JSON Lines file and prints actual results for you to check. It does not compare custom answers automatically or discover files in `tests/student/`.

Compilation is limited to 60 seconds, each input batch to 20 seconds. `--language Python` overrides the manifest for one run. On Windows, `py -3` can replace `python`.

In the instructor template, Actions checks tools and launch of all four languages. In student copies, it also checks algorithms. Public checks report pass/fail; they do not calculate grades or transfer them to Moodle. Full-scale correctness still needs your own testing.

## Краткая инструкция

Создайте свою копию через **Use this template**, выберите язык в `submission.json` и заполните шесть методов в файле из таблицы. Для Java уже выбрано правильное значение; ввод/вывод JSON подготовлен. После отправки кода **Actions → Public checks** запустится автоматически. До реализации TODO ошибки тестов ожидаемы.

Для локальной проверки Java нужны JDK 17+ и Python 3.12+: Python запускает проверяющий инструмент, решение выполняется на Java. Используйте команды выше. `--check` проверяет публичные случаи; `--input` только запускает выбранный вход и выводит фактические результаты. Свои файлы из `tests/student/` запускайте явно.

Добавьте собственные тесты и результаты, заполните `ANALYSIS.md` на английском и сдайте ссылку на конкретный коммит в Moodle с нажатием **Submit assignment**. Публичная проверка не выставляет баллы. Полные требования приведены в [русском условии](ASSIGNMENT_RU.md).
