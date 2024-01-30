https://requests.readthedocs.io/en/latest/
Basic request

```
r = requests.get('https://api.github.com/events')
r = requests.post('https://httpbin.org/post', data={'key': 'value'})
r = requests.put('https://httpbin.org/put', data={'key': 'value'})
r = requests.delete('https://httpbin.org/delete')
r = requests.head('https://httpbin.org/get')
r = requests.options('https://httpbin.org/get')
```

Usages OPTIONS

We can take advantage of the Requests OPTIONS verb to see what kinds of HTTP methods are supported on the url we just used

```
>>> verbs = requests.options(r.url)
>>> verbs.status_code
500
```

Uh, what? That’s unhelpful! Turns out GitHub, like many API providers, don’t actually implement the OPTIONS method

However, they should return the allowed methods in the headers, e.g.

```
>>> verbs = requests.options('http://a-good-website.com/api/cats')
>>> print(verbs.headers['allow'])
GET,HEAD,POST,OPTIONS
```

Headers
All header values must be a string, bytestring, or unicode

```
url = 'https://api.github.com/some/endpoint'
headers = {'user-agent': 'my-app/0.0.1'}

>>> r = requests.get(url, headers=headers)
```

If we want to access the response's headers
`>>> r.headers`
If we want to access the request's headers
`r.request.headers`
Access the headers using any capitalization we want

```
>>> r.headers['Content-Type']
'application/json'
>>> r.headers.get('content-type')
'application/json
```

Passing query/string parameters in URL

```
payload = {'key1': 'value1', 'key2': 'value2'}
>>> r = requests.get('https://httpbin.org/get', params=payload)

You can also pass a list, tuple, dict of items as a value:

payload = {'key1': 'value1', 'key2': 'value2'}
>>> r = requests.get('https://httpbin.org/get', params=payload)

payload_tuples = [('key1', 'value1'), ('key1', 'value2')]
>>> r1 = requests.post('https://httpbin.org/post', data=payload_tuples)

payload_dict = {'key1': ['value1', 'value2']}
>>> r2 = requests.post('https://httpbin.org/post', data=payload_dict)
print(r1.text)
```

Read the content of response

```
>>> r = requests.get('https://api.github.com/events')

>>> r.text
```

Decode JSON

```
>>> r = requests.get('https://api.github.com/events')

>>> r.json()
```

```
>>> commit_data = r.json()
>>> print(commit_data.keys())
['committer', 'author', 'url', 'tree', 'sha', 'parents', 'message']

>>> print(commit_data['committer'])

{'date': '2012-05-10T11:10:50-07:00', 'email': 'me@kennethreitz.com', 'name': 'Kenneth Reitz'}

>>> print(commit_data['message'])
makin' history
```

Raw content
stream=True

```
>>> r = requests.get('https://api.github.com/events', stream=True)
>>> r.raw
```

Status code

```
>>> r = requests.get('https://httpbin.org/get')
>>> r.status_code
```

## Cookies

If a response contains some Cookies, you can quickly access them:

```
url = 'http://example.com/some/cookie/setting/url'
>>> r = requests.get(url)

>>> r.cookies['example_cookie_name']
```

To send your own cookies to the server, you can use the cookies parameter

```
url = 'https://httpbin.org/cookies'
cookies = dict(cookies_are='working')

>>> r = requests.get(url, cookies=cookies)
```

Cookie jars can also be passed in to requests

```
jar = requests.cookies.RequestsCookieJar()
>>> jar.set('tasty_cookie', 'yum', domain='httpbin.org', path='/cookies')
>>> jar.set('gross_cookie', 'blech', domain='httpbin.org', path='/elsewhere')
>>> url = 'https://httpbin.org/cookies'
>>> r = requests.get(url, cookies=jar)
```

## Redirection

By default Requests will perform location redirection for all verbs
Disable redirection
`r = requests.get('http://github.com/', allow_redirects=False)`

## Timeouts

`requests.get('https://github.com/', timeout=0.001)`

## Session Objects

Let’s persist some cookies across requests:

```
s = requests.Session()

s.get('https://httpbin.org/cookies/set/sessioncookie/123456789')
r = s.get('https://httpbin.org/cookies')

print(r.text)

# '{"cookies": {"sessioncookie": "123456789"}}'

```

Sessions can also be used to provide default data to the request methods.

```
s = requests.Session()
s.auth = ('user', 'pass')
s.headers.update({'x-test': 'true'})

# both 'x-test' and 'x-test2' are sent

s.get('https://httpbin.org/headers', headers={'x-test2': 'true'})
```

## Sessions can also be used as context managers:

```
with requests.Session() as s:
    s.get('https://httpbin.org/cookies/set/sessioncookie/123456789')
```

## Prepared Requests

Тянется из session object

# Authentication

## Basic Authentication - Basic Auth

```
from requests.auth import HTTPBasicAuth
>>> basic = HTTPBasicAuth('user', 'pass')
>>> requests.get('https://httpbin.org/basic-auth/user/pass', auth=basic)

OR shorthand for using it

>>> requests.get('https://httpbin.org/basic-auth/user/pass', auth=('user', 'pass'))

```

## Digest Authentication

```
from requests.auth import HTTPDigestAuth
>>> url = 'https://httpbin.org/digest-auth/auth/user/pass'
>>> requests.get(url, auth=HTTPDigestAuth('user', 'pass'))
```

## OAuth 1 Authentication

```
from requests_oauthlib import OAuth1

url = 'https://api.twitter.com/1.1/account/verify_credentials.json'
>>> auth = OAuth1('YOUR_APP_KEY', 'YOUR_APP_SECRET',
...               'USER_OAUTH_TOKEN', 'USER_OAUTH_TOKEN_SECRET')

>>> requests.get(url, auth=auth)
```

## Custom Authentication

Let’s pretend that we have a web service that will only respond if the `X-Pizza` header is set to a password value. Unlikely, but just go with it.

```
from requests.auth import AuthBase

class PizzaAuth(AuthBase):
    """Attaches HTTP Pizza Authentication to the given Request object."""
    def __init__(self, username):
        # setup any auth-related data here
        self.username = username

    def __call__(self, r):
        # modify and return the request
        r.headers['X-Pizza'] = self.username
        return r
Then, we can make a request using our Pizza Auth:
>>> requests.get('http://pizzabin.org/admin', auth=PizzaAuth('kenneth'))
```

## Proxies

If you need to use a proxy, you can configure individual requests with the `proxies` argument to any request method:

```
proxies = {
  'http': 'http://10.10.1.10:3128',
  'https': 'http://10.10.1.10:1080',
}

requests.get('http://example.org', proxies=proxies)
```

Alternatively you can configure it once for an entire `**Session**`

```
proxies = {
  'http': 'http://10.10.1.10:3128',
  'https': 'http://10.10.1.10:1080',
}
session = requests.Session()
session.proxies.update(proxies)

session.get('http://example.org')
```

To use HTTP Basic Auth with your proxy, use the http://user:password@host/ syntax in any of the above configuration entries:

```
export HTTPS_PROXY="http://user:pass@10.10.1.10:1080"

>>> proxies = {'http': 'http://user:pass@10.10.1.10:3128/'}
```