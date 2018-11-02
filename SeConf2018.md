### Take Aways from SeConf 2018
#### ice-cream cone vs testing pyramid
* Testing should be in an ideal testing pyramid proportion not the ice-cream cone proportion.
* Exploratory manual regression testing at the top of the pyramid.
----
#### Fast feedback
* Tests should run really fast and give a very fast feedback
* Parallelization
    * Parallel execution is the key - 60 min vs 6 min (15 x 4 = 60 | 4 x 1 = 4)
* Automatic Retry of Failed Tests
----
#### Misc Talks
* Video recording with FFmpeg
* Allure report
----
#### Machine Learning and AI
* Test.io - Using Machine Learning and AI to find locators and perform actions
* Latest Appium includes test.io 's logic (https://appiumpro.com/)
* Appium classifier plugin (https://www.npmjs.com/package/test-ai-classifier)
* TensorFlow - Free AI engine
######Other Android Testing Tech
* Robotium - Android testing (https://github.com/RobotiumTech/robotium)
* Espresso - Android testing (https://developer.android.com/training/testing/espresso/)
----
#### AWS Lambdas to run Selenium tests
* Blackboard/lambda-selenium (https://github.com/blackboard/lambda-selenium)
----
####Misc Resources
- **Allure Reports,** http://allure.qatools.ru/
- **Marco Lüthy,** https://github.com/adieuadieu
- **Alister Scott,** https://watirmelon.blog/
- **Noah Isaacson,** https://github.com/nisaacson
----
#### Make tests less rude:
* Jain Waldrip **3 ways not to use Selenium**
* Checkout ExpectedConditions class which has a lot more methods now to wait efficiently
----
#### UI automation is not free
* exploratory testing
* bug bash
----
#### Key Note - Aslak Hellesøy, CEO, cucumber.io
###### Book References
* The Everything Psychology Book
* Flow
----
* Tests should tell exactly *where* the problem is. It should be testing probably just one thing and `assert` everywhere there could be a failure
* ```Integration Confidence``` (more components) vs ```Functionality Conference``` (less components.) Concept of running the same suite of test to run with different configuration.
* >I block time on calendar every month to revisit the code
    * to refactor or make any enhancements I can.
 ----
#### Accessibility Testing
* Axe core - accessibility testing (https://github.com/dequelabs/axe-core )
----
#### Misc Takeaways
* Fast and easy way to emulate mobile devices using capabilities on ChromeDriver
```java
public class Test{
    public void setup(){
        Map<String, String> mobileEmulation = new HashMap<>();
        mobileEmulation.put("deviceName", "Galaxy S9");
        ChromeOptions chromeOptions = new ChromeOptions();
        chromeOptions.setExperimentalOption("mobileEmulation", mobileEmulation);
        WebDriver driver = new ChromeDriver(chromeOptions);
    }
}
```
----
####`Sleep` is not your friend
###### • `wait`, using `javascriptExecutor()` to check for a variable
 * set a variable, then clear that variable when the process is complete. Check for the variable using `javascriptExecutor()` rather than `Thread.sleep()`.
----
#### Broken Images
* There is no out of the box solution for finding broken images in Selenium, but there are solutions.
* Here is one using JavascriptExecutor
```java
    public List<String> getAllBrokenImages() {
        List<WebElement> images = findAll(By.tagName("img"));
        List<String> brokenImages = new ArrayList<>();
        boolean isImage = false;
        for (WebElement image : images) {
            isImage = (boolean) executeScript("return arguments[0].complete " +
                    "&& typeof arguments[0].naturalWidth != 'undefined' " +
                    "&& arguments[0].naturalWidth > 0", image);

            if(!isImage)
                brokenImages.add(image.getAttribute("src"));
        }
        return brokenImages;
    }
```
----
#### What am I clicking on?
* Often while developing the test scripts, its hard to test if the driver is finding the right element to interact with.
* Highlight the element being interacted with...
```java
    public String highlightElement(By locator) {
        WebElement element = $(locator);
        String orgStyle = element.getAttribute("style");
        executeScript("arguments[0].setAttribute(arguments[1], arguments[2])", element, "style", "border: 3px solid cyan; color: yellow;");
        return orgStyle;
        }

    public void unHighlightElement(By locator, String orgStyle){
        executeScript("arguments[0].setAttribute(arguments[1], arguments[2])", $(locator), "style", orgStyle);
    }
```
----
#### Catch the driver in action
* Is Selenium executing too fast? Maybe the screenshot can help
```java
    private void takeScreenShot() {
        File scrFile = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
        String fileName = "target\\" + System.currentTimeMillis() / 1000 + "_screenshot.png";
        try {
            FileUtils.copyFile(scrFile, new File(fileName));
        } catch (IOException e) {
            e.printStackTrace();
        }
        String fullPath = System.getProperty("user.dir") + "\\" + fileName;
        System.out.println("Screenshot saved at \n" + fullPath);
    }
```
----
