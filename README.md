# <img src="https://user-images.githubusercontent.com/70295997/217977168-b299377a-fcdd-41a6-be7a-ef7328bf496b.png" width=40> Page Object Model in Test Automation Scripts

The most common sign your test automation is failing is if you encounter _issues with test scripts or test frameworks_. In fact, these account for [40% of all issues](https://github.com/lana-20/page-object-model/blob/main/the-11-most-common-challenges-in-automated-testing-web.pdf) that DevOps teams face.

Why?

Test script and test framework issues stem from problems with skillsets, culture and processes, and an overall lack of communication between testers and developers.

## <img src="https://user-images.githubusercontent.com/70295997/217975286-da48f76d-9ff6-443c-9790-b39b864a5256.png" width=40> Define the problems to solve

<img src="https://user-images.githubusercontent.com/70295997/217975426-d625b031-99b9-4de3-9ede-1747abdd605d.png" width=40> ***Readability***
* A drawback of an automation script can be that it's hard to tell what user behavior it tries to encode.
* Might need to read very closely to identify the UI elements being interacted with. Then infer the user behavior out of these interactions. That's backwards!
* Think about user behavior on a high level. Use the words which describe what the user actually attempts to do, such as "login" or "add item to cart".
* Users do **not want/intend** to use UI elements, they **have to** use these elements because that's how apps works. A user's intent is not to _tap a button_ or _type text_, but to accomplish a specific task.
* Write tests at a level that reflects user intent and experience. Such test are transparent by making it obvious what they try to test.

<img src="https://user-images.githubusercontent.com/70295997/217976055-d3711547-b41b-480e-9399-dfd50b360ac2.png" width=40> ***Separation of Concerns***
* Automation of user-like behavior involves 2 levels - **user intention** and **implementation**.
* One concern - **user intention** - is **what** the user tries to do, what the user sees themselves as doing.
* The other concern - **implementation** - is **how** the user tried to do it, meaning the precise and specific steps that are necessary for the user to do what they want.
* The code might not separate separate the concerns and have the **what** and the **how** mixed together.
* Need a distinct separation of concerns to organize the code, make it understandable, and make it reflect the larger reality the tests attempt to model.
* Also, a test can take place over two web pages or a mobile views, and nothing in the code may indicate that. 

<img src="https://user-images.githubusercontent.com/70295997/217983297-2bc19e4b-be88-4d51-bdbd-c7bafbb1bb27.png" width=40> ***Duplication***
* In 2+ different places, I may find and interact with one or another element. My developer can make changes to the app, such as these elements have different selectors. Then I need to hunt down every place in the testsuite where the current selectors are set, and update them. 
* In a complex feature with many different testcases all involving the same elements, potentially speading over multiple files, it's a significant pain to find and update all those instances.
* Also, the code can contain duplicate subsequent **Explicit Wait** <code>wait.until()</code> commands, that take an expected condition. This pattern is rather vervose and makes it hard to see what's actually happening.


----
Draft/Pending revision or inclusion(?)
? Issues with Objects

Particularly problematic to DevOps teams are objects. With object identifiers, too often teams lack the knowledge to define the right object being used. This is especially true when dev designs pages featuring multiple objects with the same ID. Two similar objects on the same script are sure to cause issues in automation.
____

Teams can address this issue head on with the help of a Page Object Model, a.k.a. POM or Pom. This design pattern ensures that if something changes, it all changes from one place.

The Page Object Model is a design pattern that is commonly used in test automation with Selenium, Appium and other testing frameworks. It is based on the idea of creating a separate class for each page in an application, with the class representing the page's elements and behaviors. The POM pattern helps to improve the maintainability and reliability of test automation by separating the test logic from the implementation details of the application under test.

A page object represents an area where the test interacts within the web application user interface.

Benefits of using page object pattern:

- Easy to read test cases
- Creating reusable code that can share across multiple test cases
- Reducing the amount of duplicated code
- If the user interface changes, the fix needs changes in only one place

<img src="https://user-images.githubusercontent.com/70295997/209742408-26f2d12a-cc1e-4c50-9c77-258d60c112ab.png" width=500>

<img src="https://user-images.githubusercontent.com/70295997/209742972-c618ff9b-562d-4803-b5c7-d3ce1715a708.png" width=500>

----

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

[PageObject by Martin Fowler](https://www.martinfowler.com/bliki/PageObject.html)

[Page Object Model and Page Factory in Selenium](https://www.browserstack.com/guide/page-object-model-in-selenium)

[Page object models](https://www.selenium.dev/documentation/test_practices/encouraged/page_object_models/)

[Page Objects](https://selenium-python.readthedocs.io/page-objects.html)



