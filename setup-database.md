# Setting up a Database

## Setup

### Nuget Packages
Install the following Nuget packages:
* Microsoft.EntityFrameworkCore.SqlServer
* Microsoft.EntityFrameworkCore.Tools

## Create your Model
The model will be represented as a table in the the database.

For Example:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

namespace MvcMovie.Models
{
    public class Movie
    {
        public int Id { get; set; }
        public string Title { get; set; }

        [DataType(DataType.Date)]
        public DateTime ReleaseDate { get; set; }
        public string Genre { get; set; }
        public decimal Price { get; set; }
    }
}
```

The Movie class contains an Id field, which is required by the database for the primary key.

### Database Context Class
A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete).

The database context is derived from Microsoft.EntityFrameworkCore.DbContext and specifies the entities to include in the data model.

1. Create a Data folder
2. Add a Data/[AppName]Context.cs file
3. Add the following code:


```csharp
using Microsoft.EntityFrameworkCore;
using [AppName].Models;

namespace MvcMovie.Data
{
    public class MvcMovieContext : DbContext
    {
        public [AppName]Context (DbContextOptions<MvcMovieContext> options)
            : base(options)
        {
        }

        public DbSet<User> User { get; set; }
        public DbSet<Tweet> Tweet { get; set; }
    }
}
```
**Note:** In Entity Framework terminology, an entity set typically corresponds to a database table. An entity corresponds to a row in the table.

The preceding code creates a DbSet<User> and DbSet<Tweet> entity sets.

### Register the Database Context
The Database Context needs to be registered as a service within the Startup class to allow dependency inject. Components that require these services are provided them via their constructor parameters. 

In the Startup.cs class add the following

```csharp
using [AppName].Data;
using Microsoft.EntityFrameworkCore;
```

and the following in the Startup.ConfigureServices:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();

    services.AddDbContext<MvcMovieContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MvcMovieContext")));
}
```

The name of the connection string is passed in to the context by calling a method on a DbContextOptions object. For local development, the ASP.NET Core configuration system reads the connection string from the appsettings.json file.

### Add a database connection string
Add a connection string to the appsettings.json file:

```json
{
  "ConnectionStrings": {
      "MvcMovieContext": "Server=(localdb)\\mssqllocaldb;Database=[AppName]Context-1;Trusted_Connection=True;MultipleActiveResultSets=true"
    }
}
```

### Scaffold CRUD Pages For Your Model
1. In Solution Explorer, right-click the Controllers folder > Add > New Scaffolded Item.
2. In the Add Scaffold dialog, select MVC Controller with views, using Entity Framework > Add.
3. Complete the Add Controller dialog:
      * Model class: [Model Name] ([AppName].Models)
      * Data context class: [AppName]Context ([AppName.Data)
      * Views: Keep the default of each option checked
      * Controller name: Keep the default MoviesController
      * Select Add

Screenshots here: https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-3.0&tabs=visual-studio#scaffold-movie-pages

This process will automatically create:
* A Controller
* Razor views for create, Delete, Details, Edit and Index

**Note:** You can't use the scaffolded pages yet because the database doesn't exist. 

### Initial Migration
Migrations is a set of tools that let you create and update a database to match your data model.

From the Tools menu, select NuGet Package Manager > Package Manager Console (PMC).

n the PMC, enter the following commands:

```console
Add-Migration InitialCreate
Update-Database
```
`Add-Migration InitialCreate`: Generates an Migrations/{timestamp}_InitialCreate.cs migration file. The `InitialCreate` argument is the migration name. Because this is the first migration, the generated class contains code to create the database schema. The database schema is based on the models specified in the [AppName]Context class.

`Update-Database`: Updates the database to the latest migration, which the previous command created. This command runs the Up method in the Migrations/{time-stamp}_InitialCreate.cs file, which creates the database.


## Sources
https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-3.0&tabs=visual-studio


## Seed Data
This tutorial has some infomration about setting up seed data for a database:



https://docs.microsoft.com/en-us/aspnet/core/tutorials/razor-pages/sql?view=aspnetcore-3.0&tabs=visual-studio

This may be usefull for running functional tests