# Library Manager

.net core has a built in library manager for client side libraries. 

These librarys can be installed by adding them to libman.json file. 

To use the library manager right click the project and click manage client side libraries. Then update the libman.json file. 

The following is my recommended configuration. **Check that these are the latest version for each library**
```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "provider": "unpkg",
      "library": "bootstrap@4.4.1",
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
      "library": "font-awesome@5.13.0",
      "destination": "wwwroot/lib/font-awesome/"
    }
  ]
}
```

Upon rebuilding your project, the libraries specified in your libman file should be downaload into wwwroot. 

To use these libraries you will need to references them; most likely in your Layout file.

More info on libman can be found here: 
https://docs.microsoft.com/en-us/aspnet/core/client-side/libman/?view=aspnetcore-3.0


