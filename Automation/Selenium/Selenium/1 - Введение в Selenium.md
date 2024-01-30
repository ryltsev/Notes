https://www.selenium.dev/documentation/webdriver/
1. Установка venv && Selenium

```
python -m venv venv - Установка венв
venv/Scripts/activate - Активация венв
```

`pip install selenium`
1. Инициализация Chrome и Firefox драйверов

```
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.service import Service

service = Service(executable_path=ChromeDriverManager().install()) - Создаём и устанавливаем эксземпляр сервиса, который будет открывать и закрывать браузер автоматически

driver = webdriver.Chrome(service=service) - Инициализируем экземпляр драйвера и передаём туда сервис, что бы браузер закрывался и открывался сам без всяких quit

```

```
from selenium import webdriver
from webdriver_manager.firefox import GeckoDriverManager
from selenium.webdriver.chrome.service import Service
service = Service(executable_path=GeckoDriverManager().install())
driver = webdriver.Firefox(service=service)
```

1. Управление навигацией браузера
driver.get("https://yandex.ru")
driver.back() - Кнопка "Назад"
driver.forward() - Кнопка "Вперёд"
driver.refresh() - Кнопка "Рефреш"
2. Get browser data
Получение URL-страницы - driver.current_url
Получение title-страницы - driver.title
Валидация данных через assert - ???
Получение исходного кода страницы - driver.page_source