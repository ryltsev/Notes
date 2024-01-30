## datetime -  https://docs.python.org/3/library/datetime.html

не оф. на русском -  [*https://pythonru.com/primery/kak-ispolzovat-modul-datetime-v-python*](https://pythonru.com/primery/kak-ispolzovat-modul-datetime-v-python)

## Если понадобится либа time от питона

time -  https://docs.python.org/3/library/time.html
не оф. на русском -  https://pythonru.com/osnovy/modul-time-v-python

##

## **Важные нюансы**

Вот о чем важно помнить при работе с `datetime` в Python:

- Рекомендуется всегда работать с UTC. Это позволяет не думать о часовых поясах, что часто приводит к ошибкам из-за разницы во времени в разных регионах.
- Дату и время стоит конвертировать в локальную только при выводе пользователю.

##

## **Текущие дата и время**

```
import datetime

dt_now = datetime.datetime.now()

Текущее время:
current_time = dt_now.time()
```

## **Текущую дату**

```
from datetime import date

current_date = date.today()
print(current_date)
```

## **Создать объекты даты и времени**

Объект времени - datetime.time(hour, minutes, seconds)

```
import datetime

timeobj= datetime.time(8,48,45)
```

Объект даты - datetime.datetime(year,month,day))

```
import datetime

date_obj = datetime.datetime(2020,10,17)
```

## **Timedelta**

`timedelta` представляет длительность (даты или времени)

```

td_object =timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)

td_object
datetime.timedelta(0)
```

Все аргументы опциональные и их значения по умолчанию равно 0. Они могут быть целыми или числами с плавающей точкой, как положительными, так и отрицательными.

Вычислить разницу для двух дат:

```
from datetime import date

first_date = date(2020, 10, 2)
second_date = date(2020, 10, 30)
delta = second_date - first_date
```

С помощью `timedelta` нельзя выполнять манипуляции над объектами `time`

```
from datetime import datetime, timedelta

current_datetime = datetime.now()
current_time = current_datetime.time()
print("Текущее время:", current_time)
tm_after_1_hr = current_time + timedelta(hours=1)
Вернётся ошибка!!!
```

### Как получать прошлые и будущие даты с помощью timedelta

```
import datetime

current_date = datetime.datetime.today()
past_date = datetime.datetime.today() – datetime.timedelta(days=n)
future_date = datetime.datetime.today() + datetime.timedelta(days=n)
n — это целое число, представляющее количество дней:
```

Если нужно, например, получить дату за прошлые две недели, то достаточно вычесть 14 дней из текущей даты:

```
import datetime

past_date = datetime.datetime.today() - datetime.timedelta(days=14)
```

Значения даты и времени могут сравниваться

```
import datetime

now = datetime.time(9, 31, 0)
next_hour = datetime.time(10, 31, 0)
print('now < next_hour:', now < next_hour)
today = datetime.date.today()
next_week = datetime.date.today() + datetime.timedelta(days=7)
print('today > next_week:', today > next_week)
```

### Как работать с часовыми поясами

```
import datetime

dt_now = datetime.datetime.utcnow()
print(dt_now)
```

Библиотека для работы с часовыми поясами

```
import pytz

pytz.all_timezones
```

```
import pytz
import datetime

tz_berlin = pytz.timezone("Europe/Berlin")
dt_berlin =datetime.datetime.now(tz_berlin)
print(dt_berlin)
```

### Конвертация часовых поясов

```
import datetime
import pytz

timezone_berlin = '2019-06-29 17:08:00'
tz_ber_obj = datetime.datetime.strptime(timezone_berlin, '%Y-%m-%d %H:%M:%S')
timezone_newyork = pytz.timezone('America/New_York')
timezone_newyork_obj = timezone_newyork.localize(tz_ber_obj)
print(timezone_newyork_obj)
print(timezone_newyork_obj.tzinfo)
```

Всегда храните даты в UTC. Вот примеры:

```
import datetime
import pytz

time_now = datetime.datetime.now(pytz.utc)
print(time_now)
```

А уже при демонстрации даты пользователю стоит использовать метод `localize` с местными настройками:

```
import datetime
import pytz

now = datetime.datetime.today()
now_utc = pytz.utc.localize(now)
```

## **Как конвертировать строки в datetime**

`strptime()`  в Python — это метод из модуля datetime. Вот его синтаксис:
`dateobj = datetime.strptime(date_string, format)`

```
import datetime

date_string = "11/17/20"
date_obj = datetime.datetime.strptime(date_string, '%m/%d/%y')
print(date_obj)
2020-11-17 00:00:00
```

Предположим, что есть следующая строка с датой: «11/17/20 15:02:34», и ее нужно конвертировать в объект `datetime`

```
from datetime import datetime

datetime_string = "11/17/20 15:02:34"
datetime_obj = datetime.strptime(datetime_string, '%m/%d/%y %H:%M:%S')
print(datetime_obj)
2020-11-17 15:02:34
```

```
from datetime import datetime

# создадим даты как строки

ds1 = 'Friday, November 17, 2020'
ds2 = '11/17/20'
ds3 = '11-17-2020'

# Конвертируем строки в объекты datetime и сохраним

dt1 = datetime.strptime(ds1, '%A, %B %d, %Y')
dt2 = datetime.strptime(ds2, '%m/%d/%y')
dt3 = datetime.strptime(ds3, '%m-%d-%Y')

print(dt1)
print(dt2)
print(dt3)
2020-11-17 00:00:00
2020-11-17 00:00:00
2020-11-17 00:00:00
```

Если строка представлена в формате «Oct 17 2020 9:00PM», то ее можно конвертировать следующим образом:

```
date_string = 'Oct 17 2020 9:00PM'
date_object = datetime.strptime(date_string, '%b %d %Y %I:%M%p')
print(date_object)
2020-10-17 21:00:00
```

Функцию `strptime()` можно использовать для конвертации строки в объект даты:

```
from datetime import datetime

date_string = '10-17-2020'
date_object = datetime.strptime(date_string, '%m-%d-%Y').date()
print(type(date_object))
print(date_object)
```

## **Как конвертировать объект datetime в строку**

```
datetime_string = datetime_object.strftime(format_string)
time_string = datetime_object.strftime(format_string[,time_object])
```

Предположим, нужно конвертировать текущий объект `datetime` в строку. Сначала нужно получить представление объекта `datetime` и вызвать на нем метод `strftime()`

```
import datetime

current_date = datetime.datetime.now()
current_date_string = current_date.strftime('%m/%d/%y %H:%M:%S')
print(current_date_string)
11/14/20 17:15:03
```

### Как получить строковое представление даты и времени с помощью функции format()

Пример №1. Конвертация текущей временной метки в объекте `datetime` в строку в формате «DD-MMM-YYYY (HH:MM:SS:MICROS)»:

```
import datetime

dt_obj =datetime.datetime.now()
dt_string = dt_obj.strftime("%d-%b-%Y (%H:%M:%S.%f)")
print('Текущее время : ', dt_string)
Текущее время :  14-Nov-2020 (17:18:09.890960)
```

Пример №2. Конвертация текущей временной метки объекта `datetime` в строку в формате «HH:MM:SS.MICROS – MMM DD YYYY».

```
import datetime

dt_obj =datetime.datetime.now()
dt_string = dt_obj.strftime("%H:%M:%S.%f - %b %d %Y")
print('Текущее время : ', dt_string)
Текущее время :  17:20:28.841390 - Nov 14 2020
```

Результат будет в формате ISO 8601, то есть YYYY-MM-DDTHH:MM:SS.mmmmmm — формат по умолчанию, что позволяет получать строки в едином формате.

| Символ | Описание | Пример |
| --- | --- | --- |
| %a  | День недели, короткий вариант | Wed |
| %A  | Будний день, полный вариант | Wednesday |
| %w  | День недели числом 0-6, 0 — воскресенье | 3   |
| %d  | День месяца 01-31 | 31  |
| %b  | Название месяца, короткий вариант | Dec |
| %B  | Название месяца, полное название | December |
| %m  | Месяц числом 01-12 | 12  |
| %y  | Год, короткий вариант, без века | 18  |
| %Y  | Год, полный вариант | 2018 |
| %H  | Час 00-23 | 17  |
| %I  | Час 00-12 | 05  |
| %p  | AM/PM | PM  |
| %M  | Минута 00-59 | 41  |
| %S  | Секунда 00-59 | 08  |
| %f  | Микросекунда 000000-999999 | 548513 |
| %z  | Разница UTC | +0100 |
| %Z  | Часовой пояс | CST |
| %j  | День в году 001-366 | 365 |
| %U  | Неделя числом в году, Воскресенье первый день недели, 00-53 | 52  |
| %W  | Неделя числом в году, Понедельник первый день недели, 00-53 | 52  |
| %c  | Локальная версия даты и времени | Mon Dec 31 17:41:00 2018 |
| %x  | Локальная версия даты | 12/31/18 |
| %X  | Локальная версия времени | 17:41:00 |
| %%  | Символ “%” | %   |

## **Другие datetime-библиотеки в Python**

### Arrow

Arrow — еще один популярный модуль, который делает более простым процесс создания, управления и форматирования дат и времени. Получить его можно с помощью pip. Для установки достаточно ввести `pip install arrow`.

Arrow можно использовать для получения текущего времени по аналогии с модулем datetime:

```
import arrow

current_time = arrow.now()
print(current_time)
print(current_time.to('UTC'))
2020-11-14T15:52:58.921198+00:00
2020-11-14T15:52:58.921198+00:00
```

### Maya

Maya упрощает процесс парсинга строк и конвертации часовых поясов. Например:

```
import maya

dt = maya.parse('2019-10-17T17:45:25Z').datetime()
print(dt.date())
print(dt)
print(dt.time())
2019-10-17
2019-10-17 17:45:25+00:00
17:45:25
```

### Dateutil

Dateutil — это мощная библиотека, которая используется для парсинга дат и времени в разных форматах. Вот некоторые примеры.

```
from dateutil import parser

dt_obj = parser.parse('Thu Oct 17 17:10:28 2019')
print(dt_obj)
dt_obj1=parser.parse('Thursday, 17. October 2019 5:10PM')
print(dt_obj1)
dt_obj2=parser.parse('10/17/2019 17:10:28')
print(dt_obj2)
t_obj=parser.parse('10/17/2019')
print(t_obj)
2019-10-17 17:10:28
2019-10-17 17:10:00
2019-10-17 17:10:28
2010-10-17 00:00:00
```

Важно, что в этом случае не нужны регулярные выражения при определении формата. Парсер ищет узнаваемые токены и либо возвращает корректный результат, либо выдает ошибку.