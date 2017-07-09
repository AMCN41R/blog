---
title: Introducing Harness, an integration test harness for MongoDb
tags: [Harness, C#, Mongo, MongoDb, Test, Testing, Unit Test, Unit Testing, Integration Test, Integration Testing]
---

## What is Harness?
Harness is a .NET library designed to manage the state of a Mongo database during testing. It is free and open source, and is licensed under the [MIT License](https://github.com/AMCN41R/Harness/blob/dev/LICENSE).

At its core, it endevours to aid unit and integration testing by putting one or more Mongo databases in a known state at the beginning of a test, or suite of tests. The framework allows you to control your code’s dependency on a Mongo database, therefore increasing test consistency and repeat-ability.



## Why do I need this?
If you want to perform some integration tests on your repositories or data access layer, it would be really helpful to have a known set of data in your target database so that you can write accurate and consistent tests. Harness allows you do do exactly that!

## How do I get it?
The easiest way to include it in a project is via [nuget](https://www.nuget.org/packages/Harness/), and can be installed with...
```
Install-Package Harness
```

## What's the simplest way to get it working?
Once you have added Harness to you project, the easiest way to get up and running is with 2 simple json files...

### Settings File
The settings file must have a .json extension. It is a json object that contains a `Databases` property that is an array of `Database` objects.

#### Example settings file, 'ExampleSettings.json'
```
{
    "Databases": [
        {
            "DatabaseName": "TestDb1",
            "ConnectionString": "mongodb://localhost:27017",
            "CollectionNameSuffix": "",
            "DropFirst": true,
            "Collections": [
                {
                    "CollectionName": "TestCollection1",
                    "DataFileLocation": "Collection1.json", // this is the path to data files described below
                    "DropFirst": false
                },
                {
                    "CollectionName": "TestCollection2",
                    "DataFileLocation": "Collection2.json",
                    "DropFirst": false
                }
            ]
        }
    ]
}
```

### Data File
Test data files must have a .json extension and contain an array of json objects.

#### Example data file, 'Collection1.json'
```
[
    {
        "_id": { "$oid": "56a69c36d1894801d0ce3d05" },
        "Col1b": "Value1b",
        "Col2b": "Value2b"
    },
    {
        "_id": { "$oid": "56a69c36d1894801d0ce3d06" },
        "Col1b": "Value3b",
        "Col2b": "Value4b"
    }
]
```

Once you have created these, it's as simple as making your class of tests extend `HarnessBase`, and to give it the`[HarnessConfig]` attribute witht the path to your settings file...

```csharp
using Harness;

[HarnessConfig(ConfigFilePath = "path/to/settings.json")]
public class MyMongoIntegrationTests : HarnessBase {
    // tests go here
    ...
}
```

## Are there more examples?
Do you like config files or fluent configuration in code? Either way, Harness has you covered. You can check out examples of how to get started with both on the Harness [GitHub](https://github.com/AMCN41R/harness) page.

## Contributing


## License
[MIT License](https://github.com/AMCN41R/Harness/blob/dev/LICENSE)
