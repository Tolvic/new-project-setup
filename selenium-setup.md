# Setting up Selenium Functional Tests

1. Create a test project
2. Ensure that the following NuGet packages are installed:

![](https://github.com/Tolvic/new-project-setup/blob/master/images/required-selenium-nuget-packages.PNG)

3. You will need the appropriate driver for the browser you wish to test in (e.g. Chrome, Firefox, etc.). You will be able to download these from the NuGet package manager (Selenium.WebDriver.ChromeDriver for Chrome, Selenium.Firefox.WebDriver for Firefox, etc.)
4. create a test file and write your Selenium Script. An example is given below:

```csharp
using System.Diagnostics.CodeAnalysis;
using NUnit.Framework;
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;

namespace QuizManagerFunctionalTests
{
    [ExcludeFromCodeCoverage]
    [TestFixture]
    class UnitTest1
    {
        private IWebDriver _driver;

        [SetUp]
        public void Init()
        {
            _driver = new ChromeDriver();
        }

        [TearDown]
        public void Cleanup()
        {
            _driver.Quit();
        }

        [Test]
        public void IndexHasTitle()
        {
            // Arrange

            var url = "https://google.com";

            // Act
            _driver.Navigate().GoToUrl(url);
            var result = _driver.Title;


            // Assert
            Assert.That(result, Does.Contain("Google").IgnoreCase);

        }
    }
}
```

**Note:** Selenium tests should always have a teardown where quit function is called on the driver. Otherwise you will be left with lots of instances of the browser running in the background which will slow down your computer and you will have to end task on each manually. 

To run functional tests in your local environment, you will need to either set up IIS to run your application or run two instances of Visual Studio: one to run the the application and the other to run the tests. 
