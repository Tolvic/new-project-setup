# Things to Delete From a New Project
The following things are included out of the box when creating an MVC app using Visual Studio. These can all be deleted 

- [ ] site.css
- [ ] site.js
- [ ] lib
    - [ ] bootstrap
    - [ ] jQuery
    - [ ] jQuery Validaation
    - [ ] jQuery Validation Unobtrusive
- [ ] Home Controller
    - [ ] constructor
    - [ ] all methods except index
- [ ] All Models
- [ ] Views
    - [ ] Privacy
    - [ ] Error
    - [ ] _ValidationScriptsPartial
    - [ ] Delete anything unneccessary in the layout
        - [ ] Stylesheets
        - [ ] Everything in the body except renderbody tag
        - [ ] Scripts
    - [ ] appsettings.json contents
- [ ] Unused project references (right click the solution and select Refactor -> Remove Unused References -> Next)
