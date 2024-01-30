РФ дока - https://pytest-docs-ru.readthedocs.io/ru/latest/
Англ дока - https://docs.pytest.org/en/7.1.x/index.html

Запуск тестов
Run tests by marker expressions - pytest -m evo
Run tests in a module - pytest test_mod.py
Run tests in a directory - pytest testing/

Run tests by keyword expressions - pytest -k "MyClass and not method" (This will run tests which contain names that match the given string expression (case-insensitive), which can include Python operators that use filenames, class names and function names as variables. The example above will run `TestMyClass.test_something` but not `TestMyClass.test_method_simple`.)

To run a specific test within a module - pytest test_mod.py::TestClass::test_method

Run tests from packages - pytest --pyargs pkg.testing
Calling pytest through python  ` -m pytest`
Calling pytest from Python code - retcode = pytest.main()

##

## Registering marks

Note that everything past the `:` after the mark name is an optional description.

You can register custom marks in your `pytest.ini` file like this:

```
[pytest]
markers =
    slow: marks tests as slow (deselect with '-m "not slow"')
    serial
```

or in your `pyproject.toml` file like this:

```
[tool.pytest.ini_options]
markers = [
    "slow: marks tests as slow (deselect with '-m \"not slow\"')",
    "serial",
```

Alternatively, you can register new markers programmatically in a [pytest_configure](https://docs.pytest.org/en/7.1.x/reference/reference.html#initialization-hooks) hook:

```
def pytest_configure(config):
    config.addinivalue_line(
        "markers", "env(name): mark test to run only on named environment"
    )
```

Создание тестовых классов (наборов тестов)

```
class TestClass:
    def test_one(self):
        x = "this"
        assert "h" in x
    def test_two(self):
        x = "hello"
        assert hasattr(x, "check")
```

##

## Изменение вывода сообщений трассировки

```
pytest --showlocals # показывать локальные переменные в сообщениях

pytest -l           # показывать локальные переменные в сообщениях (краткий вариант)

pytest --tb=auto    # (по умолчанию) "расширенный" вывод для первого и
                    # последнего сообщений, и "короткий" для остальных
pytest --tb=long    # исчерпывающий, подробный формат сообщений
pytest --tb=short   # сокращенный формат сообщений
pytest --tb=line    # только одна строка на падение
pytest --tb=native  # стандартный формат библиотеки Python
pytest --tb=no      # никаких сообщений
```