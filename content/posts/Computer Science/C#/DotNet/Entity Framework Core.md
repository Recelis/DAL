+++
title = 'Entity Framework'
date = 2024-11-29T08:25:30.532Z
draft = true
+++

[docs](https://learn.microsoft.com/en-us/training/modules/persist-data-ef-core/)

# Entity Framework1

## EF Core Architecture

### DbContext
Is a class that represents config, connection strings, logging, and the model.
Derived classes would include properties `DbSet<T>`.

### EF Core Provider
Translates object graph changes to SQL.

### Database Provider
Plugin library designed for specific database engine.

