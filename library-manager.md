# Library Manager

.net core has a built in library manager for client side libraries. 

These librarys can be installed by adding them to libman.json file. 

To use the library manager right click the project and click manage client side libraries. Then update the libman.json file. 

The following is my recommended configuration. 
```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "provider": "unpkg",
      "library": "bootstrap@4.3.1",
      "destination": "wwwroot/lib/bootstrap/"
    },
    {
      "library": "jquery@3.4.1",
      "destination": "wwwroot/lib/jquery/"
    },
    {
      "library": "jquery-validate@1.19.1",
      "destination": "wwwroot/lib/jquery-validation/"
    },
    {
      "library": "jquery-validation-unobtrusive@3.2.11",
      "destination": "wwwroot/lib/jquery-validation-unobtrusive/"
    },
    {
      "library": "font-awesome@5.8.2",
      "destination": "wwwroot/lib/font-awesome/"
    }
  ]
}
© 2019 GitHub, Inc.
```

More info on libman can be found here: 
https://docs.microsoft.com/en-us/aspnet/core/client-side/libman/?view=aspnetcore-3.0