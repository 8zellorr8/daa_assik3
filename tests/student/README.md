# Your own tests

Save your own JSON Lines inputs, expected results, actual outputs and short explanations in this directory. Explain why the expected answers are correct and which mistake each test detects.

From the repository root, run an input file that you created:

```sh
python tools/run.py --input tests/student/my_cases.jsonl
```

Each line must contain one complete request. This command runs the program and prints its output; it does not compare your results automatically. For non-unique answers, check validity and optimality rather than copying one edge list or label numbering.

Call the route-reconstruction helper directly from your own test code. It is not a separate JSON task. Record results for a reachable target, the source, an unreachable target and tied/parallel edges. Refer to the assignment for other required cases and large-input limits.

Сохраните здесь собственные входы JSON Lines, ожидаемые и фактические результаты и объяснения тестов. Команда выше запускает ваш вход, но не сравнивает ответы автоматически. Функцию восстановления пути вызывайте непосредственно из своего тестового кода. Публичные проверки не запускают произвольные файлы из этой папки.
