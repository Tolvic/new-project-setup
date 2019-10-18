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

By default, Entity Framework interprets a property that's named ID or classnameID as the primary key

Entity framework interprets properties that are named [ClassName]ID as a foreign key.  

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
The Database Context needs to be registered as a service within the `Startup` class to allow dependency inject. Components that require these services are provided them via their constructor parameters. 

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

    services.AddDbContext<[AppName]Context>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("[AppName]Context")));
}
```

The name of the connection string is passed in to the context by calling a method on a DbContextOptions object. For local development, the ASP.NET Core configuration system reads the connection string from the appsettings.json file.

### Add a database connection string
Add a connection string to the appsettings.json file:

```json
{
  "ConnectionStrings": {
      "[AppName]Context": "Server=(localdb)\\mssqllocaldb;Database=[AppName]Context-1;Trusted_Connection=True;MultipleActiveResultSets=true"
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

https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/working-with-sql?view=aspnetcore-3.0&tabs=visual-studio

This is usuefull while developing an application. 

It may also be usefull for running functional tests

## Modifying Tables
See this tutorial:

https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/new-field?view=aspnetcore-3.0&tabs=visual-studio

## Adding Validation
You can declaratively specify validation rules in one place (in the model class) and the rules are enforced everywhere in the app.

The DataAnnotations namespace provides a set of built-in validation attributes. For Example

```csharp
public class Movie
{
    public int Id { get; set; }

    [StringLength(60, MinimumLength = 3)]
    [Required]
    public string Title { get; set; }

    [Display(Name = "Release Date")]
    [DataType(DataType.Date)]
    public DateTime ReleaseDate { get; set; }

    [Range(1, 100)]
    [DataType(DataType.Currency)]
    [Column(TypeName = "decimal(18, 2)")]
    public decimal Price { get; set; }

    [RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
    [Required]
    [StringLength(30)]
    public string Genre { get; set; }

    [RegularExpression(@"^[A-Z]+[a-zA-Z0-9""'\s-]*$")]
    [StringLength(5)]
    [Required]
    public string Rating { get; set; }
}
```

* `Required` and `MinimumLength` indicate that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.
* `RegularExpression` -> input must meet regular expression 
* `Range` -> set min and max values
* `StringLength` -> Max length for a string (min length is optional)
* Value types (such as decimal, int, float, DateTime) are inherently required and don't need the [Required] attribute.

**Note:** Client side validation requires the jQuery client side validation library (included in the example [libman](https://github.com/Tolvic/new-project-setup/blob/master/library-manager.md) file)


If you disable JavaScript in your browser, client validation is disabled , inputs should be validated server-side using `ModelState.IsValid`

### DataType Attributes
The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.

The DataType attribute is used to specify a data type that's more specific than the database intrinsic type, they're not validation attributes.

The `DisplayFormat` attribute is used to explicitly specify the format for inputs, for example:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

You can use the DisplayFormat attribute by itself, but it's generally a good idea to use the DataType attribute.

The DataType Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more

More information about Validation and DataType attributes can be found here: https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/validation?view=aspnetcore-3.0
