# Setting up Selenium Functional Tests

1. Create a test project
2. Ensure that the following NuGet packages are installed:

![](https://github.com/Tolvic/new-project-setup/blob/master/images/required-selenium-nuget-packages.PNG)

3. create a test file and write your Selenium Script. An example is given below:

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

## Hosting .NET Core application in IIS
1. Open IIS and right-click on Sites, select Add Website.
2. Give the site a name and set the physical path to `%SystemDrive%\inetpub\wwwroot`.
3. Change the binding type to HTTPS and click Ok.

![](https://github.com/Tolvic/new-project-setup/blob/master/images/iis-add-website.PNG)

4. Open the Application Pools from the left hand panel.
5. Double-click on the Application Pool that matches the name of the site.
6. Change the .NET CLR version to No Managed Code and click Ok.

![](https://github.com/Tolvic/new-project-setup/blob/master/images/iis-edit-application-pool.PNG)

7. Right-click the site you created in the left hand panel and select Add Application.
8. Give the application an alias, this will be used to access the site later on.
9. Set the physical path to `[PathToProject]\bin\Debug\netcoreapp3.0` and click Ok.

![](https://github.com/Tolvic/new-project-setup/blob/master/images/iis-add-application.PNG)

10. Access the site by navigating to `https://localhost/[ApplicationAlias]` (e.g. `https://localhost/QuizManager`).