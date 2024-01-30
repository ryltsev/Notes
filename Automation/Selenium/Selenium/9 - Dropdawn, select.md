Современные дропдауны через дивы
1. Способ

```
SELECT_LOCATOR = ("id", "dropdawn")
driver.get("example.com")
driver.find_element(*SELECT_LOCATOR).send_keys("Example")
driver.find_element(*SELECT_LOCATOR).send_keys(Keys.ENTER)
```

1. Способ

Поставить скрипт на паузу, найти сначала локатор дропдауна, а потом локатор конкретной опции и просто кликать сначала на дропдаун, а потом на опцию

Скрипт в консоле: setTimeout(function() { debugger; }, 5000);

Для работы с мультиселектами используем копипаст

```
SELECT_LOCATOR = ("id", "dropdawn")
driver.get("example.com")
driver.find_element(*SELECT_LOCATOR).send_keys("Example")
driver.find_element(*SELECT_LOCATOR).send_keys(Keys.ENTER)
driver.find_element(*SELECT_LOCATOR).send_keys("Example 1")
driver.find_element(*SELECT_LOCATOR).send_keys(Keys.ENTER)
```

Старые дропдауны через select
1. Создать объект селекта

```
from selenium.webdriver.support.select import Select

SELECT_LOCATOR = ("id", "select")
driver.get("example.com")

DROPDAWN = Select(driver.find_element(*SELECT_LOCATOR)) Создали экземляр класса селект и передали в него локатор нашего селекта

```

1. Далее работаем с объектом разными методами, например выбираем значение по value

`DROPDAWN.select_by_value("option 2")`
Можно гетнуть все опции из селекта (Запишется список web элементов)
`all_options = DROPDAWN.options`
И проверить наличие определённой опции по тексту

```
for option in all_options:
    if "Option 2" in option.text:
        print("Опция присутствует")
```

Проверить наличие определённой опции по индексу

```
for option in all_options:
    DROPDAWN.select_by_index(all_options.index(option))
```

Проверить наличие определённой опции по значению

```
for option in all_options:
    DROPDAWN.select_by_value(option.get_attribute("value"))
```