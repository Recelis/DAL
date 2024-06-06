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
These are `classes` with `public methods` that exposes HTTP endpoints.

### Program.cs
This configures all the services and HTTP request pipeline. Also has app's entry point.

### [projectName].csproj
This contains `configuration metadata` for project.

### [projectName].http
Contains config to test REST APIs from VS Code.