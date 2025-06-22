[docs](https://learn.microsoft.com/en-gb/training/modules/build-web-api-aspnet-core/3-exercise-create-web-api)

As practiced on [musicCollection](https://github.com/Recelis/musicCollection) repo.

# ASP .NET Core MVC

## Creating ASP .NET project

```C#
dotnet new webapi -controllers -f net8.0
```

This creates a ASP .NET API project that uses `controllers`.

To run:

```C#
dotnet run
```

This installs project dependencies, compiles code and hosts web API on `ASP.NET Core Kestrel web server` for both HTTP and HTTPS endpoints.

In this project, it is not necessary to do a `dotnet tool restore` or a `dotnet restore` or even a `dotnet build`.

## Structure of Project

### Controllers

These are `classes` with `public methods` that exposes HTTP endpoints. These are public classes with one or more public methods known as **actions**.

#### Base Class: `ControllerBase`

The base class provides functionality for handling HTTP requests.

#### API controller class attributes

```csharp
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
```

Attributes are applied to the `WeatherForecastController`.
[ApiController] enables opinionated behaviours to building web APIs as defined in the `Microsoft.AspNetCore.Mvc.ApiControllerAttribute` class.

### Program.cs

This configures all the services and HTTP request pipeline. Also has app's entry point.

### [projectName].csproj

This contains `configuration metadata` for project.

### [projectName].http

Contains config to test REST APIs from VS Code.

#### Rest Client Send Request

You can actually test your API with the Send Request command in your [projectName].http file. provided that you have `Rest Client` installed in your VSCode.

### launchSettings.json

This is the file that contains the config for the app.

## Data Store

Models exist in a directory called Models. This is implemented using as `public class` under a namespace. e.g.

```csharp
namespace ContosoPizza.Models;

public class Pizza
{
    public int Id { get; set; }
    public string? Name { get; set; }
    public bool IsGlutenFree { get; set; }
}
```

Typically, this will be a connection to your DB.

## Data Service

[docs](https://learn.microsoft.com/en-gb/training/modules/build-web-api-aspnet-core/5-exercise-add-data-store)

This is the interface to the data store. Your controllers will interface with your data service and provide business logic. This extra layer will deal with your business logic leaving your controllers to deal with the HTTP requests.

```csharp
public class PizzaService
{
    private readonly MyDbContext _db;

    public PizzaService(MyDbContext db)
    {
        _db = db;
    }

    public async Task AddPizzaAsync(Pizza pizza)
    {
        _db.Pizzas.Add(pizza);
        await _db.SaveChangesAsync();
    }
}
```

### Inmemory vs Db DataService

Because your services will be added to your API via `Dependency Injection`, you can specify the interface and the class that you want to add as part of this service.

```c#
// if running unit tests
builder.Services.AddSingleton<IQuestionnaireService, InMemoryQuestionnaireService>();
// else if running deployment
builder.Services.AddSingleton<IQuestionnaireService, PostgresQuestionnaireService>();
```

This will require that your classes InMemoryQuestionnaireService and PostgresQuestionnaireService have to implement IQuestionnaireService.

```csharp
public class InMemoryQuestionnaireService : IQuestionnaireService
```

#### Db DataService

You can connect your Database to your DataService by adding your DB Context as a member.

```c#
public class PostgresQuestionnaireService : IQuestionnaireService
{
    private readonly LifeTrackerContext _lifeTrackerContext;
}
```

This will use dependency injection to pass to your controllers.

## Controllers

As before, all controller classes extends off the ControllerBase class.

```csharp
using Microsoft.AspNetCore.Mvc;

namespace ContosoPizza.Controllers;

[ApiController]
[Route("[controller]")]
public class PizzaController: ControllerBase
{
    private readonly PizzaService _pizzaService;

    public PizzaController(PizzaService pizzaService)
    {
        _pizzaService = pizzaService;
    }

    // GET all action
    [HttpGet]
    public ActionResult<List<Pizza>> GetAll () => PizzaService.GetAll();
    // GET  by Id action
    [HttpGet("{id}")]
    public ActionResult<Pizza> Get(int id) {
        var pizza = PizzaService.Get(id);
        if (pizza == null) return NotFound();
        return pizza;
    }
    // POST action
    [HttpPost]
    public IActionResult Create(Pizza pizza)
    {
        // This code will save the pizza and return a result
    }
    // PUT action
    [HttpPut("{id}")]
    public IActionResult Update(int id, Pizza pizza)
    {
        // This code will update the pizza and return a result
    }
    // DELETE action
    [HttpDelete("{id}")]
    public IActionResult Delete(int id)
    {
        // This code will delete the pizza and return a result
    }

}
```

For an opinionated MVC server, even for this example, the GetAll endpoint will be found on the [localhost:port]/pizza endpoint. This is because of the [Route] attribute and the /pizza part was due to the controller being named PizzaController.

[ActionResult] attribute is the base class for all action results in ASP .NET Core.

### Action Result Class

To set the different HTTP Statuses of the responses, you can use the ActionResult Class methods.

You'll need to include the MVC package.

```c#
using Microsoft.AspNetCore.Mvc;
```

This will allow you to return methods such as NotFound(), Ok()... etc which automatically set your HTTP Status to the corresponding state.

### Model binding

[docs](https://learn.microsoft.com/en-us/aspnet/core/mvc/models/model-binding?view=aspnetcore-9.0)

This is a way to bind incoming data from requests to your action parameters. It avoids writing your own converters from strings in your requests to your data models.

```c#
[HttpGet("{id}")]
public ActionResult<Pet> GetById(int id, bool dogsOnly)
```

When the app gets a request:

```http
https://contoso.com/api/pets/2?DogsOnly=true
```

This would go through the request string and assign the "2" to 2 as an int to id and "true" as a string to true as a boolean for dogsOnly.

### FromBody

The [FromBody] attribute is used to read in the request's body and assign it to the argument.

```c#
public async Task<IActionResult> CreateTemplate([FromBody] TemplateDto templateDto)
```

There are other From\* attributes as well.

# Assemblies

[docs](https://learn.microsoft.com/en-us/dotnet/standard/assembly/)
Assemblies are fundamental units of deployment, version control, reuse, activation scoping, and security permission for .NET applications.

They are made running `build` in Visual Studio. Each package will contain one or more assembly.
