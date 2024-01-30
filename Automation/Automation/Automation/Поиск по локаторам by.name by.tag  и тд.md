Примеры: https://selenium-python.readthedocs.io/getting-started.html#:~:text=%3DFalse)-,2.4.%20Walkthrough%20of%20the%20example,-%C2%B6

# Navigating

WebDriver will wait until the page has fully loaded (that is, the onload event has fired) before returning control to your test or script

# Locating Elements

https://selenium-python.readthedocs.io/locating-elements.html#locating-elements
For single element:

- find_element

For multiple elements (these methods will return a list):

- find_elements

```
from selenium.webdriver.common.by import By

driver.find_element(By.XPATH, '//button[text()="Some text"]')
driver.find_elements(By.XPATH, '//button')
```

These are the attributes available for By class:

```
ID = "id"
NAME = "name"
XPATH = "xpath"
LINK_TEXT = "link text"
PARTIAL_LINK_TEXT = "partial link text"
TAG_NAME = "tag name"
CLASS_NAME = "class name"
CSS_SELECTOR = "css selector"
```

```
find_element(By.ID, "id")
find_element(By.NAME, "name")
find_element(By.XPATH, "xpath")
find_element(By.LINK_TEXT, "link text")
find_element(By.PARTIAL_LINK_TEXT, "partial link text")
find_element(By.TAG_NAME, "tag name")
find_element(By.CLASS_NAME, "class name")
find_element(By.CSS_SELECTOR, "css selector")
```

If you want to locate several elements with the same attribute replace find_element with find_elements.

## Locating by Id

```
<html>
 <body>
  <form id="loginForm">
   <input name="username" type="text" />
   <input name="password" type="password" />
   <input name="continue" type="submit" value="Login" />
  </form>
 </body>
</html>

login_form = driver.find_element(By.ID, 'loginForm')
```

## Locating by Name

```
<html>
 <body>
  <form id="loginForm">
   <input name="username" type="text" />
   <input name="password" type="password" />
   <input name="continue" type="submit" value="Login" />
   <input name="continue" type="button" value="Clear" />
  </form>
</body>
</html>

username = driver.find_element(By.NAME, 'username')
password = driver.find_element(By.NAME, 'password')

This will give the “Login” button as it occurs before the “Clear” button:
continue = driver.find_element(By.NAME, 'continue')
```

## Locating by XPath

```
<html>
 <body>
  <form id="loginForm">
   <input name="username" type="text" />
   <input name="password" type="password" />
   <input name="continue" type="submit" value="Login" />
   <input name="continue" type="button" value="Clear" />
  </form>
</body>
</html>

1. Absolute path (would break if the HTML was changed only slightly)
login_form = driver.find_element(By.XPATH, "/html/body/form[1]")
2. First form element in the HTML
login_form = driver.find_element(By.XPATH, "//form[1]")
3. The form element with attribute id set to loginForm
login_form = driver.find_element(By.XPATH, "//form[@id='loginForm']")
```

The username element can be located like this:

```
1. First form element with an input child element with name set to username
username = driver.find_element(By.XPATH, "//form[input/@name='username']")

2. First input child element of the form element with attribute id set to loginForm

username = driver.find_element(By.XPATH, "//form[@id='loginForm']/input[1]")
3. First input element with attribute name set to username
username = driver.find_element(By.XPATH, "//input[@name='username']")
```

The “Clear” button element can be located like this:

```
1. Input with attribute name set to continue and attribute type set to button

clear_button = driver.find_element(By.XPATH, "//input[@name='continue'][@type='button']")

2. Fourth input child element of the form element with attribute id set to loginForm

clear_button = driver.find_element(By.XPATH, "//form[@id='loginForm']/input[4]")

```

These examples cover some basics, but in order to learn more, the following references are recommended:

- [W3Schools XPath Tutorial](https://www.w3schools.com/xml/xpath_intro.asp)
- [W3C XPath Recommendation](http://www.w3.org/TR/xpath)
- [XPath Tutorial](http://www.zvon.org/comp/r/tut-XPath_1.html) - with interactive examples.

Here is a couple of very useful Add-ons that can assist in discovering the XPath of an element:

- [xPath Finder](https://addons.mozilla.org/en-US/firefox/addon/xpath_finder) - Plugin to get the elements xPath.
- [XPath Helper](https://chrome.google.com/webstore/detail/hgimnogjllphhhkhlmebbmlgjoejdpjl) - for Google Chrome

## Locating Hyperlinks by Link Text

```
<html>
 <body>
  <p>Are you sure you want to do this?</p>
  <a href="continue.html">Continue</a>
  <a href="cancel.html">Cancel</a>
</body>
</html>

continue_link = driver.find_element(By.LINK_TEXT, 'Continue')
continue_link = driver.find_element(By.PARTIAL_LINK_TEXT, 'Conti')
```

## Locating Elements by Tag Name

```
<html>
 <body>
  <h1>Welcome</h1>
  <p>Site content goes here.</p>
</body>
</html>

heading1 = driver.find_element(By.TAG_NAME, 'h1')
```

## Locating Elements by Class Name

```
<html>
 <body>
  <p class="content">Site content goes here.</p>
</body>
</html>

content = driver.find_element(By.CLASS_NAME, 'content')
```

## Locating Elements by CSS Selectors

```
<html>
 <body>
  <p class="content">Site content goes here.</p>
</body>
</html>

content = driver.find_element(By.CSS_SELECTOR, 'p.content')
```

Работа с iFrame (например когда подгружается страница с оплатой от банка, где сумма и надо ввести код)

```
@allure.step
def checking_text_price_in_tinkoff(self):
    time.sleep(0.5)
    iframe = self.driver.find_element(By.CSS_SELECTOR, 'iframe')
    self.driver.switch_to.frame(iframe)
    Wait.text_present_in_element(Cart.locator_total_price_in_tinkoff,
                                 Cart.text_total_price_in_tinkoff,
                                 self.driver)
```