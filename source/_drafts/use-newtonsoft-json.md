---
title: use-newtonsoft-json
tags:
---

.NET Core 3 is here and that means upgrade time!

It's great news that Microsoft have added the [System.Text.Json](https://docs.microsoft.com/en-us/dotnet/api/system.text.json?view=netcore-3.1) namespace so that we no longer have to reach for `Newtonsoft.Json` as the first thing we do after *File > New Project*.

However, if you don't feel ready, or there are few quirks and edge cases that you find when upgrading, you can configure your application to use `Newtonsoft.Json` instead!


### Using Newtonsoft.Json
It's really easy to set up. Simply, adding a package reference and then adding it to the service collection in your `Startup.cs` file...

#### Packagage Reference

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.NewtonsoftJson" Version="3.1.1" />
```

#### Startup.cs
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
      .AddMvc()
      .AddNewtonsoftJson();

    // OR

    services
        .AddControllers()
        .AddNewtonsoftJson();

    // OR

    services
        .AddControllersWithViews()
        .AddNewtonsoftJson();
}
```
You can also configure the serialization options...
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddControllers()
        .AddNewtonsoftJson(opts =>
        {
            opts.SerializerSettings.ContractResolver =
                new Newtonsoft.Json.Serialization.CamelCasePropertyNamesContractResolver();

            opts.SerializerSettings.Converters.Add(
                new Newtonsoft.Json.Converters.StringEnumConverter());
        });
}
```

### Conclusion
That's it! It's really that simple.

If you're wanting to upgrade your existing applications, but are unsure of the side affects or edge cases of using the new `System.Text.Json` namespace, you can really easily configure your application to use the perhaps more familiar `Newtonsoft.Json` library.
