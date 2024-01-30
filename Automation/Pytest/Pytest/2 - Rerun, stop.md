Rerun

- Перезапуск упавших тестов
- `--lf`, `--last-failed` - to only re-run the failures.
- `--ff`, `--failed-first` - to run the failures first and then the rest of the tests. All tests will be run but the first previous failures will be executed first
- `--nf`, `--new-first` - to run new tests first followed by the rest of the tests, in both cases tests are also sorted by the file modified time, with more recent files coming first.

```
pytest --lf
pytest --ff
```

- Перезапуск упавших тестов сразу после их падения

`pip install pytest-rerunfailures`

- To re-run all test failures, use the --reruns command line option with the maximum number of times you’d like the tests to run:

`pytest --reruns 5`
-

### Перезапуск конкретных кейсов

To mark individual tests as flaky, and have them automatically re-run when they fail, add the flaky mark with the maximum number of times you’d like the test to run:

```
@pytest.mark.flaky(reruns=5)
def test_example():
    import random
    assert random.choice([True, False])
```

Stop

- Остановка тестового прогона после падения N тестов

```
pytest -x           # остановка после первого упавшего теста
pytest --maxfail=2  # остановка после первых двух упавших тестов
```

- Чтобы получить список 10 самых медленных тестов, выполните:

`pytest --durations=10`