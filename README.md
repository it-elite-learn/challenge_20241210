# Challenge 10.12.2024 

Scenario:
You are developing a Library Management System for a local library. The system will allow management of books, authors, and library members, with core CRUD (Create, Read, Update, Delete) operations.

## Entities:

Book  
Author  
LibraryMember

## Technical Requirements:

Create an ASP.NET Core Web API  
Use Entity Framework Core for data persistence  
Implement Repository Pattern   
Implement basic validation  
Set up database migrations

## Detailed Exercise Steps:

#### Project Setup

Create a new ASP.NET Core Web API project  
Set up the project structure  
Install necessary NuGet packages:  

- Microsoft.EntityFrameworkCore  
- Microsoft.EntityFrameworkCore.Sqlite  
- Microsoft.EntityFrameworkCore.Tools -> should be installed by now  


#### Entity Properties

```csharp
public class Book
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string ISBN { get; set; }
    public int PublicationYear { get; set; }
    public int AuthorId { get; set; }
    public Author Author { get; set; }
    public List<BookLoan> BookLoans { get; set; }
}

public class Author
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public DateTime DateOfBirth { get; set; }
    public List<Book> Books { get; set; }
}

public class LibraryMember
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
    public string PhoneNumber { get; set; }
    public List<BookLoan> BookLoans { get; set; }
}

public class BookLoan
{
    public int Id { get; set; }
    public int BookId { get; set; }
    public Book Book { get; set; }
    public int LibraryMemberId { get; set; }
    public LibraryMember LibraryMember { get; set; }
    public DateTime LoanDate { get; set; }
    public DateTime? ReturnDate { get; set; }
}
```

#### Database Context and Configuration

- Create a DbContext
- Configure entity relationships
- Set up connection string
- Create initial migration

#### Repository Pattern Implementation
Create generic repository interfaces and implementations

```csharp
public interface IRepository<T> where T : class
{
    Task<T> GetByIdAsync(int id);
    Task<IEnumerable<T>> GetAllAsync();
    Task<T> AddAsync(T entity);
    Task UpdateAsync(T entity);
    Task DeleteAsync(int id);
}
```


#### Edpoints

Controller for Authors
```csharp
[HttpGet]
Get() {}

[HttpGet("{id}")]
Get(int id){}

[HttpPost]
Post(Author author){}

[HttpPut("{id}")]
Put(int id, Author author){}

[HttpDelete("{id}")]
Delete(int id){}

// Optional: Get books by a specific author
[HttpGet("{id}/books")]
GetAuthorBooks(int id) {}
```

Controller for Books
```csharp
// GET: api/Books
[HttpGet]
Get(
    [FromQuery] string title = null, 
    [FromQuery] int? authorId = null,
    [FromQuery] int pageNumber = 1, 
    [FromQuery] int pageSize = 10)

// GET: api/Books/5
[HttpGet("{id}")]
Get(int id)

// POST: api/Books
[HttpPost]
Post([FromBody] Book book)


// PUT: api/Books/5
[HttpPut("{id}")]
Put(int id, [FromBody] Book book)

// DELETE: api/Books/5
[HttpDelete("{id}")]
Delete(int id)
```
