# Hosting .NET Core application in IIS
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
