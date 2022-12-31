# POM - Page Object Model in Test Automation Scripts

The Page Object Model (POM) is a design pattern that is commonly used in test automation with Selenium and other testing frameworks. It is based on the idea of creating a separate class for each page in an application, with the class representing the page's elements and behaviors. The POM pattern helps to improve the maintainability and reliability of test automation by separating the test logic from the implementation details of the application under test.

Here is an example of how the POM pattern might be implemented in a test automation project using Selenium:

    from selenium import webdriver
    from selenium.webdriver.common.by import By

    class LoginPage:
      def __init__(self, driver):
        self.driver = driver

      def enter_username(self, username):
        self.driver.find_element(By.ID, "username").send_keys(username)

      def enter_password(self, password):
        self.driver.find_element(By.ID, "password").send_keys(password)

      def click_login(self):
        self.driver.find_element(By.XPATH, "//button[text()='Log in']").click()

    driver = webdriver.Chrome()
    driver.get("http://example.com/login")

    login_page = LoginPage(driver)
    login_page.enter_username("testuser")
    login_page.enter_password("testpass")
    login_page.click_login()

In this example, the LoginPage class represents the login page of the application, with methods for entering the username and password and clicking the login button. The test logic is separated from the implementation details of the page, making it easier to write and maintain the test code. The POM pattern can be extended to include multiple page classes, each representing a different page in the application. This helps to organize the test code and improve its maintainability and reliability.

----

A page object represents an area where the test interacts within the web application user interface.

Benefits of using page object pattern:

- Easy to read test cases
- Creating reusable code that can share across multiple test cases
- Reducing the amount of duplicated code
- If the user interface changes, the fix needs changes in only one place

<img src="https://user-images.githubusercontent.com/70295997/209742408-26f2d12a-cc1e-4c50-9c77-258d60c112ab.png" width=500>

<img src="https://user-images.githubusercontent.com/70295997/209742972-c618ff9b-562d-4803-b5c7-d3ce1715a708.png" width=500>


----


[PageObject by Martin Fowler](https://www.martinfowler.com/bliki/PageObject.html)

[Page Object Model and Page Factory in Selenium](https://www.browserstack.com/guide/page-object-model-in-selenium)

[Page object models](https://www.selenium.dev/documentation/test_practices/encouraged/page_object_models/)

[Page Objects](https://selenium-python.readthedocs.io/page-objects.html)



