# Coding Conventions

## Introduction

In order to maintain readable, structured code we need a series of standardized conventions and coding style, so that you and others can easily read and understand the code.
We seek that every .NET developer uses and follows these conventions and coding style, though there are some exceptions.

## EditorConfig

A lot of the code style rules can be enforced by the use of an `.editorconfig` file added to the solution.

An `.editorconfig` file for .NET projects can be found in our public [editorconfig repository](https://github.com/team-noest/editorconfig/tree/master/dotnet).

### Using an .editorconfig file in Visual Studio

To use this file, you need to install the `EditorConfig Language Service` extension in Visual Studio. While VS2017 should include native support for `.editorconfig` files, it appears not to work without this extension. Also, changes made to the `.editorconfig` file seems to only apply after restarting Visual Studio. Since Visual Studio 2019 this issue has been resolved.

## Naming conventions

### Use PascalCasing for event-, property, namespace-, class- and methodnames

```csharp
namespace Noest.UserManagement
{
    public class UserAccount
    {
        public string Name { get; set; }

        public void UpdateName(string name)
        {
            this.Name = name;
        }
    }
}
```

### Use camelCasing for method parameters and local variables

```csharp
public class Noest
{
    public void CreateUserAccount(string name, string position)
    {
        var userAccount = new UserAccount(name, position);
    }
}
```

### Use predefined type aliases instead of system type names

```csharp
// Good
string firstName;
int lastIndex;
bool isDeleted;

// Bad
String firstName;
Int32 lastIndex;
Boolean isDeleted;
```

### Interface names always start with capital letter I

The name should be a noun or adjective.

```csharp
public interface IGadgetRepository
{
}

public interface IGadgetService
{
}

public interface IGroupable
{
}
```

### Avoid using abbreviations.

Exceptions do exist such as `Id`, `Ip`, `Ftp`, `Uri` ...

```csharp
// Good
UserGroup userGroup;
Assignment taskAssignment;

// Bad
UserGroup userGrp;
Assignment tskAsgnmnt;

// Exceptions
CustomerId customerId;
XmlDocument xmlDocument;
UriPart uriPart;
```

### DO NOT use include parent class name within a property name.

```csharp
// Good
Customer.Name

// Bad
Customer.CustomerName
```

### DO NOT use Hungarian notation for any other type identification in identifiers

```csharp
// Good
int counter;
string name;

// Bad
int iCounter;
string strName;
```

### DO NOT use all-CAPS for constant or static readonly fields

```csharp
// Good
public const string DeveloperType = "BackEnd";
public static readonly string DeveloperLevel = "Intermediate";

// Bad
public const string DEVELOPERTYPE = "BackEnd";
public static readonly string DEVELOPER_LEVEL = "Intermediate";
```

### DO NOT use underscores in identifiers.

Exception: private (readonly) variables.

```csharp
// Good
public DateTime clientAppointment;
public TimeSpan timeLeft;

// Bad
public DateTime client_appointment;
public TimeSpan time_left;

// Exception: private (readonly) variables must start with an underscore
private DateTime _registrationDate;
private readonly TimeSpan _cookieLifetime = TimeSpan.FromDays(7);
```

### DO NOT omit acces modifiers

Declare all identifiers with the appropriate acces modifier instead of allowing the default
``` csharp
// Bad
void MethodName(string parameter);

// Good
private void MethodName(string parameter);
public void MethodName(string parameter);
```

### DO NOT suffix enum names with Enum

```csharp
// Good
public enum MustardBrand
{
    DevosLemmens,
    Tierenteyn,
    Wostyn,
    Bister
}

// Bad
public enum MustardBrandEnum
{
    DevosLemmens,
    Tierenteyn,
    Wostyn,
    Bister
}
```

### Data Transfer Objects

If you use DTO's in your application, we recommend that you should have one DTO class for each entity/domain object, where the name can be suffixed with `Dto`.

```csharp
// <EntityName> + <Dto>
AccountDto
LocationDto
```

### Async Methods

Don't suffix Async methods with `Async`.

```csharp
// Good
Task<User> GetUserById(int userId);
Task<User[]> GetAllUsers();

// Bad
Task<User> GetUserByIdAsync(int userId);
Task<User[]> GetAllUsersAsync();
```

## Coding Style

### Tabs vs Spaces

We use the default indentation of 4 spaces, no tabs.

### Declare all member variables at the top of a class, with static/const members at the very top

In general, we follow this order:

* `private const`
* `private static readonly`
* `private readonly`
* `private`
* `public properties`

```csharp
public class Developer
{
    private const string Prefix = "NST";
    private static readonly string Seed = Cryptography.GenerateSeed(32);

    private readonly string _language;
    private int _age;

    public string Number { get; set; }
    public decimal Wage { get; set; }

    // Constructor
    public Developer()
    {
        // ...
    }
}
```

### Curly brackets below method/function/class definition

```csharp
// Good
public void SomeMethod()
{
    // ...
}

// Bad
public int CalculateHours() {
    // ...
}

// Exception: property declarations
public int Id { get; set; }

// Exception: if-statements with small one-line body
if (bookingService == null)
    throw new ArgumentNullException(nameof(bookingService));
```

### Order of namespaces

Order alphabetically but keep `System` directives on top.

### Summarize methods

Only summarize public methods that are exposed by the project.

This also includes ASP.NET Controller methods.

```csharp
/// <summary>
/// Description of what the method does.
/// </summary>
/// <param name="userId">An optional user id.</param>
/// <returns>Does the method return a view? A result value?</returns>
public IActionResult Index(int userId = null)
{
    // ...
}
```

Make sure to keep summaries up-to-date!

### Commenting

Writing and maintaining comments is expensive and it's the first thing that gets outdated during refactoring or bugfixes.

> If your feel your code is too complex to understand without comments, your code is probably just bad -- Sammy Larbi

Also read [coding without comments](https://blog.codinghorror.com/coding-without-comments/).

### Spacing between functions, methods and field declarations

Try to keep your code files structured. In short: have the amount of whitespace that is easy on the eye. **Think paragraph-like**.

```csharp
// Good
public void Method(int number)
{
    var variable = new Variable();
    variable.Property += number;

    if (statement)
    {
        variable.Boolean = true;
        return variable;
    }

    return variable;
}

public void AnotherMethod()
{
    // Do Stuff
}
```

```csharp
// Bad
public void Method(int number)
{
    var variable = new Variable();
    variable.Property += number;
    if(statement)
        return variable;
    return variable;
}
public void AnotherMethod()
{
    // Do Stuff
}
```

### Usage of regions

Regions are a way to structure large blocks of code. However, since we believe in small methods and classes, we will not allow the usage of regions.

###  Working with lots of arguments

When a constructor or method takes more than 3 arguments, sort them under each other instead of creating a long unwrapped list.

```csharp
// Good
public AccountController(
    IUnitOfWork unitOfWork,
    IObjectService objectService,
    ILogger logger,
    IHelper helper)
{
    _unitOfWork = unitOfWork;
    // ...
}

// Bad
public AccountController(IUnitOfWork unitOfWork, IObjectService objectService, ILogger logger, IHelper helper)
{
    _unitOfWork = unitOfWork;
    // ...
}
```

Also, when calling constructors or methods with more than 1 argument, use the argument names in the call. This makes the code safe against refactoring.

```csharp
// Good
accountController.AddAccount(username: command.Username, email: command.Email);

// Bad
accountController.AddAccount(command.Username, command.Email);
```

### ValueTuples

The use of C# 7 `ValueTuple` is only allowed in private methods. Any public method should not take or return a `ValueTuple`.

If a public method needs to take or return a combination of multiple fields, create a result or context class with the required fields.

The problem with tuples is that you can easily swap values without the compiler (or you) noticing. Consider the examples below:

```csharp
// Bad: tuple elements get mistakenly swapped in calling code
public (string FirstName, string LastName) GetPersonNameInfo()
{
    return (FirstName: "Bob", LastName: "Dylan");
}

(string LastName, string FirstName) result = GetPersonNameInfo();

// Bad: tuple elements get mistakenly swapped in method itself
public (string Firstname, string LastName) GetPersonNameInfo()
{
    return ("Dylan", "Bob");
}

// Good
public PersonNameInfo GetPersonNameInfo()
{
    return new PersonNameInfo
    {
        FirstName = "Bob",
        Lastname = "Dylan"
    };
}
```

### Usage of the ? operator

You can express calculations that otherwise need an `if-else` construction more concisely by using the conditional operator.

```csharp
bool IsValid;

// If-Else construction
if (isValid)
{
    expression;
}
else
{
    second_expression;
}

// Conditional expression
IsValid ? expression : second_expression;
```

We don't force one way or the other, just use what is the most readable / understandable for the given situation.

### Input parameters in lambda expressions

For simple and short lambda expressions, we allow the use of `x` as input parameter name. For more complex expressions, use full names.

```csharp
// Good
var enabledUsers = users.Where(x => x.IsEnabled);
var enabledUsers = users.Where(user => user.IsEnabled);

// Bad
var enabledUsers = users.Where(w => w.IsEnabled);

```
