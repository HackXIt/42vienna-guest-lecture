# AUTOMATED_TESTING.md

## Automated Testing Frameworks

Automation testing uses scripts and tools to execute tests automatically, improving efficiency and repeatability.

### Frameworks Overview

*Please note, if you use Robot Framework, you do **not** need Playwright or Seleniums as a dependency, they come along when installing the corresponding Robot Framework library.*

- **Robot Framework:** Python-based, keyword-driven approach, great for beginners. *(Supports both Playwright & Selenium as a generalized Test Framework)*
    - [Robot Framework Website](https://robotframework.org/)
    - [Robot Framework Documentation & Guides](https://docs.robotframework.org/docs)
    - [Robot Framework User Guide (Original Handbook)](https://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html)
    - Playwright based Robot Framework Library: [BrowserLibrary](https://robotframework-browser.org/)
    - Selenium based Robot Framework Library: [SeleniumLibrary](https://docs.robotframework.org/docs/different_libraries/selenium)
    - [VSCode Extension](https://marketplace.visualstudio.com/items?itemName=d-biehl.robotcode)
        - [RobotCode Documentation](https://robotcode.io/)
    - [Repository on GitHub](https://github.com/robotframework/robotframework)

- **Playwright:** JavaScript-based, suitable for cross-browser testing.
    - [Website Playwright.dev](https://playwright.dev/)
    - [Playwright Documentation - Introduction](https://playwright.dev/docs/intro)
    - [VSCode Extension](https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright)
    - [Repository on GitHub](https://github.com/microsoft/playwright)

- **Selenium:** Versatile browser automation tool supporting multiple languages.
    - [Website Selenium.dev](https://www.selenium.dev/)
    - [General Documentation](https://www.selenium.dev/documentation/)
    - [Selenium Unofficial Python Documentation](https://selenium-python.readthedocs.io/)
    - [Repository on GitHub](https://github.com/SeleniumHQ/selenium)

### Setup for Saucedemo.com

#### General Steps:
1. **Install Framework Dependencies**:
   - Robot Framework: `pip install robotframework`
   - Playwright: https://playwright.dev/docs/intro#installing-playwright
      - `npm init playwright@latest`
   - Selenium: `pip install selenium`
2. **Setup Test Files/Directories**:
   - Organize your tests into clear folders (e.g., `tests/login/`)

### Automated Test Example

#### Robot Framework Example:
```robotframework
# Example file: login_test.robot
# Robot Framework is somewhat similar to python
# Keywords and their arguments are seperated by whitespace (4 spaces)
# The document is divided into sections: Settings, Variables, Test Cases, Keywords
*** Settings ***
# Import the Browser library, which utilizes Playwright for browser automation
Library    Browser

# Define actions to perform before and after the test suite
Suite Setup    Open SauceDemo Website
Suite Teardown    Close Browser    ALL

*** Variables ***
# Variables are defined using:
# ${VARIABLE_NAME}    string_value
# @{LIST_NAME}    string_value_1    string_value_1    # ...
# &{DICT_NAME}    key_1=value_1    key_2=value_2    # ...
# You can define variables in this variables section, which can be used in this file

# Define the URL of the application under test
${URL}    https://www.saucedemo.com

# Define user credentials for login
${USERNAME}    standard_user
${PASSWORD}    secret_sauce

# Define locators for elements on the login page
${USERNAME_FIELD}    data-test=username
${PASSWORD_FIELD}    data-test=password
${LOGIN_BUTTON}      data-test=login-button

# Define a locator for an element that appears upon successful login
${PRODUCTS_TITLE}    css=.title

# The test case section starts with the title of a test case
# followed by a sequence of keywords and their arguments
*** Test Cases ***
Successful Login
    [Documentation]    Verify that a user can log in with valid credentials
    # Navigate to the login page
    New Page    ${URL}

    # Fill in the username and password fields
    Fill Text    ${USERNAME_FIELD}    ${USERNAME}
    Fill Text    ${PASSWORD_FIELD}    ${PASSWORD}

    # Click the login button
    Click    ${LOGIN_BUTTON}

    # Wait for the products page to load by checking the presence of the title
    Wait For Elements State    ${PRODUCTS_TITLE}    visible

    # Verify that the title text is "Products"
    Get Text    ${PRODUCTS_TITLE}    ==    Products

*** Keywords ***
Open SauceDemo Website
    # Launch a new browser instance (Chromium by default)
    New Browser    chromium
    # Create a new browser context (like a fresh user profile)
    New Context
    # Open a new page/tab in the browser
    New Page    ${URL}
```

#### Playwright Example:
```javascript
// Import Playwright test functions
const { test, expect } = require('@playwright/test');

// Define the login test
test('Successful login on SauceDemo', async ({ page }) => {
  // Navigate to the SauceDemo login page
  await page.goto('https://www.saucedemo.com');

  // Enter the username
  await page.fill('[data-test="username"]', 'standard_user');

  // Enter the password
  await page.fill('[data-test="password"]', 'secret_sauce');

  // Click the login button
  await page.click('[data-test="login-button"]');

  // Wait for the products page to load and verify the title
  await expect(page.locator('.title')).toHaveText('Products');
});

```

#### Selenium (Python) Example:
```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service as ChromeService
from selenium.webdriver.chrome.options import Options
import time

# Set up Chrome options
chrome_options = Options()
chrome_options.add_argument("--start-maximized")

# Initialize the Chrome driver
driver = webdriver.Chrome(service=ChromeService(), options=chrome_options)

try:
    # Navigate to the SauceDemo login page
    driver.get("https://www.saucedemo.com")

    # Enter the username
    driver.find_element(By.ID, "user-name").send_keys("standard_user")

    # Enter the password
    driver.find_element(By.ID, "password").send_keys("secret_sauce")

    # Click the login button
    driver.find_element(By.ID, "login-button").click()

    # Wait for the products page to load
    time.sleep(2)  # Not recommended for real tests; use explicit waits instead

    # Verify the title of the products page
    title = driver.find_element(By.CLASS_NAME, "title").text
    assert title == "Products", f"Expected title to be 'Products' but got '{title}'"

    print("Login test passed successfully.")

finally:
    # Close the browser
    driver.quit()
```
