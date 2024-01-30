Assertions

```
from colorama import Fore

class AssertResponse:

    def __init__(self, status_code: str, response_body: dict, expected_result: dict = None):

        self.response_body = response_body
        self.expected_result = expected_result
        self.status_code = status_code

    def assertions(self, key_data: str = "data"):
        """
        @key_data: main key response body {key_data: {...}}
        """
        if self.response_body.get(key_data, None):
            for key, expected in self.expected_result.items():
                actual = self.response_body[key_data].get(key, None)
                assert actual == expected, (
                    f"\n{Fore.BLUE}Key: [{key}] "
                    f"\n{Fore.GREEN}Expected : {expected} "
                    f"\n{Fore.CYAN}Actual : {actual}"
                )
        self.assert_status_code()

    @staticmethod

    def check_array(response_array, expected_array, index_on_array: int = None):

        """
        Метод позволяет перебрать все элементы массива и проверить их состав.
        :param response_array: массив из ответа {"data": {"answers": [...]}}
        :param expected_array: ожидаемый состав массива

        :param index_on_array: индекс конкретного массива который нужно проверить,

        если не указан проверяет все элементы массива
        """
        for index, array in enumerate(expected_array, start=0):
            for key, value in array.items():
                if index_on_array:
                    index = index_on_array
                actual = response_array[index].get(key, None)
                assert actual == value, (
                    f"\n{Fore.BLUE}Key: [{key}] "
                    f"\n{Fore.GREEN}Expected : {value} "
                    f"\n{Fore.CYAN}Actual : {actual}"
                )

    def assert_status_code(self):
        response_status_code = self.response_body.get('status')
        assert response_status_code == self.status_code, (
            f"\n{Fore.GREEN}Expected status: {self.status_code} "
            f"\n{Fore.CYAN}Actual status: {response_status_code}"
        )
```

Client

```
from requests import request

from tests.API.Helpers.log_info import LogInfoApi
from tests.API.Helpers.decode import Decode

class Client:

    def base_request(self, status_code: int, method, url, **kwargs):
        response = request(method, url, **kwargs)

        LogInfoApi.log_info(log=response)

        assert response.status_code == status_code, (
            f"\nExpected code: {status_code}\n"
            f"Actual code: {response.status_code}"
        )

        return Decode.json(response)

    @staticmethod

    def get(url, *, params=None, data=None, headers=None, cookies=None, files=None,

            auth=None, timeout=None, allow_redirects=True, proxies=None,

            hooks=None, stream=None, verify=None, cert=None, json=None, expected_status_code: int = 200):

        return Client().base_request(expected_status_code, 'get', url,

                                     params=params, data=data, headers=headers, cookies=cookies, files=files,

                                     auth=auth, timeout=timeout, allow_redirects=allow_redirects, proxies=proxies,

                                     hooks=hooks, stream=stream, verify=verify, cert=cert, json=json)

    @staticmethod

    def post(url, *, params=None, data=None, headers=None, cookies=None, files=None,

             auth=None, timeout=None, allow_redirects=True, proxies=None,

             hooks=None, stream=None, verify=None, cert=None, json=None, expected_status_code: int = 200):

        return Client().base_request(expected_status_code, 'post', url,

                                     params=params, data=data, headers=headers, cookies=cookies, files=files,

                                     auth=auth, timeout=timeout, allow_redirects=allow_redirects, proxies=proxies,

                                     hooks=hooks, stream=stream, verify=verify, cert=cert, json=json)

    @staticmethod

    def put(url, *, params=None, data=None, headers=None, cookies=None, files=None,

            auth=None, timeout=None, allow_redirects=True, proxies=None,

            hooks=None, stream=None, verify=None, cert=None, json=None, expected_status_code: int = 200):

        return Client().base_request(expected_status_code, 'put', url,

                                     params=params, data=data, headers=headers, cookies=cookies, files=files,

                                     auth=auth, timeout=timeout, allow_redirects=allow_redirects, proxies=proxies,

                                     hooks=hooks, stream=stream, verify=verify, cert=cert, json=json)

    @staticmethod

    def delete(url, *, params=None, data=None, headers=None, cookies=None, files=None,

               auth=None, timeout=None, allow_redirects=True, proxies=None,

               hooks=None, stream=None, verify=None, cert=None, json=None, expected_status_code: int = 200):

        return Client().base_request(expected_status_code, 'delete', url,

                                     params=params, data=data, headers=headers, cookies=cookies, files=files,

                                     auth=auth, timeout=timeout, allow_redirects=allow_redirects, proxies=proxies,

                                     hooks=hooks, stream=stream, verify=verify, cert=cert, json=json)

    @staticmethod

    def patch(url, *, params=None, data=None, headers=None, cookies=None, files=None,

              auth=None, timeout=None, allow_redirects=True, proxies=None,

              hooks=None, stream=None, verify=None, cert=None, json=None, expected_status_code: int = 200):

        return Client().base_request(expected_status_code, 'patch', url,

                                     params=params, data=data, headers=headers, cookies=cookies, files=files,

                                     auth=auth, timeout=timeout, allow_redirects=allow_redirects, proxies=proxies,

                                     hooks=hooks, stream=stream, verify=verify, cert=cert, json=json)

```

Decode

```
from json.decoder import JSONDecodeError

from requests import Response

class Decode:

    @staticmethod
    def json(response: Response):
        try:
            return response.json()
        except JSONDecodeError:
            pass
```

log_info

```
import logging

class LogInfoApi:
    @staticmethod
    def log_info(log):
        logging.info(
            f"\nRequest url: {log.request.url} "
            f"\nRequest body: {log.request.body} "
            f"\nMethod: {log.request.method}"
            f"\nResponse status: {log.status_code} "
            f"\nResponse body: {log.text}\n\n"
            "<-----------CURL----------->"
            f"\ncurl -i -X {log.request.method} {log.request.url} "

            f"{LogInfoApi.get_parse_headers(log.request.headers)} -d '{log.request.body}' "

        )

    @staticmethod
    def get_parse_headers(headers):
        if headers:

            return ' '.join([f"-H '{key}:{value}'" for key, value in headers.items()])

        return None
```

parser

```
import os
import configparser

class Parser:
    config = configparser.ConfigParser()

    file_db = os.path.join(os.path.dirname(__file__), "../", "Config", "db.ini")

    config.read(file_db)

    file_local = os.path.join(os.path.dirname(__file__), "../", "Config", "local.ini")

    config.read(file_local)
    file_black = os.path.join(os.path.dirname(__file__), "black.sh", "&")
```

time

```
import datetime as dt
from datetime import datetime
from datetimerange import DateTimeRange
from bson.timestamp import Timestamp

class TypeTime:
    current_datetime = datetime.today()
    current_year_srt = current_datetime.strftime("%Y")
    current_year_int = datetime.today().year
    current_date = datetime.utcnow().strftime("%Y.%m.%d")
    range_date_one = DateTimeRange(
        current_year_srt + "-09-01", current_year_srt + "-12-31"
    )
    range_date_two = DateTimeRange(
        current_year_srt + "-01-01", current_year_srt + "-08-31"
    )
    tomorrow = current_datetime + dt.timedelta(days=1)
    yesterday = current_datetime - dt.timedelta(days=1)

    @staticmethod
    def timestamp_today():
        timestamp = Timestamp(int(dt.datetime.today().timestamp()), 0)
        return timestamp

    @staticmethod
    def date_today():
        date_today = TypeTime.current_datetime
        return date_today

    @staticmethod
    def date_tomorrow():
        date_tomorrow = TypeTime.tomorrow
        return date_tomorrow

    @staticmethod
    def date_yesterday():
        date_yesterday = TypeTime.yesterday
        return date_yesterday

    @staticmethod
    def begin_29_days():
        date_start_29_days = TypeTime.start_29_days
        return date_start_29_days

    @staticmethod
    def ending_29_days():
        date_end_29_days = TypeTime.end_29_days
        return date_end_29_days

    # возвращает id текущего УГ
    @staticmethod
    def current__study_year_id():
        if TypeTime.current_date in TypeTime.range_date_one:
            return int(TypeTime.current_year_int)
        if TypeTime.current_date in TypeTime.range_date_two:
            return int(TypeTime.current_year_int - 1)

    # возвращает id следующего УГ
    @staticmethod
    def next__study_year_id():
        if TypeTime.current_date in TypeTime.range_date_one:
            return int(TypeTime.current_year_int + 1)
        if TypeTime.current_date in TypeTime.range_date_two:
            return int(TypeTime.current_year_int)
```

```
class LogInfoApi:
    @staticmethod
    def log_info(log):
        logging.info(
            f"\nRequest url: {log.request.url} "
            f"\nRequest body: {log.request.body} "
            f"\nMethod: {log.request.method}"
            f"\nResponse status: {log.status_code} "
            f"\nResponse body: {log.text}\n\n"
            "<-----------CURL----------->"
            f"\ncurl -i -X {log.request.method} {log.request.url} "

            f"{LogInfoApi.get_parse_headers(log.request.headers)} -d '{log.request.body}' "

        )
```