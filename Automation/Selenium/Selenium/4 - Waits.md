Implicity waits

После установки, неявное ожидание устанавливается для жизни экземпляра WebDriver объекта.

```
from selenium import webdriver
driver = webdriver.Firefox()
driver.implicitly_wait(10) # seconds
driver.get("http://somedomain/url_that_delays_loading")
myDynamicElement = driver.find_element_by_id("myDynamicElement")
```

Explicity waits
Первый вариант

```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
driver = webdriver.Firefox()
driver.get("http://somedomain/url_that_delays_loading")
try:
    element = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.ID, "myDynamicElement"))
    )
finally:
    driver.quit()
```

Второй вариант

```
from selenium.webdriver.support import expected_conditions as EC
wait = WebDriverWait(driver, 10)
element = wait.until(EC.element_to_be_clickable((By.ID,'someid')))
```

1. title_is : Ожидает, что заголовок страницы будет точно соответствовать заданному тексту.

`wait.until(EC.title_is('Expected Title'))`

1. title_contains : Ожидает, что заголовок страницы будет содержать заданный текст.

`wait.until(EC.title_contains('Expected Text'))`

1. url_to_be : Ожидает, что URL страницы будет точно соответствовать заданному URL.

`wait.until(EC.url_to_be('https://example.com'))`
1. url_contains : Ожидает, что URL страницы будет содержать заданный текст.
`wait.until(EC.url_contains('example'))`

1. presence_of_element_located : Ожидает, что элемент будет присутствовать в DOM.

`wait.until(EC.presence_of_element_located((By.ID, 'element_id')))`
1. visibility_of_element_located : Ожидает, что элемент станет видимым.
`wait.until(EC.visibility_of_element_located((By.XPATH, 'xpath_expression')))`

1. visibility_of : Ожидает, что элемент станет видимым и отображаемым на странице.

`element = wait.until(EC.visibility_of(driver.find_element(By.ID, 'element_id')))`

1. invisibility_of_element_located : Ожидает, что элемент станет невидимым.

`wait.until(EC.invisibility_of_element_located((By.CSS_SELECTOR, 'css_selector')))`

1. text_to_be_present_in_element : Ожидает, что определенный текст появится в элементе.

`wait.until(EC.text_to_be_present_in_element((By.ID, 'element_id'), 'expected_text')))`

1. text_to_be_present_in_element_value : Ожидает, что определенный текст появится в значении элемента (например, в поле ввода).

`wait.until(EC.text_to_be_present_in_element_value((By.ID, 'element_id'), 'expected_text')))`

1. element_to_be_clickable : Ожидает, что элемент станет кликабельным.
`wait.until(EC.element_to_be_clickable((By.ID, 'element_id')))`
1. element_to_be_selected : Ожидает, что элемент будет выбран.
`wait.until(EC.element_to_be_selected((By.ID, 'element_id')))`

1. element_selection_state_to_be : Ожидает, что состояние выбора элемента будет соответствовать заданному состоянию (True/False).

`wait.until(EC.element_selection_state_to_be((By.ID, 'element_id'), True))`

1. element_located_selection_state_to_be : Ожидает, что состояние выбора элемента (по его локатору) будет соответствовать заданному состоянию (True/False).

`wait.until(EC.element_located_selection_state_to_be((By.ID, 'element_id'), True))`

1. frame_to_be_available_and_switch_to_it : Ожидает, что фрейм будет доступен и переключается на него.

`wait.until(EC.frame_to_be_available_and_switch_to_it((By.NAME, 'frame_name')))`

1. alert_is_present : Ожидает, что появится всплывающее окно (alert).
`wait.until(EC.alert_is_present())`