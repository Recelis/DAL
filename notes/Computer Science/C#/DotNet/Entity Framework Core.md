[docs](https://learn.microsoft.com/en-us/training/modules/persist-data-ef-core/)

# Entity Framework

## EF Core Architecture

### DbContext
Is a class that represents config, connection strings, logging, and the model.
Derived classes would include properties `DbSet<T>`.

### EF Core Provider
Translates object graph changes to SQL.

### Database Provider
Plugin library designed for specific database engine.

