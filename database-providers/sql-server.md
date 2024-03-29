<a href="https://entityframework-extensions.net/">**Improve EF Core performance with EF Extensions**</a>

<a href="https://entityframework-extensions.net/">
<img src="https://zzzprojects.github.io/images/logo/entityframework-extensions-pub.jpg" width="600" />
</a>

# SQL Server

## SQL Server Provider

Microsoft SQL Server is a relational database management system \(RDBMS\) that supports a wide variety of transaction processing, business intelligence, and analytic applications in corporate IT environments.

* It is the default database provider which is available when you install [Entity Framework Extensions](https://entityframework-extensions.net/download)
* It allows Entity Framework Core to be used with Microsoft SQL Server \(including SQL Azure\).

### Install Entity Framework Core

Let's create a new application using the **Console App \(.NET Core\)** template, and install [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Z.EntityFramework.Extensions.EFCore/). [I](https://www.nuget.org/packages/Z.EntityFramework.Extensions.EFCore/)t is available as a nuget package and you can install it using **Nuget Package Manager**.

In the **Package Manager Console** window, enter the following command.

```bash
PM> Install-Package Microsoft.EntityFrameworkCore
```

You can also install it by right-clicking on your project in **Solution Explorer** and select **Manage Nuget Packages...**

![](../.gitbook/assets/image%20%286%29.png)

Search for [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Z.EntityFramework.Extensions.EFCore/) and install the latest version by pressing the install button. **I**t doesn't have additional logic that won't apply to all scenarios.

For example, EF Core will need to know what database or datastore you plan on working with and who those providers are in individual packages.

### Register EF Core Provider

For SQL Server LocalDB, which is installed with Visual Studio, we need to install [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer) and will get all the packages required for EF Core.

```bash
PM> Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

Now, you are ready to start your application.

### Create Data Model

Model is a collection of classes to interact with the database.

* A model stores data that is retrieved according to the Controller's commands and displayed in the View.
* It can also be used to manipulate the data to implement the business logic.

To create a data model for our application, we will start with the following two entities.

```csharp
public class Author
{
    public int AuthorId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public DateTime BirthDate { get; set; }
    public List<Book> Books { get; set; }
}

public class Book
{
    public int BookId { get; set; }
    public string Title { get; set; }
    public Author Author { get; set; }
}
```

There's a one-to-many relationship between `Author` and `Book` entities. In other words, an author can write any number of books, and a book can be written by only one author.

### Create Database Context

The database context class provides the main functionality to coordinate Entity Framework with a given data model.

* You create this class by deriving from the `System.Data.Entity.DbContext` class.
* In your code, you specify which entities are included in the data model.
* You can also customize certain Entity Framework behaviors.

So, let's add a new `BookStore` class which will inherit the `DbContext` class.

```csharp
public class BookStore : DbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(@"Data Source=(localdb)\ProjectsV13;Initial Catalog=BookStoreDb;");
    }
        
    public DbSet<Author> Authors { get; set; }
    public DbSet<Book> Books { get; set; }
}
```

In EF Core, the `DbContext` has a virtual method called `OnConfiguring` which will get called internally by EF Core.

* It will pass in an `optionsBuilder` instance which can be used to configure options for the `DbContext`.
* The `optionsBuilder` has `UseSqlServer` method which expects a connection string as a parameter.

Now, we are done with the required classes and database creation. Let's add some authors and book records to the database and then retrieve them.

```csharp
using (var context = new BookStore())
{
    context.Database.EnsureCreated();
    
    var authors = new List<Author>
    {
        new Author
        {
            FirstName ="Carson",
            LastName ="Alexander",
            BirthDate = DateTime.Parse("1985-09-01"),
            Books = new List<Book>()
            {
                new Book { Title = "Introduction to Machine Learning"},
                new Book { Title = "Advanced Topics on Machine Learning"},
                new Book { Title = "Introduction to Computing"}
            }
        },
        new Author
        {
            FirstName ="Meredith",
            LastName ="Alonso",
            BirthDate = DateTime.Parse("1970-09-01"),
            Books = new List<Book>()
            {
                new Book { Title = "Introduction to Microeconomics"}
            }
        },
        new Author
        {
            FirstName ="Arturo",
            LastName ="Anand",
            BirthDate = DateTime.Parse("1963-09-01"),
            Books = new List<Book>()
            {
                new Book { Title = "Calculus I"},
                new Book { Title = "Calculus II"}
            }
        }
    };

    context.Authors.AddRange(authors);
    context.SaveChanges();
}

using (var context = new BookStore())
{
    var list = context.Authors
        .Include(a => a.Books)
        .ToList();

    foreach (var author in list)
    {
        Console.WriteLine(author.FirstName + " " + author.LastName);

        foreach (var book in author.Books)
        {
            Console.WriteLine("\t" + book.Title);
        }
    }
}
```

If you run the application, you will see that authors and books are successfully inserted into the database.

## References

- [EF Core - SQL Server Provider](https://www.learnentityframeworkcore.com/database-providers#ef-core-sql-server-provider)

