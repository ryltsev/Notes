1. Отключение режима WebDriver - То есть веб будет думать, что это человек
`chrome_options.add_argument("--disable-blink-features=AutomationControlled")`

- Работа с User-agent - помогает при headless моде, если не проходит проверка на человека. Юзер агенты можно нагуглить
- Обойти капчу - попросить разраба сделать определённый юзер агент, например - Automation, что бы капча отключалась пари передаче данного агента.

`chrome_options.add_argument("--user-agent=Any string")`

- Сделать скриншот

`driver.save_screenshot("filename.png")`

- Работа с alerts

```
alert = wait.until(EC.alert_is_present())
driver.switch_to.alert
alert.accept() - Принять аллерт
alert.dismiss() - Отклонить аллерт
alert.text - Получить текст алерта (Вызывается как свойство, без скобок)
alert.send_keys("Hello world") - Ввести текст в алерт, если есть инпут
```