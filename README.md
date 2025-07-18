# Selenium Automation Testing

## 📘 Index

1. 📚 [Overview](#overview)
2. [Selenium Components](#2-selenium-components)
   - a. [Selenium IDE](#a-selenium-ide)
   - b. [Selenium WebDriver](#b-selenium-webdriver)
     - i. [Key Concepts](#i-key-concepts)
     - ii. [WebDriver Hierarchy](#ii-webdriver-hierarchy)
   - c. [Selenium Grid](#c-selenium-grid)
     - i. [Purpose](#i-purpose)
     - ii. [Architecture & Use Case](#ii-architecture--use-case)
3. [Architecture & Design](#3-architecture--design)

   - a. [High-Level Design (HLD)](#a-high-level-design-hld)
   - b. [Selenium Architecture Diagram](#b-selenium-architecture-diagram)
   - c. [JAR Files (Java Archive)](#c-jar-files-java-archive)

4. [Environment Setup](#4-environment-setup)

   - a. [Manual Setup (Deprecated)](#a-old-manual-env-setup-not-recommended)
   - b. [Automated Setup with Maven](#b-automated-env-setup-mavenbuild-tool)

5. [Selenium Methods](#5-selenium-methods)

   - a. [Selenium Interfaces and their respective method Description](#a-selenium-interfaces-and-their-respective-method-description)
     - i. [SearchContext](#i-searchcontexti)
     - ii. [WebDriver](#ii-webdriver-i)
     - iii. [JavascriptExecutor](#iii-javascriptexecutor-i)
     - iv. [TakesScreenshot](#iv-takesscreenshot-i)
   - b. [Selenium Interfaces and Method Examples (Real-Time Use Cases)](#b-selenium-interfaces-and-method-examples-real-time-use-cases)

6. [Browser Navigation](#6-browser-navigation-methods)

   - a. [Navigation Methods](#a-browser-navigation-method-description)
   - b. [Get vs Navigate Comparison](#b-get-vs-navigate)

7. [WebDriver & WebElement](#7-selenium-webdriver--webelement-methods)

   - a. [WebDriver Interface-Locator Methods](#a-webdriver-interface-locator-methods)
   - b. [WebElement Interface-Action & Property Methods](#b-webelement-interface-action--property-methods)
   - c. [Testing Process](#c-webelements-testing-process)

8. [Locators in Selenium](#8-locators-in-selenium)

   - a. [Locator Types](#a-locator-types)
   - b. [Locator Syntax Table](#b-selenium-locator-syntax-table)
   - c. [XPath Essentials](#c-xpath-essentials)
     - i. [Absolute & Relative XPath](#i-absolute--relative-xpath)
     - ii. [XPath Tips & Tricks](#ii-xpath-tips--tricks)
     - iii. [Handling Special Characters in XPath](#iii-handling-special-characters-in-xpath)
     - iv. [Traversal Techniques](#iv-xpath-traversing-examples)

9. [General Test Scenarios](#9-general-test-scenarios)

10. [Wait Strategies](#10-wait-strategies)

    - a. [Implicit Wait](#a-implicit-wait)
    - b. [Explicit & Fluent Wait](#b-Explicit--fluent-waits)
    - c. [Thread.sleep() Note](#c-threadsleep-note)

11. [Common Exceptions](#11-common-exceptions)

12. [Practice Website](#12-practice-website)

---

# 1 Selenium Components

---

## a. **Selenium IDE**

- A browser plugin (mostly for Chrome and Firefox)
- Used for:
  - Record and playback of test cases
  - Creating automated test cases without programming knowledge
- Best for:
  - Beginners
  - Simple UI flow testing

---

## b. **Selenium web driver**

    - component in selenium
    - is an interface -->
    	[ contains methods] -------------->implement in a class RemoteWebDriver Class ---- extended to child class [ChromeDriver,FireFoxDriver,EdgeDriver]
    - is an API

### i. Key Concepts:

| Concept                 | Description                                                                                           |
| ----------------------- | ----------------------------------------------------------------------------------------------------- |
| **WebDriver Interface** | Contains essential browser automation methods (like `get()`, `findElement()`, `quit()` etc.)          |
| **Implemented by**      | `RemoteWebDriver` class implements the WebDriver interface.                                           |
| **Extended by**         | Browser-specific drivers like `ChromeDriver`, `FirefoxDriver`, `EdgeDriver` extend `RemoteWebDriver`. |

### ii. WebDriver Hierarchy

```text
SearchContext [I]  <-- root interface
     |
WebDriver [I]  <-- contains browser automation methods
     |
RemoteWebDriver [C]  <-- implementation of WebDriver
     |
+---------------------------+
|                           |
ChromeDriver [C]      FirefoxDriver [C]     EdgeDriver [C]
```

## c. **Selenium Grid**

### i. Purpose

Run tests in parallel across multiple machines/browsers/environments.

### ii. Architecture & Use Case

- **Hub**: Central point to control execution.
- **Node(s)**: Machines where actual tests run.
- Use Case:
- Suppose you want to run your tests on **Chrome + Windows**, **Firefox + Linux**, and **Edge + macOS** all at once. Selenium Grid allows you to do that using parallel test execution.

---

# 2 Architecture & Design

## a. High-Level Design (HLD)

1. JAVA client = contains java code to call web driver methods
2. web driver = perform method action on browser & SOMETIME capture info from browser [eg. title of web page,text display on web page]
   |
   | // b/w some w3c protocols earlier jason
   |
3. browser

## b. Selenium Architecture Diagram

```
            SearchContext[I] **root interface**
					|
	JavascriptExecuter[I]	WebDriver[I]	TakescreenShot[I]
					|
				RemoteWebDriver[C] **supermost class**
					|
		ChromiumDriver[C]	FirefoxDriver[C]
			|
	ChromeDriver[c]  EdgeDriver[C]

```

## c. JAR Files (Java Archive)

- Similar to .zip file
- Compress all java classes,methods,interfaces into 1 single file called jar file
- `full form` : JavaArchiveFiles

# 3 ⚙️ Environment Setup

## a. Old Manual Env. Setup **Not Recommended**

1. create a new java project
2. dowmload webdriver jars(.zip) and add to your project

## b. Automated Env. Setup **MAVEN[BUILD TOOL]**

**UNDERSTAND MAVEN AS PLAYSTORE**

1. create a simple maven project
2. will write test scripts in src/test/java
3. open pom.xml
4. add selenium dependency [mvn repository]
   in boot project the version is managed by boot
5. Update project

---

# 5 Selenium Methods

## a. Selenium Interfaces and their respective method Description

### i. SearchContext[I]

---

| name                       | work                                                       | returns                                          | example                                                          |
| -------------------------- | ---------------------------------------------------------- | ------------------------------------------------ | ---------------------------------------------------------------- |
| `findElement(By locator)`  | Locates a single web element on the page                   | `WebElement`                                     | `WebElement element = driver.findElement(By.id("username"));`    |
| `findElements(By locator)` | Locates all matching elements using the specified locator. | `List<WebElement> (can be empty but never null)` | `List<WebElement> links = driver.findElements(By.tagName("a"));` |

---

### ii. WebDriver [I]

| Name                 | Work                                                              | Returns            | Example                                                                 |
| -------------------- | ----------------------------------------------------------------- | ------------------ | ----------------------------------------------------------------------- |
| `get(String url)`    | Opens the given URL in the browser.                               | `void`             | `driver.get("https://www.google.com");`                                 |
| `getTitle()`         | Gets the title of the current page.                               | `String`           | `String title = driver.getTitle();`                                     |
| `getCurrentUrl()`    | Gets the URL of the current open page.                            | `String`           | `String url = driver.getCurrentUrl();`                                  |
| `getPageSource()`    | Gets the page source (HTML content) of the current page.          | `String`           | `String html = driver.getPageSource();`                                 |
| `findElement(By)`    | Finds a single web element on the page.                           | `WebElement`       | `WebElement el = driver.findElement(By.id("username"));`                |
| `findElements(By)`   | Finds all elements matching the given locator.                    | `List<WebElement>` | `List<WebElement> buttons = driver.findElements(By.tagName("button"));` |
| `getWindowHandle()`  | Returns the ID of the current window (or tab).                    | `String`           | `String winId = driver.getWindowHandle();`                              |
| `getWindowHandles()` | Returns IDs of all open windows/tabs.                             | `Set<String>`      | `Set<String> allWins = driver.getWindowHandles();`                      |
| `switchTo()`         | Switches driver's context to a different window, frame, or alert. | `TargetLocator`    | `driver.switchTo().window("win2");`                                     |
| `manage()`           | Accesses browser-level options like window size, cookies, etc.    | `Options`          | `driver.manage().window().maximize();`                                  |
| `close()`            | Closes the current browser window.                                | `void`             | `driver.close();`                                                       |
| `quit()`             | Quits the driver and closes every associated window.              | `void`             | `driver.quit();`                                                        |

### iii. JavascriptExecutor [I]

| Name                                  | Work                                                         | Returns  | Example                                                                                                                                   |
| ------------------------------------- | ------------------------------------------------------------ | -------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `executeScript(script, args...)`      | Executes synchronous JavaScript in the browser.              | `Object` | `JavascriptExecutor js = (JavascriptExecutor) driver; js.executeScript("document.getElementById('btn').click();");`                       |
| `executeAsyncScript(script, args...)` | Executes asynchronous JavaScript using a callback mechanism. | `Object` | `JavascriptExecutor js = (JavascriptExecutor) driver; js.executeAsyncScript("window.setTimeout(arguments[arguments.length - 1], 500);");` |

### iv. TakesScreenshot [I]

| Name                             | Work                                           | Returns                       | Example                                                                   |
| -------------------------------- | ---------------------------------------------- | ----------------------------- | ------------------------------------------------------------------------- |
| `getScreenshotAs(OutputType<T>)` | Captures the screenshot of the browser window. | `File`, `String`, or `byte[]` | `File src = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);` |

## b. Selenium Interfaces and Method Examples (Real-Time Use Cases)

---

## ✅ i. `SearchContext [I]`

| Method             | Real-Time Use Case                                                                                                                                                    |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `findElement(By)`  | Suppose you're automating a **login page**, and you want to enter the username. <br>`WebElement usernameInput = driver.findElement(By.id("username"));`               |
| `findElements(By)` | On a **dashboard page**, you want to count all buttons with class `"btn-primary"`. <br>`List<WebElement> buttons = driver.findElements(By.className("btn-primary"));` |

---

## ✅ ii. `WebDriver [I]`

| Method               | Real-Time Use Case                                                                                                                                   |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `get(String url)`    | You want to **open the homepage** of your application. <br>`driver.get("https://yourapp.com/home");`                                                 |
| `getTitle()`         | You verify if the page title matches the expected value after login. <br>`assert driver.getTitle().equals("Dashboard");`                             |
| `getCurrentUrl()`    | After clicking a link, check if the user is navigated to the correct page. <br>`String currentUrl = driver.getCurrentUrl();`                         |
| `getPageSource()`    | You want to **validate some HTML content** in your test. <br>`String source = driver.getPageSource();`                                               |
| `findElement(By)`    | Find and click the **submit button** on a form. <br>`driver.findElement(By.id("submit")).click();`                                                   |
| `findElements(By)`   | Collect all menu items in the navbar and iterate over them. <br>`List<WebElement> menuItems = driver.findElements(By.cssSelector("ul.nav > li"));`   |
| `getWindowHandle()`  | You're testing a page that opens a new **tab**, and you want to store the current tab ID. <br>`String parent = driver.getWindowHandle();`            |
| `getWindowHandles()` | After clicking a "Help" link that opens in new tab, you want to switch to it. <br>`Set<String> handles = driver.getWindowHandles();`                 |
| `switchTo()`         | Used to **switch to an alert or a popup**. <br>`driver.switchTo().alert().accept();` <br>OR switch to frame: `driver.switchTo().frame("frameName");` |
| `manage()`           | Before starting, **maximize the browser** and clear cookies. <br>`driver.manage().window().maximize();` <br>`driver.manage().deleteAllCookies();`    |
| `close()`            | Close the current tab after verifying the content. <br>`driver.close();`                                                                             |
| `quit()`             | End the test completely by closing all tabs/windows. <br>`driver.quit();`                                                                            |

---

## ✅ iii. `JavascriptExecutor [I]`

| Method                 | Real-Time Use Case                                                                                                                                                        |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `executeScript()`      | Scroll down the page to load more content dynamically. <br>`js.executeScript("window.scrollBy(0,500);");`                                                                 |
| `executeAsyncScript()` | Wait for some background script to finish (like animations or delayed loaders). <br>`js.executeAsyncScript("window.setTimeout(arguments[arguments.length - 1], 2000);");` |

---

## ✅ iv. `TakesScreenshot [I]`

| Method                        | Real-Time Use Case                                                                                                                                                                                                          |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `getScreenshotAs(OutputType)` | If a test fails (e.g., login error), take a screenshot and save it. <br>`java  <br>File src = ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);  <br>FileUtils.copyFile(src, new File("error_login.png"));  <br>` |

---

## 6 Browser Navigation Methods

## a. Browser Navigation method (Description)

| S.No | Method                      | Description                                            | Return Type | Example Code                                   |
| ---- | --------------------------- | ------------------------------------------------------ | ----------- | ---------------------------------------------- |
| 1    | `navigate().to(String url)` | Opens a specified URL (similar to `get()` method)      | `void`      | `driver.navigate().to("https://example.com");` |
| 2    | `navigate().refresh()`      | Reloads the current page                               | `void`      | `driver.navigate().refresh();`                 |
| 3    | `navigate().back()`         | Navigates back to the previous page in browser history | `void`      | `driver.navigate().back();`                    |
| 4    | `navigate().forward()`      | Moves forward to the next page in browser history      | `void`      | `driver.navigate().forward();`                 |

## b. Get Vs Navigate

| Feature                       | `driver.get(url)`                            | `driver.navigate().to(url)`                        |
| ----------------------------- | -------------------------------------------- | -------------------------------------------------- |
| **Purpose**                   | Opens a URL in the browser                   | Also opens a URL in the browser                    |
| **Return Type**               | `void`                                       | `void`                                             |
| **Navigation History**        | ❌ Does **not** maintain browser history     | ✅ Maintains browser history (allows back/forward) |
| **Chaining**                  | ❌ Cannot be chained                         | ✅ Can be chained using `navigate()` object        |
| **Support for Other Actions** | ❌ No support for `back()`, `forward()` etc. | ✅ Supports `back()`, `forward()`, `refresh()`     |
| **Common Use Case**           | Basic page load                              | Page load with navigation control                  |
| **Syntax**                    | `driver.get("https://example.com")`          | `driver.navigate().to("https://example.com")`      |
| **Performance**               | ⚡ Slightly faster (direct load)             | 🕒 Slightly slower due to history tracking         |

> **✅ Tip:**  
> Use `get()` when simply loading a page. Use `navigate().to()` when you plan to use browser navigation (like `back()`, `forward()`).

# 7 Selenium WebDriver & WebElement Methods

- WebElements : Anything which is present on webpage is called as webelement

---

## a. WebDriver Interface-Locator Methods

| S.No | Method                | Description                                                 | Return Type        | Example Code                                                      |
| ---- | --------------------- | ----------------------------------------------------------- | ------------------ | ----------------------------------------------------------------- |
| 1    | `findElement(By by)`  | Finds a **single WebElement** matching the given locator    | `WebElement`       | `WebElement btn = driver.findElement(By.id("submit"));`           |
| 2    | `findElements(By by)` | Finds **multiple WebElements** (returns empty list if none) | `List<WebElement>` | `List<WebElement> items = driver.findElements(By.tagName("li"));` |

---

## b. WebElement Interface-Action & Property Methods

| S.No | Method                                 | Description                                               | Return Type | Example Code                                                   |
| ---- | -------------------------------------- | --------------------------------------------------------- | ----------- | -------------------------------------------------------------- |
| 1    | `click()`                              | Clicks on the element                                     | `void`      | `driver.findElement(By.id("submit")).click();`                 |
| 2    | `sendKeys(CharSequence... keysToSend)` | Sends input text to the element                           | `void`      | `driver.findElement(By.name("username")).sendKeys("admin");`   |
| 3    | `clear()`                              | Clears the text from an input field                       | `void`      | `driver.findElement(By.name("email")).clear();`                |
| 4    | `getText()`                            | Gets the visible (inner) text of the element              | `String`    | `String msg = driver.findElement(By.id("message")).getText();` |
| 5    | `getAttribute(String name)`            | Returns the value of the given attribute of the element   | `String`    | `String type = element.getAttribute("type");`                  |
| 6    | `getCssValue(String propertyName)`     | Gets the CSS property value of the element                | `String`    | `String color = element.getCssValue("color");`                 |
| 7    | `getTagName()`                         | Returns the tag name of the element                       | `String`    | `String tag = element.getTagName();`                           |
| 8    | `isDisplayed()`                        | Checks if the element is visible                          | `boolean`   | `if (element.isDisplayed()) { ... }`                           |
| 9    | `isEnabled()`                          | Checks if the element is enabled (can be interacted with) | `boolean`   | `if (element.isEnabled()) { ... }`                             |
| 10   | `isSelected()`                         | Checks if a checkbox or radio button is selected          | `boolean`   | `if (checkbox.isSelected()) { ... }`                           |
| 11   | `submit()`                             | Submits a form element                                    | `void`      | `driver.findElement(By.id("form")).submit();`                  |
| 12   | `getLocation()`                        | Gets the coordinates of the element                       | `Point`     | `Point point = element.getLocation();`                         |
| 13   | `getSize()`                            | Returns the height and width of the element               | `Dimension` | `Dimension size = element.getSize();`                          |
| 14   | `getRect()`                            | Returns element's dimensions and position                 | `Rectangle` | `Rectangle rect = element.getRect();`                          |

---

## c. WebElements testing process

1. Inspect elem
2. locate elem
3. find elem
4. perform action

---

# 8 Locators in Selenium

## a. Locator Types

1. id[string] By.id("username")
2. name[string] By.name("username")
3. classname[string]
4. tagname[string]
5. linkedtext[string] <a href="index.html">This is link text</a> By.linkText("This is link text")
6. partiallinktext[string] <a href="index.html">This is link text</a> By.linkText("link text")
7. cssselector[string] : tagname['attribute name'='attribute value'] input[type='password'] || id=> input#email class=>tagName.classValue
8. xpath[string]

# b. Selenium Locator Syntax Table

| S.No | Locator Type      | Syntax Example                         | Description / Usage                                                       |
| ---- | ----------------- | -------------------------------------- | ------------------------------------------------------------------------- |
| 1    | `id`              | `By.id("username")`                    | Locates element by the `id` attribute.                                    |
| 2    | `name`            | `By.name("username")`                  | Locates element by the `name` attribute.                                  |
| 3    | `className`       | `By.className("form-control")`         | Locates element by the `class` attribute. Only one class name is allowed. |
| 4    | `tagName`         | `By.tagName("input")`                  | Locates element by tag name (e.g. `input`, `button`, `div`).              |
| 5    | `linkText`        | `By.linkText("Click here")`            | Locates anchor (`<a>`) elements by **exact visible text**.                |
| 6    | `partialLinkText` | `By.partialLinkText("Click")`          | Locates anchor (`<a>`) elements by **partial** match of visible text.     |
| 7    | `cssSelector`     | `By.cssSelector("input[type='text']")` | Uses CSS rules to locate elements. Fast and flexible.                     |
|      |                   | `By.cssSelector("input#email")`        | ID selector using CSS.                                                    |
|      |                   | `By.cssSelector("input.form-control")` | Class selector using CSS.                                                 |
| 8    | `xpath`           | `By.xpath("//input[@id='email']")`     | Powerful way to locate elements using **path-based expressions**.         |
|      | Absolute XPath    | `/html/body/div/form/input[1]`         | Full path from root node (not recommended).                               |
|      | Relative XPath    | `//input[@name='email']`               | Starts from anywhere in the DOM (preferred).                              |
|      | XPath by text     | `//button[text()='Login']`             | Matches exact visible text.                                               |
|      | XPath contains()  | `//div[contains(@class, 'login-box')]` | Partial match of attribute or text.                                       |
|      | XPath index       | `(//input[@type='text'])[1]`           | Selects the first match.                                                  |
|      | XPath last()      | `(//input[@type='text'])[last()]`      | Selects the last match.                                                   |
|      | Parent-child      | `//div[@id='box']/input`               | Finds child element inside parent.                                        |
|      | Child-parent      | `//input[@id='email']/..`              | Navigates to parent of current element.                                   |

---

💡 **Note**: Prefer `id`, `name`, and `cssSelector` when available — they’re usually faster and more readable.

## c. XPath Essentials

Locates elements using XPath expression.

### i. Absolute & Relative XPath

1.  Absolute XPath (not recommended)

```
/html/body/div[2]/form/input[1]
```

2.  Relative XPath (preferred)

```
//input[@type='text']                      // By attribute
//input[text()='Username']                // By exact text
//input[contains(text(),'User')]          // By partial text
//img[contains(@src,'logo')]              // By partial attribute
//div[1]                                  // First matching div
//div[last()]                             // Last matching div
//input[@id='email']                      // By specific attribute
```

### ii. XPath Tips & Tricks

//_ = all elements
//_[atribute='value'] = all matching attribute value
//tagname = all matching tagname
//tagname[1] = all the 1st matching tagname
//tagname[last()] = all matching tagname
//tagname[@attribute='value']

         xpath by attribute  = tagname[@an='av']
         xpath by text       = tagname[text()='tv']
          xpath by  contains =   tn[contains(text(),'tv')] OR tn[contains(@an,'av')] OR img[contains(@src,'img')]

### iii. Handling Special Characters in XPath

```
//a[contains(text(),"It's here")]         // use double quotes outside
//a[contains(text(),'He said "Hello"')]   // use single quotes outside
```

---

## iv. XPath Traversing Examples

| Traversing Type   | Example                                            | Description                   |
| ----------------- | -------------------------------------------------- | ----------------------------- |
| Parent to child   | `//div[@id='box']/input`                           | Select input inside a div     |
| Child to parent   | `//input[@id='email']/..`                          | Go up to parent               |
| Following-sibling | `//label[text()='Email']/following-sibling::input` | Locate input after a label    |
| Ancestor          | `//input[@id='email']/ancestor::form`              | Go to ancestor element (form) |
| Preceding-sibling | `//input[@id='email']/preceding-sibling::label`    | Get the label before input    |

---

# 9 General Test Scenarios

| S.No | Task                                                 | Action Steps                                                                                                                                                                              | Link to Code  |
| ---- | ---------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| 1    | Open browser based on user input                     | - Accept browser name (e.g., Chrome, Firefox)<br>- Open respective browser                                                                                                                | _To be added_ |
| 2    | Perform manage operations (browser-level operations) | - Open browser<br>- `deleteAllCookies()`<br>- `setSize()`<br>- `setPosition()`<br>- `maximize()`                                                                                          | _To be added_ |
| 3    | Perform navigation operations                        | - Open browser<br>- `maximize()` <br>- `deleteAllCookies()` <br>- goto url <br>- navigate to diff url <br>- navigate to previous page <br>-navigate to next page<br>-refresh current page | _To be added_ |

# 10 Wait Strategies

In selenium we have only 2 type of wait

## a. Implicit wait

## b. Explicit & Fluent Wait

- Explicit wait -> under explicit wait we have Fluent Wait

## c. Thread.sleep() Note

NOTE: thread.sleep()
method came from java its not a wait strategy of selenium.but can be used for wait but will wait for exact time period that is defined in it.

# 11 Common Exceptions

1. NoSuchElementException = synchronization problem(when your automation script run faster than the page)
2. ElementNotFoundException = when the locator is incorrect

# 12 Practice Website

1. OrangeHRM
2. Adactin
