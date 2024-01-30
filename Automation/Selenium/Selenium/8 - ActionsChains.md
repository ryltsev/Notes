- Actions chains | Цепочка действий

```
from selenium.webdriver.common.action_chains import ActionChains
action = ActionChains(driver)
action.move_to_element(menu).click(hidden_submenu).perform()
```

- Performs all stored actions - perform()
- Right click | Правый клик мышкой - context_click()
- Double click | Двойной клик мышкой - double_click()
- Click and hold | Клик с удержанием - click_and_hold()
- Scroll to element/Hover | - move_to_element()
- Drag and Drop | Перетаскивание элементов - drag_and_drop()
- Pause - pause()
- Release | Отпустить удерживаемую кнопку мыши на элементе - release()