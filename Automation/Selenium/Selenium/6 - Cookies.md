- Что такое cookies - Небольшие файлики с инфой, которая сохраняется в момент посещения сайта. При повторном посещении сайта, браузер отправляет эти файлы на сервер, что бы сайт вспомнил некую инфу про нас, например, что ложили в корзину, таймеры и тд

1. Получение cookies

```
driver.get_cookie("name") - Получить конкретную куку по имени
driver.get_cookies() - Получись список всех кук
```

1. Добавление cookies

```
driver.add_cookie({
    "name": "Example",
    "value": "Example"
})
```

1. Замена cookies

```
driver.get_cookie("name") - Вот наша кука
driver.delete_cookie("name") - Просто удаляем куку по имени
driver.delete_all_cookies() - Или удалить все куки
driver.add_cookie({
    "name": "name",
    "value": "value"
}) - И просто добавляем новую с тем же именем и нужными нам параметрами
```

1. Сохранение cookies в файл

- Заходим на сайт

`driver.get("https://www.freeconferencecall.com/")`

- Авторизация
- Парсим и сохраняем куки в файл - Заранее импортируем либу - import pickle

`pickle.dump(driver.get_cookies(), open(os.path.dirname(__file__) + "\\cookies\\cookies.pkl", "wb")) - wb значит "Write bytes"`

1. Чтение cookies из файла

`cookies = pickle.load(open(os.path.dirname(__file__) + "\\cookies\\cookies.pkl", "rb")) - rb значит "Read bytes"`

1. Авторизация через cookies

- Добавляем все куки циклом

```
for cookie in cookies:
    driver.add_cookie(cookie)
```

- Рефрешем страницу

`driver.refresh()`