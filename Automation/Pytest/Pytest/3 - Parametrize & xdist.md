Parametrize

```

@pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+4", 6), ("6*9", 42)])

def test_eval(test_input, expected):
    assert eval(test_input) == expected
```

- parametrize marker on a class or a module

```
@pytest.mark.parametrize("n,expected", [(1, 2), (3, 4)])
class TestClass:
    def test_simple_case(self, n, expected):
        assert n + 1 == expected
    def test_weird_simple_case(self, n, expected):
        assert (n * 1) + 1 == expected
```

- To parametrize all tests in a module, you can assign to the `**pytestmark**` global variable:

```
pytestmark = pytest.mark.parametrize("n,expected", [(1, 2), (3, 4)])
class TestClass:
    def test_simple_case(self, n, expected):
        assert n + 1 == expected
    def test_weird_simple_case(self, n, expected):
        assert (n * 1) + 1 == expected
```

Xdist

- to install

`pip install pytest-xdist`

- xdist will use as many processes as your computer has CPU cores

`pytest -n auto`

- Pass a number of processes

`-n 8`

- The test distribution algorithm is configured with the `--dist` command-line option:

- `--dist load`  **(default)**: Sends pending tests to any worker that is available, without any guaranteed order. Scheduling can be fine-tuned with the –maxschedchunk option, see output of pytest –help.
- `--dist loadscope`: Tests are grouped by **module** for *test functions* and by **class** for *test methods*. Groups are distributed to available workers as whole units. This guarantees that all tests in a group run in the same process. This can be useful if you have expensive module-level or class-level fixtures. Grouping by class takes priority over grouping by module.
- `--dist loadfile`: Tests are grouped by their containing file. Groups are distributed to available workers as whole units. This guarantees that all tests in a file run in the same worker.
- `--dist loadgroup`: Tests are grouped by the `xdist_group` mark. Groups are distributed to available workers as whole units. This guarantees that all tests with same `xdist_group` name run in the same worker.

```
@pytest.mark.xdist_group(name="group1")
def test1():
    pass
class TestA:
    @pytest.mark.xdist_group("group1")
    def test2():
        pass
```

This will make sure `test1` and `TestA::test2` will run in the same worker. Tests without the `xdist_group` mark are distributed normally as in the `--dist=load` mode.

- `--dist worksteal`: Initially, tests are distributed evenly among all available workers. When a worker completes most of its assigned tests and doesn’t have enough tests to continue (currently, every worker needs at least two tests in its queue), an attempt is made to reassign (“steal”) a portion of tests from some other worker’s queue. The results should be similar to the `load` method, but `worksteal` should handle tests with significantly differing duration better, and, at the same time, it should provide similar or better reuse of fixtures.
- `--dist no`: The normal pytest execution mode, runs one test at a time (no distribution at all).