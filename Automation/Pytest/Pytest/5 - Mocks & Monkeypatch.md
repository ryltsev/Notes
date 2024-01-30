## Monkeypatching functions

```

# contents of test_module.py with source code and the test

from pathlib import Path

def getssh():
    """Simple function to return expanded homedir ssh path."""
    return Path.home() / ".ssh"

def test_getssh(monkeypatch):
    # mocked return function to replace Path.home
    # always return '/abc'

    def mockreturn():
        return Path("/abc")
    # Application of the monkeypatch to replace Path.home
    # with the behavior of mockreturn defined above.
    monkeypatch.setattr(Path, "home", mockreturn)
    # Calling getssh() will use mockreturn in place of Path.home
    # for this test with the monkeypatch.
    x = getssh()
    assert x == Path("/abc/.ssh")
```

## Monkeypatching returned objects: building mock classes

```

# contents of app.py, a simple API retrieval example

import requests

def get_json(url):
    """Takes a URL, and returns the JSON."""
    r = requests.get(url)
    return r.json()
```

We need to mock `r`, the returned response object

```

# contents of test_app.py, a simple test for our API retrieval

# import requests for the purposes of monkeypatching

import requests

# our app.py that includes the get_json() function

# this is the previous code block example

import app

# custom class to be the mock return value

# will override the requests.Response returned from requests.get

class MockResponse:

# mock json() method always returns a specific testing dictionary

    @staticmethod
    def json():
        return {"mock_key": "mock_response"}

def test_get_json(monkeypatch):

    # Any arguments may be passed and mock_get() will always return our
    # mocked object, which only has the .json() method.
    def mock_get(*args, **kwargs):
        return MockResponse()

    # apply the monkeypatch for requests.get to mock_get
    monkeypatch.setattr(requests, "get", mock_get)

    # app.get_json, which contains requests.get, uses the monkeypatch
    result = app.get_json("https://fakeurl")
    assert result["mock_key"] == "mock_response"
```

This mock can be shared across tests using a `fixture`:

```

# contents of test_app.py, a simple test for our API retrieval

import pytest
import requests

# app.py that includes the get_json() function

import app

# custom class to be the mock return value of requests.get()

class MockResponse:
    @staticmethod
    def json():
        return {"mock_key": "mock_response"}

# monkeypatched requests.get moved to a fixture

@pytest.fixture
def mock_response(monkeypatch):
    """Requests.get() mocked to return {'mock_key':'mock_response'}."""
    def mock_get(*args, **kwargs):
        return MockResponse()
    monkeypatch.setattr(requests, "get", mock_get)

# notice our test uses the custom fixture instead of monkeypatch directly

def test_get_json(mock_response):
    result = app.get_json("https://fakeurl")
    assert result["mock_key"] == "mock_response"
```

## Global patch example: If you want to prevent the “requests” library from performing http requests in all your tests, you can do:

```

# contents of conftest.py

import pytest

@pytest.fixture(autouse=True)
def no_requests(monkeypatch):
    """Remove requests.sessions.Session.request for all tests."""
    monkeypatch.delattr("requests.sessions.Session.request")
```

This autouse fixture will be executed for each test function and it will delete the method `request.session.Session.request` so that any attempts within tests to create http requests will fail.