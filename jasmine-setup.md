# Setting Up Jasmine Unit Testing

* Download the latest standalone version of Jasmine from here: https://github.com/jasmine/jasmine/releases
* In your test project create a folder called lib and create a folder called Jasmine inside of it.
* Copy in the Jasmine files you have downloaded. At the time of writing this (Jasmine-3.4.0), the expected files are:
  * boot.js
  * jasmine.css
  * jasmine.js
  * jasmine-favicon.png
  * jasmine-html.js
* create a Scripts folder for your Jasmine unit tests and write your tests in here. The below is a sample Jasmine test. 


```javascript
/// <reference path="../Jasmine/jasmine.js"/>
/// <reference path="../Jasmine/jasmine-html.js"/>

describe("file/module being tested", function () {
    beforeEach(function () {

    });

    afterEach(function () {

    });

    function setup() {

    }

    it("x + y returns 3", function () {
        // Arrange
        var x = 1;
        var y = 2;

        // Act
        var result = x + y;


        // Assert
        expect(result).toBe(3);
    });
});
```

## Source 
https://www.youtube.com/watch?v=qk91C1WbPRA
https://www.youtube.com/watch?v=gjRM-tTZwcw
