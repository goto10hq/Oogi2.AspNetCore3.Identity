![Oogi2.AspNetCore3.Identity](https://raw.githubusercontent.com/goto10hq/Oogi2.AspNetCore3.Identity/master/oogi2.aspnetcoreidentity-icon.png)

# Oogi2.AspNetCore3.Identity

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Latest Version on NuGet](https://img.shields.io/nuget/v/Oogi2.AspNetCore.Identity.svg?style=flat-square)](https://www.nuget.org/packages/Oogi2.AspNetCore.Identity/)
[![NuGet](https://img.shields.io/nuget/dt/Oogi2.AspNetCore3.Identity.svg?style=flat-square)](https://www.nuget.org/packages/Oogi2.AspNetCore3.Identity/)
[![Visual Studio Team services](https://img.shields.io/vso/build/frohikey/c3964e53-4bf3-417a-a96e-661031ef862f/128.svg?style=flat-square)](https://github.com/goto10hq/Oogi2.AspNetCore3.Identity)
[![netcoreapp6.0](https://img.shields.io/badge/.NETCoreApp-6.0-blue.svg)](https://github.com/dotnet/core)

ASP.NET Core 6.0 Identity with DocumentDB (CosmosDB) as database layer. It is powered by [Oogi2](https://github.com/goto10hq/Oogi2).
It's special fork that targets ASP.NET Core 6.0.

# Configuration

```csharp
public void ConfigureServices(IServiceCollection services)
{
 var connection = new Connection
 (
  Configuration["endpoint"], 
  Configuration["authorizationKey"], 
  Configuration["database"], 
  Configuration["collection"]
 );
 
 services.AddSingleton<IConnection>(connection);

 services.AddIdentity<ApplicationUser, ApplicationRole>()
 .AddDocumentDbStores()
 // I've prepared Czech localization
 //.AddErrorDescriber<CzechIdentityErrorDescriber>()
 // And English which is not needed... just for kicks
 //.AddErrorDescriber<EnglishIdentityErrorDescriber>()
 .AddDefaultTokenProviders();
}
```

If you don't need any own properties for user/role, you can just use ``IdentityUser`` and ``IdentityRole``.
In our example we extended them with some funky properties:

```csharp
[EntityType("entity", "oogi2/user")]
public class ApplicationUser : IdentityUser<ApplicationRole>
{
 public int NumberOfNumbers { get; set; }
}

[EntityType("entity", "oogi2/role")]
public class ApplicationRole : IdentityRole
{
 public bool IsClever { get; set; }
}
```

If you wanna know more about ``EntityType`` attribute, check [Oogi2](https://github.com/goto10hq/Oogi2).

And voila... that's all. EZPZ.

## Acknowledgement

Based on [AspNetCore.Identity.DocumentDb](https://github.com/codekoenig/AspNetCore.Identity.DocumentDb) by [codekoenig](https://github.com/codekoenig)

## License

MIT © [frohikey](http://frohikey.com) / [Goto10 s.r.o.](http://www.goto10.cz)
