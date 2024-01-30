1. Работа с обЪектом опций
Создаём объект опций до объявлений драйвера

```
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument("--headless") - Без графического запуска
chrome_options.add_argument("--incognito") - В режиме инкогнито

chrome_options.add_argument("--ignore-certificate-errors") - Если на сайте нет SSL серта или http

chrome_options.add_argument("--window-size=X,Y") - Установка размера окна на драйвер. То есть браузер откроется сразу в нужном размере

chrome_options.add_argument("--disable-cache") - Отключаем кэш, если используем обычный режим

service = Service(executable_path=ChromeDriverManager().install())
driver = webdriver.Chrome(service=service, options=chrome_options)

driver.set_window_size(1920, 1080) - Установка размера окна уже после открытия браузера

```

1. Стратегия загрузки страницы

chrome_options.page_load_strategy = "normal" - дожидается загрузки всех ресурсов (используется по умолчанию)

chrome_options.page_load_strategy = "eager" - дожидается загрузки DOM
1. Скачивание файлов

```
prefs = {

    "download.default_directory": f"{os.getcwd()}/downloads" делаем путь до папки в которой хранится код getcwd(current working directory) и далее проваливаемся в созданную папку downloads

}

chrome_options.add_experimental_option("prefs", prefs) Прокидываем название параметры, закреплено имя "prefs" и затем прокидыаем наши параметры

```

1. Загрузка файлов
Если input
`send_keys(путь до файла)`

Если другой тег, то input может быть скрыт. Попробовать найти по //input[@type='file']