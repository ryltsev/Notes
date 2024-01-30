- Autouse. Авто-использование фикстур - @pytest.fixture(autouse=True)
- Пред- и постусловия в фикстурах (yield)

```
@pytest.fixture
def sending_user(mail_admin):
    user = mail_admin.create_user()
    yield user
    mail_admin.delete_user(user)
```

- Область видимости фикстур:

```

- function: the default scope, the fixture is destroyed at the end of the test.
- class: the fixture is destroyed during teardown of the last test in the class.
- module: the fixture is destroyed during teardown of the last test in the module.
- package: the fixture is destroyed during teardown of the last test in the package.
- session: the fixture is destroyed at the end of the test session.

```

-

### Fixtures can **request** other fixtures

```
@pytest.fixture
def first_entry():
    return "a"
@pytest.fixture
def order(first_entry):
    return [first_entry]
```

-

### A test/fixture can **request** more than one fixture at a time

```
@pytest.fixture
def first_entry():
    return "a"

@pytest.fixture
def second_entry():
    return 2

@pytest.fixture
def order(first_entry, second_entry):
    return [first_entry, second_entry]
```

-

### Fixtures can be **requested** more than once per test (return values are cached)

```
@pytest.fixture
def first_entry():
    return "a"

@pytest.fixture
def order():
    return []

@pytest.fixture
def append_first(order, first_entry):
    return order.append(first_entry)

def test_string_only(append_first, order, first_entry):
    # Assert
    assert order == [first_entry]
```

-

## Factories as fixtures

```
@pytest.fixture
def make_customer_record():
    def _make_customer_record(name):
        return {"name": name, "orders": []}
    return _make_customer_record

def test_customer_records(make_customer_record):
    customer_1 = make_customer_record("Lisa")
    customer_2 = make_customer_record("Mike")
    customer_3 = make_customer_record("Meredith")
```

If the data created by the factory requires managing, the fixture can take care of that:

```
@pytest.fixture
def make_customer_record():
    created_records = []
    def _make_customer_record(name):
        record = models.Customer(name=name, orders=[])
        created_records.append(record)
        return record
    yield _make_customer_record
    for record in created_records:
        record.destroy()
def test_customer_records(make_customer_record):
    customer_1 = make_customer_record("Lisa")
    customer_2 = make_customer_record("Mike")
    customer_3 = make_customer_record("Meredith")
```

-

### Override a fixture

### on a folder

```
tests/
    conftest.py
        # content of tests/conftest.py
        import pytest
        @pytest.fixture
        def username():
            return 'username'
    test_something.py
        # content of tests/test_something.py
        def test_username(username):
            assert username == 'username'
    subfolder/
        conftest.py
            # content of tests/subfolder/conftest.py
            import pytest
            @pytest.fixture
            def username(username):
                return 'overridden-' + username
        test_something_else.py
            # content of tests/subfolder/test_something_else.py
            def test_username(username):
                assert username == 'overridden-username'
```

###             on a test module level

```
tests/
    conftest.py
        # content of tests/conftest.py
        import pytest
        @pytest.fixture
        def username():
            return 'username'
    test_something.py
        # content of tests/test_something.py
        import pytest
        @pytest.fixture
        def username(username):
            return 'overridden-' + username
        def test_username(username):
            assert username == 'overridden-username'
    test_something_else.py
        # content of tests/test_something_else.py
        import pytest
        @pytest.fixture
        def username(username):
            return 'overridden-else-' + username
        def test_username(username):
            assert username == 'overridden-else-username'
```