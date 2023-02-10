# <img src="https://user-images.githubusercontent.com/70295997/217977168-b299377a-fcdd-41a6-be7a-ef7328bf496b.png" width=40><img src="https://user-images.githubusercontent.com/70295997/217999210-749e2280-b4c1-4ef5-9704-59783fc19267.png" width=40> Page Object Model in Test Automation Scripts

The most common sign your test ***automation*** is failing is if you encounter ___issues with test scripts or test frameworks___. In fact, these account for [40% of all issues](https://github.com/lana-20/page-object-model/blob/main/the-11-most-common-challenges-in-automated-testing-web.pdf) that DevOps teams face.

<img src="https://user-images.githubusercontent.com/70295997/217998000-577fd244-2b72-4c7d-937d-a850289d26f6.png" width=40> Why?

Test script and test framework issues stem from problems with skillsets, culture and processes, and an overall lack of communication between testers and developers.

## <img src="https://user-images.githubusercontent.com/70295997/217991104-55be6e30-d0c5-419e-a29e-623dcef57c7b.png" width=40> Problems

<img src="https://user-images.githubusercontent.com/70295997/217975426-d625b031-99b9-4de3-9ede-1747abdd605d.png" width=40> ***Readability***
* A drawback of an automation script can be that it's hard to tell what user behavior it tries to encode.
* Might need to read very closely to identify the UI elements being interacted with. Then infer the user behavior out of these interactions. That's backwards!
* Think about user behavior on a high level. Use the words which describe what the user actually attempts to do, such as "login" or "add item to cart".
* Users do **not want/intend** to use UI elements, they **have to** use these elements because that's how apps works. A user's intent is not to _tap a button_ or _type text_, but to accomplish a specific task.
* Write tests at a level that reflects user intent and experience. Such test are transparent by making it obvious what they try to test.

<img src="https://user-images.githubusercontent.com/70295997/217976055-d3711547-b41b-480e-9399-dfd50b360ac2.png" width=40> ***Separation of Concerns***

<img src="https://user-images.githubusercontent.com/70295997/217997494-5385731c-3cf7-4e8c-8abe-dbd167da63da.png" width=400>

* Automation of user-like behavior involves 2 levels - **user intention** and **implementation**.
* One concern - **user intention** - is **what** the user tries to do, what the user sees themselves as doing.
* The other concern - **implementation** - is **how** the user tried to do it, meaning the precise and specific steps that are necessary for the user to do what they want.
* The code might not separate separate the concerns and have the **what** and the **how** mixed together.
* Need a distinct separation of concerns to organize the code, make it understandable, and make it reflect the larger reality the tests attempt to model.
* Also, a test can take place over two web pages or a mobile views, and nothing in the code may indicate that. 

<img src="https://user-images.githubusercontent.com/70295997/217983297-2bc19e4b-be88-4d51-bdbd-c7bafbb1bb27.png" width=40> ***Duplication***
* In 2+ different places, I may find and interact with one or another element. My developer can make changes to the app, such as these elements have different selectors. Then I need to hunt down every place in the testsuite where the current selectors are set, and update them. 
* In a complex feature with many different testcases all involving the same elements, potentially speading over multiple files, it's a significant pain to find and update all those instances.
* Element locators, a.k.a. object identifiers, can be particularly problematic to DevOps. Often the team lacks the knowledge to define the right element/object being used. A developer might design pages/views featuring multiple elements/objects with the same Id. 2 similar elements/objects in the same script cause issues in automation.
* Furthermore, the code can contain duplicate subsequent **Explicit Wait** <code>wait.until()</code> commands, that take an expected condition. This pattern is rather verbose and makes it hard to see what's actually happening.

<img src="https://user-images.githubusercontent.com/70295997/217990718-060c8748-901f-4c5e-8469-fe6f178b0fa6.png" width=40> ***Ambiguity***
* Sometimes, the code is plainly obscure, and it might be hard to tell what it's supposed to do. The script can contain a <code>driver.back()</code> call. It's unclear whether it's meant to navigate back a page/view or trigger any other other behaviors associated with the back button.
* Comment such parts for team discussion and comprehension.

## <img src="https://user-images.githubusercontent.com/70295997/217992251-184a6a61-e427-46bb-b8f1-ce7640ec85ea.png" width=40> Solution

Teams can address this issue head on with the help of a **Page Object Model**, a.k.a. POM or Pom. This design pattern ensures that if something changes, it all changes from one place.

<img src="https://user-images.githubusercontent.com/70295997/217998934-65abf84a-f9d3-40ed-9866-38eb8a6cb774.png" width=40> POM is commonly used in automation with Selenium, Appium and other testing frameworks. It is based on the idea of creating a separate class for each page/view in an application, with the class representing the page's elements and behaviors. The POM pattern helps to improve the maintainability and reliability of test automation by separating the test logic from the implementation details of the application under test (AUT).

Benefits of using page object pattern:
- Easy to read testcases
- Reusable code that can be shared across multiple testcases
- Reduction of duplicated code
- UI changes needs be fixed in only one place

### <img src="https://user-images.githubusercontent.com/70295997/217995742-eb499d19-e8ac-4115-8e26-d2fd2d3ffe94.png" width=40> Page Object
* Represents an area where the test interacts within the web/mobile app UI.
    * **Page** = webpage. This pattern was established as a part of web-based UI testing, before the advent of mobile app views. The UIs were all located on different **pages**.
    * **Object** = software object = class. This means making a model of a given page/view as a software object, e.g, a Python class.
* Construct a model for each page/view that has certain responsibilites.
* Use these objects in the test code, which has a different set of responsibilties.

### Page Object - Responsibilities
* The object is responsible for knowing all about the UI elements for a given page/view.
* Moreoever, it's responsible for **preventing** anyone else from knowing about them 👉 This pattern in softwate development is know as [***encapsulation***](https://github.com/lana-20/oop-encapsulation).
* Low-level details about UI elements on a specific page/view are unimportant to anyone outside the page/view. No need to grant access to them.
* Expose ***high-level*** actions that a user can might take on a given page/view. Choice of actions is contextual and situational.
   * E.g.: On a login page a user's action is **login**. A user doesn't think along the lines of "I need/intend to click the login button". So, **login** is an action I can take on the login page. An ability to **request a new password** is another example of user action.  The object exposes user actions as methods.
* Make data available for the purpose of understanding of what happens on the page/view. The page object can create methods or properties that return ***high-level*** info about what happens.
   * E.g.: Logging into a bank web app and landing on the dashboard page. One of the high-level info retrieval methods that the page object for this page might expose would be a method that gets the current account balance. This is something a use might want to do. This info is also useful for verification during the test run.

Page Object exposes high-level user actions and data via methods or properties of a class. Like any class method, the **login** action represented as method can take parameters. Think abstractly about the high-level ***what*** not the low-level ***how***. Conceptually, the login user action requires 2 pieces of info - username and password, hence these are the parameters that the login method takes.

POM is a model where objects expose high-level user actions to the outside world, while encapsulating knowledge about the action implementation using specific UI elements, that it doesn't share with anyone else.


### Test Case - Responsibilities
* Tests instantiate and use page objects in order to construct user flows at a high level. Testcases never, or rarely, know about the UI elements or access them directly. No need to use the WebDriver session object inside testcases at all. A testcase must define a user flow implementing high-level user actions. Users don't care or know about **drivers**.
* Make assertions on the state of the app to prove that things work.
   * Implicit Assertions: When navigating through various app pages/views. E.g., if an element doesn't exist the test fails. Hence, moving through a certain flow makes some assertions.
   * Explicit Assertions: When retrieving some info from the page/view and using the <code>assert</code> keyword to verify the app behaves as expected.

∴ POM helps implement the desired kind of **separation of concerns**. When using the model correctly, the test code becomes highly readable and reusable, and the automation code gets **encapsulated** in the natural groupings of page objects. **Duplication is discouraged** because ① different tests use the same page object actions which are exposed, and because ② the page objects themselves treat UI element selectors as data, and I can place them at the top of their file.

----

Here is a trivial example of how the POM pattern might be implemented in a test automation project using Selenium:

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

In this example, the LoginPage class represents the login page of the app, with methods for entering the username and password and clicking the login button. The test logic is separated from the implementation details of the page, making it easier to write and maintain the test code. The POM pattern can be extended to include multiple page classes, each representing a different page in the app. This helps to organize the test code and improve its maintainability and reliability.

----

[PageObject by Martin Fowler](https://www.martinfowler.com/bliki/PageObject.html)

[Page Object Model and Page Factory in Selenium](https://www.browserstack.com/guide/page-object-model-in-selenium)

[Page object models](https://www.selenium.dev/documentation/test_practices/encouraged/page_object_models/)

[Page Objects](https://selenium-python.readthedocs.io/page-objects.html)



