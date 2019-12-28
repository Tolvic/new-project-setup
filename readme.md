# New Project Setup Checklist

This repository is to provide me with a checklist when creating a new project

- [ ] Create Repository and clone to local machine
    - include a .gitignore file for visual studio
    - include a Readme file
- [ ] Add custom exclusions to .gitignore
    - [ ] */wwwroot/lib (client side libraries)
- [ ] [Create new project](new-mvc-project.md)
- [ ] Delete anything [unnecessary](things-to-delete-in-new-project.md) that comes out of the box
- [ ] Install client side libraries using [LibMan](library-manager.md)
- [ ] Set up unit test project
- [ ] install unit testing libraries:
    - [ ] Nunit 
    - [ ] Moq
    - [ ] [Jasmine](jasmine-setup.md)
- [ ] Set up Functional test project
- [ ] Install functional test libraries
    - [ ] [Selenium](selenium-setup.md)
- [ ] [Setup database](setup-database.md)

## Steps to research in the future
- [ ] [Setup IIS to run the web app](hosting-apps-in-iis.md) (current implementation does not work. Needs further investigation)
- [ ] Set up a logger
    - https://docs.microsoft.com/en-us/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log-for-insight
