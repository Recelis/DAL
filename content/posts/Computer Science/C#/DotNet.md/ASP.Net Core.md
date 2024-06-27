+++
title = 'ASP .NET'
date = 2024-03-18T09:50:39+10:00
draft = true
+++

[docs](https://learn.microsoft.com/en-gb/training/modules/build-web-api-aspnet-core/3-exercise-create-web-api)

As practiced on [musicCollection](https://github.com/Recelis/musicCollection) repo.

# ASP .NET Core

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
This is the interface to the data store. Your controllers will interface with your data service and provide business logic.

## Controllers
As before, all controller classes extends off the ControllerBase class.

```csharp
using Microsoft.AspNetCore.Mvc;

namespace ContosoPizza.Controllers;

[ApiController]
[Route("[controller]")]
public class PizzaController: ControllerBase
{
    public PizzaController()
    {

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
