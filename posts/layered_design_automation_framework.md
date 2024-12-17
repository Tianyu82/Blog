---
title: '[Selenium] Building a Maintainable Test Automation Architecture (TAA) with Layered Design'
date: '2024-10-27'
---

One of the most important principles for building a maintainable Test Automation Architecture (TAA) is to use a layered design. This approach enables better organization, separation of concerns, and more manageable test automation. In this blog, we’ll explore the two primary layers in a TAA: the **Test Abstraction Layer** (TAL) and the **Test Execution Layer** (TEL).

## Understanding the Test Execution Layer

The **Test Execution Layer** focuses on the implementation of the test flow and business logic of the tests. In this layer, you define the actual test scenarios, such as filling in a login form, submitting it, and verifying the results against expected outcomes. This layer also includes the test data that you used for conducting positive testing and negative testing.

For example:
- In a simple login test, the TEL handles the steps: filling in the login form, submitting it, and asserting the success or error message.
- In a more complex test, it might involve navigating across multiple pages (e.g., performing operations on Page A, then Page B, and so on), processing data (like date format conversions), and making assertions.

The TEL should not directly interact with web elements or make use of Selenium’s WebDriver functions. Instead, it should rely on the **Test Abstraction Layer** to handle these interactions.

## Why We Need the Test Abstraction Layer

The **Test Abstraction Layer** serves as an interface between the test logic and the physical interactions required to control the Software Under Test (SUT). Here, we use **Page Objects** to encapsulate UI elements and interactions. 

Page Objects represent areas of a web application (e.g., a login page) and abstract away the details of how we interact with them. For example, a `LoginPage` object may include a `fillLoginForm()` function that sets text fields and clicks the submit button. This encapsulation allows tests in the TEL to call `LoginPage.fillLoginForm()` without knowing the specifics of the underlying web element interactions.

### Benefits of Using Page Objects

Page Objects provide several advantages in a test automation framework:
1. **Reusability**: The code is reusable across multiple test scripts.
2. **Reduced Maintenance**: Changes in the UI only need to be updated within the Page Object, minimizing impact on the TEL.
3. **Encapsulation**: All GUI operations are encapsulated in a single layer, ensuring a clear separation between the business logic and technical implementation.
4. **Separation of Concerns**: The TEL handles only business logic, while the TAL deals with the technical details of interacting with the SUT.

## Best Practices for Page Objects

When designing Page Objects in your automation framework, follow these guidelines:
1. **No Business Assertions**: Page Objects should not contain business logic or assertions, such as verifying that an error message matches "Please enter a valid username."
2. **GUI-Related Assertions**: Any **technical** verifications related to the GUI (e.g., waiting for elements to load) should be handled within the Page Object.
3. **Encapsulate Waits**: Page Objects should manage all necessary waits, ensuring that elements are ready before use in the TEL.
4. **Isolate Selenium Calls**: Only Page Objects should contain calls to Selenium functions. This keeps the TEL independent of any specific UI interaction framework.

## Example: Implementing the Test Abstraction Layer and Test Execution Layer

Below is an example of a simple login test using the Page Object Model, demonstrating the separation between TAL and TEL.

### `login_page.py` (Test Abstraction Layer)

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

class LoginPage:
    def __init__(self, driver):
        self.driver = driver
        self.wait = WebDriverWait(driver, 10)

    def enter_username(self, username):
        username_field = self.wait.until(EC.visibility_of_element_located((By.ID, 'username')))
        username_field.clear()
        username_field.send_keys(username)

    def enter_password(self, password):
        password_field = self.wait.until(EC.visibility_of_element_located((By.ID, 'password')))
        password_field.clear()
        password_field.send_keys(password)

    def click_login_button(self):
        login_button = self.wait.until(EC.element_to_be_clickable((By.ID, 'loginButton')))
        login_button.click()

    def get_error_message(self):
        error_message = self.wait.until(EC.visibility_of_element_located((By.ID, 'errorMessage')))
        return error_message.text
```

In this `LoginPage` class:
- Each method (e.g., `enter_username`, `enter_password`, `click_login_button`) represents a single action on the page.
- These methods encapsulate Selenium calls, keeping the UI interactions separated from the TEL.

### `test_login.py` (Test Execution Layer)

```python
import unittest
from selenium import webdriver
from login_page import LoginPage

class LoginTests(unittest.TestCase):
    def setUp(self):
        self.driver = webdriver.Chrome()
        self.driver.get("https://example.com/login")
        self.login_page = LoginPage(self.driver)

    def tearDown(self):
        self.driver.quit()

    def test_successful_login(self):
        self.login_page.enter_username("validUser")
        self.login_page.enter_password("validPassword")
        self.login_page.click_login_button()
        
        self.assertIn("dashboard", self.driver.current_url)

    def test_unsuccessful_login(self):
        self.login_page.enter_username("invalidUser")
        self.login_page.enter_password("invalidPassword")
        self.login_page.click_login_button()
        
        error_message = self.login_page.get_error_message()
        self.assertEqual(error_message, "Invalid username or password.")

if __name__ == "__main__":
    unittest.main()
```

In the `LoginTests` class:
- The `test_successful_login` and `test_unsuccessful_login` test cases implement the business logic, such as asserting URLs or error messages, without directly interacting with Selenium commands.
- Each test case relies on the `LoginPage` methods to perform actions on the page.
