# Joint.DB.Mongo

## MongoDB

Adds the set of conventions and ease of use for [MongoDB](https://www.mongodb.com/) integration with .NET Core.
![MongoRobo3T][image1]

## Installation

```
dotnet add package Joint.DB.Mongo
```

## Dependencies

- [Joint](https://www.nuget.org/packages/Joint/)
- [Joint.CQRS.Queries](https://www.nuget.org/packages/Joint.CQRS.Queries/)
- [MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/)

## Usage

Extend `IJointBuilder` with `AddMongo()` that will register the required services.

```c#
public static IJointBuilder RegisterJoint(this IJointBuilder builder)
{
    builder.AddMongo();
    // Other services.

    return builder;
}
```

In order to use `IMongoRepository` abstraction, invoke `AddMongoRepository<TDocument, TIdentifiable>("collectionName")` for each document that you would like to be able to access with this repository abstraction and ensure that document type implements `IIdentifiable` interface.

```c#
public static IJointBuilder RegisterJoint(this IJointBuilder builder)
{
    builder.AddMongo()
        .AddMongoRepository<RefreshTokenDocument, Guid>("refreshTokens");
    // Other services.

    return builder;
}
```

By using the provided `IMongoRepository` you can access helper methods such as `AddAsync()`, `BrowseAsync()` etc. instead of relying on `IMongoDatabase` abstraction available via [MongoDB.Driver](https://docs.mongodb.com/drivers/csharp).

```c#
public class SomeService
{
    private readonly IMongoRepository<RefreshTokenDocument, Guid> _repository;

    public SomeService(IMongoRepository<RefreshTokenDocument, Guid> repository)
    {
        _repository = repository;
    }
}
```

## Options

- connectionString - connection string e.g. mongodb://localhost:27017.
- database - database name.
- seed - boolean value, if true then IMongoDbSeeder.SeedAsync() will be invoked (if implemented).
- SetRandomDatabaseSuffix - might be helpful for the integration testing.

### appsettings.json

```json
"mongo": {
  "connectionString": "mongodb://localhost:27017",
  "database": "some-database-name",
  "seed": false,
  "setRandomDatabaseSuffix": false
}
```

## Table of Contents

- [Joint](https://github.com/flapek/Joint)
- [Joint.Auth](https://github.com/flapek/Joint.Auth)
- [Joint.CQRS.Commands](https://github.com/flapek/Joint.CQRS.Commands)
- [Joint.CQRS.Events](https://github.com/flapek/Joint.CQRS.Events)
- [Joint.CQRS.Queries](https://github.com/flapek/Joint.CQRS.Queries)
- [Joint.DB.Mongo](https://github.com/flapek/Joint.DB.Mongo)
- [Joint.DB.Redis](https://github.com/flapek/Joint.DB.Redis)
- [Joint.Docs.Swagger](https://github.com/flapek/Joint.Docs.Swagger)
- [Joint.Exception](https://github.com/flapek/Joint.Exception)
- [Joint.Logging](https://github.com/flapek/Joint.Logging)
- [Joint.WebApi](https://github.com/flapek/Joint.WebApi)

[image1]: https://github.com/flapek/Joint/blob/master/Resources/MongoRobo3T.png
