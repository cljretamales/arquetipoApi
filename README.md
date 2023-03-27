# ```IMPLEMENTACION Y CONFIGURACION ARQUETIPO API .NET 6``` #

 <p align="left">
   <img src="https://img.shields.io/badge/STATUS-EN%20DESAROLLO-green">
</p>

***

# ```Contenido``` #
1. [Requisitos Previos](#requisitos-previos)
2. [Creación Proyecto WebApi Clean](#creación-proyecto-webapi-clean)
3. [Distribución Carpetas en Arquetipo.Api](#distribución-carpetas-arquetipo-api)
4. [Configuracion Proyecto Arquetipo.Api.csproj](#configuracion-proyecto-arquetipo-api-csproj)
5. [Agregar Archivo Seguridad HeaderValidationAttribute](#agregar-archivo-seguridad-headerValidationAttribute)

***
## **```Requisitos Previos```**

Para crear una Web API en .NET Core 6 en Windows, debes cumplir con los siguientes requisitos previos:

1. **Instalar ```.NET Core 6 SDK```:** Debes instalar el ```.NET Core 6 SDK``` en tu máquina. Puedes descargarlo desde el sitio web oficial de .NET: https://dotnet.microsoft.com/download/dotnet/6.0. Asegúrate de seleccionar el instalador adecuado para tu versión de Windows (x64 o ARM64).

2. **Instalar ```Visual Studio Code```:** Para desarrollar aplicaciones en ```.NET Core 6```, necesitarás un entorno de desarrollo. Puedes utilizar ```Visual Studio Code```, que es multiplataforma y más ligero que Visual Studio 2022. Descarga e instala desde la siguiente ubicación:
    * **Visual Studio Code: https://code.visualstudio.com/download**

    También deberás instalar la **```extensión "C#"```** para obtener soporte para el ```lenguaje C#``` y las herramientas de ```.NET```: https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp

***
## **```Creación Proyecto WebApi Clean```**

Abra la terminal (```línea de comandos```, ```PowerShell```, etc.) y navegue hasta la carpeta donde desea crear la solución y los proyectos.

* Ejecute el siguiente comando para crear una nueva solución llamada **```Arquetipo```**:

```C#
dotnet new sln -n Arquetipo
```

* A continuación, cree tres carpetas para los proyectos :

```C#
mkdir Arquetipo.Api
mkdir Arquetipo.Tests
mkdir Arquetipo.IntegrationTests
```

Navegue a cada una de las carpetas recién creadas y cree los proyectos correspondientes :

```C#
cd Arquetipo.Api
dotnet new webapi -n Arquetipo.Api
cd ..

cd Arquetipo.Tests
dotnet new xunit -n Arquetipo.Tests
cd ..

cd Arquetipo.IntegrationTests
dotnet new xunit -n Arquetipo.IntegrationTests
cd ..
```

Agregue los proyectos a la solución y establezca las relaciones entre ellos :

```C#
dotnet sln Arquetipo.sln add Arquetipo.Api/Arquetipo.Api.csproj
dotnet sln Arquetipo.sln add Arquetipo.Tests/Arquetipo.Tests.csproj
dotnet sln Arquetipo.sln add Arquetipo.IntegrationTests/Arquetipo.IntegrationTests.csproj

cd Arquetipo.Tests dotnet add reference ../Arquetipo.Api/Arquetipo.Api.csproj
cd ..

cd Arquetipo.IntegrationTests dotnet add reference ../Arquetipo.Api/Arquetipo.Api.csproj
cd ..
```

Siguiendo estos pasos, se creará una solución con tres proyectos : **```"Arquetipo.Api.csproj"```** (Web API), **```"Arquetipo.Tests.csproj"```** (pruebas unitarias con xUnit) y **```"Arquetipo.IntegrationTests"```** (pruebas de integración con xUnit). Además, se establecerán las relaciones entre los proyectos, permitiendo ejecutar pruebas que dependen del proyecto ```Web API```.

***
## **Distribución Carpetas ```Arquetipo Api```**

En el siguiente esquema se muestra la distribución de ```carpetas``` y ```namespaces``` para un proyecto ```Web API``` en ```.Net6```

Aca encontraras la definicion y la funcionalidad de la palabra reservada [**namespace**](https://learn.microsoft.com/es-es/dotnet/csharp/language-reference/language-specification/namespaces)

```C++
Arquetipo
│   .gitignore
│   README.md
│   Arquetipo.sln
│
│
└──────Arquetipo.Api
        │   namespace Arquetipo.Api
        │   Arquetipo.Api.csproj
        │   Program.cs
        │   Startup.cs
        │
        ├───Controllers
        │       namespace Arquetipo.Api.Controllers
        │       WeatherForecastController.cs
        │       ProductsController.cs
        │
        ├───Configuration
        │       namespace Arquetipo.Api.Configuration
        │       ConfigureSwaggerOptions.cs
        │       // Aquí van las clases relacionadas con la configuración, como la configuración de servicios, RabbitMQ, etc.
        │
        ├───Handlers
        │       namespace Arquetipo.Api.Handlers
        │       // Aquí van los orquestadores entre la capa controller y otras capas Repository, Services, etc.
        │
        ├───Infrastructure
        │       namespace Arquetipo.Api.Infrastructure
        │           ├────Contexts
        │           │       namespace Arquetipo.Api.Infrastructure.Contexts
        │           │       DbNameDatabaseContext.cs
        │           │       // aquí van las clases relacionadas con la configuración de EF y herecias de DbContext.
        │       TableRepository.cs
        │       ITableRepository.cs // interface que expone los metodos de TableRepository.
        │       Table2Repository.cs
        │       ITable2Repository.cs
        │       // Aquí van las clases relacionadas con la infraestructura, como repositorios, acceso a datos, etc.
        │       // Cada repository expone CRUD de una sola tabla o contexto.
        │
        ├───Models
        │       namespace Arquetipo.Api.Models
        │       ├───Headers
        │       │       namespace Arquetipo.Api.Models.Headers
        │       │       HeaderBase.cs
        │       │       // Aqui va la clase Header base para las respuesta (Body) de los servicios Rest.
        │       │
        │       ├───Requests
        │       │       namespace Arquetipo.Api.Models.Requests
        │       │       ItemRequest.cs
        │       │       // Aqui van las clases Requests de los servicios (Input metodos Controller).
        │       │
        │       ├───Reponses
        │       │       namespace Arquetipo.Api.Models.Responses
        │       │       ResultadoResponse.cs
        │       │       GetResponse.cs
        │       │       // Aqui van las clases de return de los metodos de los Controllers.
        │       │
        │       WeatherForecast.cs
        │
        ├───Security
        │       namespace Arquetipo.Api.Security
        │       HeaderValidationAttribute.cs
        │       // Aquí van las clases relacionadas con la seguridad, como la autenticación, autorización, etc.
        │
        └───Services
                namespace Arquetipo.Api.Controllers.Services
                IWeatherForecastService.cs  // interface que expone los metodos de la clase
                                           // WeatherForecastService(inyeccion de dependencia).
                WeatherForecastService.cs
                // Aqui van las clases relacionadas con el consumo de servicios ApiRest de otras Apis.
```
***

## **Configuracion Proyecto ```Arquetipo Api csproj```**

Agruegue los siguientes elementos dentro de la etiqueta ```<PropertyGroup>``` en el archivo ```Arquetipo.Api.csproj``` y luego **```"Guarde"```** los cambios ingresados en el archivo ```*.csproj```:

```XML
<PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
    <RootNamespace><Arquetipo.Api></Arquetipo></RootNamespace>
    <Nullable>disable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
</PropertyGroup>
```

* **```TargetFramework```** : Especifica la versión de ```.NET``` que se utilizará para ```compilar``` y ```ejecutar``` el proyecto
* **```RootNamespace```** : Define el espacio de nombres raíz predeterminado para el proyecto.
* **```Nullable```** : Indica que las anotaciones de ```referencia nula``` están ```deshabilitadas```.
* **```ImplicitUsings```** : Habilita los ```"usings"``` implícitos en el proyecto.
* **```GenerateDocumentationFile```** : Indica que el compilador generará un archivo de ```documentación XML``` para el proyecto. Este archivo contiene información sobre las ```clases```, ```métodos``` y ```propiedades``` del ```Arquetipo.Api```, basada en los ```comentarios``` del código. Es **```"REQUERIDO"```** para la ```Documentacion de los EnPoints``` del ```Arquetipo.Api``` en ```Swagger```.
* ```NoWarn``` : Suprime las advertencias del compilador asociadas con el código de advertencia ```CS1591```. La advertencia se produce cuando los elementos públicos o protegidos del código ```no tienen``` comentarios de ```documentación XML```.

***

A Continuacón se deben agregar  **```packages Nugget```** que necesita el proyecto para su correcto funcionamiento.
Para esto existe dos forma, la primera es instalando los packages por terminal y la otra es modificando el archivo ```Arquetipo.Api.csproj```.

1. Instala los siguientes packegs desde tu terminal ejecutando los siguientes scripts en la raiz del proyecto ```Arquetipo.Api```:

***

* [**```Microsoft.AspNetCore.Mvc.Versioning```**](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Versioning/) : Este paquete proporciona funcionalidades de ```control de versiones``` para las ```API```. Permite manejar diferentes ```versiones``` de una ```API``` de manera más organizada y estructurada. Para instalar ejecute el siguiente script:

```C#
dotnet add package Microsoft.AspNetCore.Mvc.Versioning --version 5.0.0
```

***

* [**```Microsoft.AspNetCore.Mvc.Versioning.ApiExplorer```**](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Versioning.ApiExplorer) : Este paquete complementa al paquete de ```control de versiones``` mencionado anteriormente y proporciona compatibilidad con ```API Explorer``` para las ```API``` de ```ASP.NET Core``` con ```versionamiento```. Para instalar ejecute el siguiente script:

```C#
dotnet add package Microsoft.AspNetCore.Mvc.Versioning.ApiExplorer --version 5.0.0
```

***

* [**```Microsoft.Extensions.Logging.EventLog```**](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/) : Este paquete proporciona un ```proveedor de registro``` para el ```registro de eventos de Windows```. Permite a las aplicaciones de ```.NET Core``` registrar eventos en el ```registro de eventos de Windows```. Para instalar ejecute el siguiente script:

```C#
dotnet add package Microsoft.Extensions.Logging.EventLog --version 7.0.0
```

***

* [**```NLog.Web.AspNetCore```**](https://www.nuget.org/packages/NLog.Web.AspNetCore) : ```NLog``` es un popular marco de registro para ```.NET```. Este paquete proporciona integración entre ```NLog``` y ```ASP.NET Core```, lo que facilita el uso de ```NLog``` en aplicaciones ```web``` de ```.NET Core```. Para instalar ejecute el siguiente script:

```C#
dotnet add package NLog.Web.AspNetCore --version 5.2.2
```

***

* [**```Swashbuckle.AspNetCore```**](https://www.nuget.org/packages/Swashbuckle.AspNetCore#supportedframeworks-body-tab) : ```Swashbuckle``` es una herramienta que genera automáticamente ```documentación de API``` y ```UI de Swagger``` para aplicaciones ```web API``` de ```ASP.NET Core```. Facilita la exploración y el consumo de las ```APIs``` por parte de otros desarrolladores. Para instalar ejecute el siguiente script:

```C#
dotnet add package Swashbuckle.AspNetCore --version 6.2.3
```

***

* [**```Microsoft.AspNetCore.Authentication.JwtBearer```**](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.JwtBearer/8.0.0-preview.2.23153.2) : Este paquete proporciona ```middleware``` para la autenticación de ```tokens JWT``` (```JSON Web Tokens```) en aplicaciones web de ```.NET Core```. Facilita la implementación de la autenticación basada en ```tokens JWT``` en la ```API```. Para instalar ejecute el siguiente script:

```C#
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer --version 6.0.0
```


**```Nota```**: las versiones packages utilizadas en este documento son **latest** por ende existe la posibilidad que pueden variar cuando lea este documento.

***

2. Abra el archivo **```Arquetipo.Api.csproj```** y agregue las siguientes referencias de ```packages``` al proyecto dentro de la etiqueta **```"<ItemGroup>"```** y ```"Guarde"``` los cambios realizados en el archivo.

```XML
<ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Versioning" Version="5.0.0" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Versioning.ApiExplorer" Version="5.0.0" />
    <PackageReference Include="Microsoft.Extensions.Logging.EventLog" Version="7.0.0" />
    <PackageReference Include="NLog.Web.AspNetCore" Version="5.2.2" />
    <PackageReference Include="Swashbuckle.AspNetCore" Version="6.2.3" />
    <PackageReference Include="Microsoft.AspNetCore.Authentication.JwtBearer" Version="6.0.0" />
  </ItemGroup>
```

* Si realizo los pasos de forma corercta en el punto anterior 1 o 2, el archivo **```"Arquetipo.Api.csproj"```** deberia tener la suiguiente configuración:

```XML
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
    <RootNamespace>Arquetipo.Api</RootNamespace>
    <Nullable>disable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <GenerateDocumentationFile>true</GenerateDocumentationFile>
    <NoWarn>$(NoWarn);1591</NoWarn>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Versioning" Version="5.0.0" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Versioning.ApiExplorer" Version="5.0.0" />
    <PackageReference Include="Microsoft.Extensions.Logging.EventLog" Version="7.0.0" />
    <PackageReference Include="NLog.Web.AspNetCore" Version="5.2.2" />
    <PackageReference Include="Swashbuckle.AspNetCore" Version="6.2.3" />
    <PackageReference Include="Microsoft.AspNetCore.Authentication.JwtBearer" Version="6.0.0" />
  </ItemGroup>

</Project>
```

* Para finalizar ejecute el siguiente ```script``` en la ```terminal ```de la ruta base donde se encuentra el proyecto (```*.csproj```) o solucion (```*.sln```), asi restaurar e instalar los packeges ```nugguets```:

```C#
dotnet restore
```

***

## **Agregar Archivo Seguridad ```HeaderValidationAttribute```**

Para **```segurizar```** los EndPoints debemos crear dentro de la carpeta ```Security``` (si no existe se debe crear) un archivo con el nombre **```"HeaderValidationAttribute.cs"```**, una vez creado agregue el siguiente codigo y **```"Guarde"```** los cambios :

```C#
// Importaciones de espacios de nombres necesarios
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.Filters;

// Define un espacio de nombres personalizado
namespace Arquetipo.Api.Security;

// La clase HeaderValidationAttribute hereda de ActionFilterAttribute
public class HeaderValidationAttribute : ActionFilterAttribute
{
    // Lista de nombres de encabezados requeridos
    private readonly List<string> _headerNames;

    // Constructor de la clase
    public HeaderValidationAttribute()
    {
        // Carga los nombres de los encabezados requeridos desde una variable de entorno
        // y los divide en una lista, eliminando espacios en blanco
        var headersFromEnv = Environment.GetEnvironmentVariable("REQUIRED_HEADERS");
        if (headersFromEnv != null)
        {
            _headerNames = headersFromEnv.Split(',').Select(h => h.Trim()).ToList();
        }
        else
        {
            _headerNames = new List<string>();
        }

    }

    // Sobrescribe el método OnActionExecuting del filtro de acción
    public override void OnActionExecuting(ActionExecutingContext context)
    {
        // Verifica si todos los encabezados requeridos están presentes en la solicitud
        if (_headerNames.Count > 0 && !_headerNames.All(h => context.HttpContext.Request.Headers.ContainsKey(h)))
        {
            // Si falta alguno de los encabezados requeridos, devuelve un resultado de BadRequest
            context.Result = new BadRequestObjectResult($"One or more required headers are missing.");
            return;
        }

        // Valida que el encabezado "X-Custom-Header1" tenga el valor "resultado esperado"
        if (context.HttpContext.Request.Headers.TryGetValue("X-Custom-Header1", out var customHeaderValue) && customHeaderValue != "resultado esperado")
        {
            context.Result = new BadRequestObjectResult($"The value of X-Custom-Header1 is not valid.");
            return;
        }

        // Aquí puedes agregar más validaciones, si es necesario.

        // Llama al método base OnActionExecuting
        base.OnActionExecuting(context);
    }
}
```
A continuacion un ejemplo de implementacion en un ```Controller```.

```C#
using Microsoft.AspNetCore.Mvc;
using Arquetipo.Api.Security; // Asegúrate de importar el espacio de nombres donde se encuentra el atributo

namespace Arquetipo.Api.Controllers;

[ApiController]
[Route("api/[controller]")]
public class MyController : ControllerBase
{
    // Aplica el atributo HeaderValidationAttribute al nivel del método
    [HttpGet]
    [HeaderValidation]
    public IActionResult Get()
    {
        // ... lógica de tu acción GET
        return Ok("Ejemplo de validación de encabezados.");
    }

    // También puedes aplicar el atributo a nivel de clase, lo que afectará a todos los métodos del controlador
    // [HeaderValidation]

    // Otros métodos del controlador...
}
```

* En este ejemplo, el atributo ```HeaderValidation``` se aplica al método Get() del controlador. Cuando se realiza una solicitud a la ruta del método Get(), el filtro de acción ```HeaderValidationAttribute``` se ejecutará antes de que se ejecute el método, verificando la presencia de los encabezados requeridos en la ```solicitud HTTP```. Si alguno de los encabezados requeridos falta, se devolverá un resultado de ```BadRequest (400)```.

* Las **variables de ambientes** se puede configurar en el archivo "appsettings.Development.json" o "launch.json" en Visual Code :

  1. ejemplo **appsettings.json** :

    ```json
    {
      "Logging": {
        "LogLevel": {
          "Default": "Information",
          "Microsoft.AspNetCore": "Warning"
        }
      },
      "AllowedHosts": "*",
      "REQUIRED_HEADERS": "X-Custom-Header1, X-Custom-Header2"
    }
    ```

  2. ejemplo **launch.json** :

    ```json
    {
    "version": "0.2.0",
    "configurations": [
        {
            "name": ".NET Core Launch (web)",
            "type": "coreclr",
            "request": "launch",
            "preLaunchTask": "build",
            "program": "${workspaceFolder}/bin/Debug/net6.0/Arquetipo.Api.dll",
            "args": [],
            "cwd": "${workspaceFolder}",
            "stopAtEntry": false,
            "serverReadyAction": {
                "action": "openExternally",
                "pattern": "\\bNow listening on:\\s+(https?://\\S+)"
            },
            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development",
                "REQUIRED_HEADERS": "X-Custom-Header1, X-Custom-Header2"
            },
            "sourceFileMap": {
                "/Views": "${workspaceFolder}/Views"
            }
        },
        {
            "name": ".NET Core Attach",
            "type": "coreclr",
            "request": "attach"
        }
      ]
    }
    ```
***
## Agrgar Archivo Configuracion ConfigureSwaggerOptions

La clase ```ConfigureSwaggerOptions``` configura las opciones de ```SwaggerGen``` en una aplicación ```ASP.NET Core.``` A continuacion crea dentro de la carpeta **```"Configuration"```** el archivo **ConfigureSwaggerOptions.cs**

Copia y guarda el siguiente codigo en el archivo ```ConfigureSwaggerOptions.cs```

```C#
using System.Reflection;
using Microsoft.AspNetCore.Mvc.ApiExplorer;
using Microsoft.Extensions.Options;
using Microsoft.OpenApi.Models;
using Swashbuckle.AspNetCore.SwaggerGen;

namespace Arquetipo.Api.Configuration;
// La clase ConfigureSwaggerOptions implementa la interfaz IConfigureNamedOptions<SwaggerGenOptions>
// para configurar las opciones de SwaggerGen en una aplicación ASP.NET Core.
public class ConfigureSwaggerOptions : IConfigureNamedOptions<SwaggerGenOptions>
{
  private readonly IApiVersionDescriptionProvider _provider;

  // El constructor recibe un objeto IApiVersionDescriptionProvider para obtener información
  // sobre las versiones de la API.
  public ConfigureSwaggerOptions(IApiVersionDescriptionProvider provider)
  {
    _provider = provider;
  }

  // El método Configure se utiliza para configurar las opciones de SwaggerGen.
  public void Configure(SwaggerGenOptions options)
  {
    // Configurar la definición de seguridad para el esquema Bearer JWT.
    options.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme()
    {
      Name = "Authorization",
      Type = SecuritySchemeType.ApiKey,
      Scheme = "Bearer",
      BearerFormat = "JWT",
      In = ParameterLocation.Header,
      Description = "JWT Authorization header using the Bearer scheme. \r\n\r\n Enter 'Bearer' [space] and then your token in the text input below.\r\n\r\nExample: \"Bearer 12345abcdef\"",
    });

    // Agregar el requisito de seguridad para el esquema Bearer JWT.
    options.AddSecurityRequirement(new OpenApiSecurityRequirement
    {
      {
        new OpenApiSecurityScheme
          {
            Reference = new OpenApiReference
              {
                Type = ReferenceType.SecurityScheme,
                Id = "Bearer"
              }
          },
          Array.Empty<string>()
      }
    });

    // Generar documentos de Swagger para cada versión de la API.
    foreach (var description in _provider.ApiVersionDescriptions)
    {
      options.SwaggerDoc(
        description.GroupName,
        CreateVersionInfo(description));

      var xmlFile = $"{Assembly.GetExecutingAssembly().GetName().Name}.xml";
      var xmlPath = Path.Combine(AppContext.BaseDirectory, xmlFile);
      options.IncludeXmlComments(xmlPath);
    }

  }

  // Este método sobrecargado permite configurar las opciones de SwaggerGen con un nombre específico.
  public void Configure(string name, SwaggerGenOptions options)
  {
    Configure(options);
  }

  // Este método privado crea un objeto OpenApiInfo a partir de una descripción de versión de API.
  private static OpenApiInfo CreateVersionInfo(ApiVersionDescription description)
  {
    var info = new OpenApiInfo()
    {
      Title = "Arquetipo Api",
      Version = description.ApiVersion.ToString()
    };

    if (description.IsDeprecated)
    {
      info.Description += " This API version has been deprecated.";
    }

    return info;
  }
}
```
* **```nota```** : la configuración de esta ckase la haremos desde el archico **```Startup.cs```** que veremos en los siguientes pasos.

# Configuración ```Program``` y Creación ```Startup```  #

Remplece el codigo de Program.cs por el siguiente codigo:


```C#
using Microsoft.Extensions.Logging.EventLog;
using Microsoft.Extensions.Logging;
using NLog.Extensions.Logging;
using NLog.Web;
using BciSeguros.MS;
using Microsoft.AspNetCore.Mvc.ApiExplorer;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

// Configuring the logging system
builder.Host.ConfigureLogging((hostingContext, logging) =>
{
    var environmentName = hostingContext.HostingEnvironment.EnvironmentName;

    // Clear existing logging providers
    logging.ClearProviders();

    // Check if the environment is "Development"
    if (environmentName.Equals("Development", StringComparison.OrdinalIgnoreCase))
    {
        // Add Console logging provider
        logging.AddConsole();

        // Add NLog logging provider with configuration from appsettings.json
        logging.AddNLog(hostingContext.Configuration.GetSection("NLog"));

        // Add EventLog logging provider with custom source and log names
        logging.AddEventLog(new EventLogSettings
        {
            SourceName = "ArquetipoMs",
            LogName = "ArquetipoMS"
        });
    }
    else
    {
        // Add Console logging provider
        logging.AddConsole();

        // Uncomment the following line to add NLog logging provider in non-development environments
        // logging.AddNLog(hostingContext.Configuration.GetSection("NLog"));
    }
});

var startup = new Startup(builder.Configuration);

startup.ConfigureServices(builder.Services);

var app = builder.Build();

var provider = app.Services.GetRequiredService<IApiVersionDescriptionProvider>();

startup.Configure(app, app.Environment, provider);

app.Run();
```

* A continuación una descripción del código:

1. Se crea un objeto builder de tipo WebApplication.Builder utilizando el método WebApplication.CreateBuilder(args).
2. Se configura el registro mediante el método builder.Host.ConfigureLogging(). Se utiliza un delegado para configurar el registro en función del entorno de la aplicación. Si el entorno es "Development", se agregan los proveedores de registro de consola, NLog y EventLog. Si no, solo se agrega el registro de consola.
3. Se crea una instancia de la clase Startup y se le pasa la configuración del constructor para su posterior configuración.
4. Se llama al método startup.ConfigureServices(builder.Services) para configurar los servicios requeridos en la aplicación.
5. Se construye la aplicación con el método app = builder.Build().
6. Se obtiene el proveedor de descripción de versión de API utilizando app.Services.GetRequiredService<IApiVersionDescriptionProvider>().
7. Se llama al método startup.Configure(app, app.Environment, provider) para configurar la aplicación, sus entornos y el proveedor de descripción de la versión de la API.
8. Finalmente, se ejecuta la aplicación con app.Run().

El código en sí es muy limpio y fácil de entender. La implementación del registro es flexible, ya que se basa en el entorno de la aplicación para decidir qué proveedores de registro usar. En este caso, se emplea el registro en consola, NLog y EventLog, lo que garantiza un registro detallado durante el desarrollo y un registro más simple en otros entornos.

* La clase Startup se utiliza para configurar los servicios y la aplicación en sí.

***

* Ejemplo de configuración de NLog en el archivo appsettings.json.

```json
{
  "NLog": {
    "autoReload": true,
    "throwConfigExceptions": true,
    "internalLogLevel": "info",
    "internalLogFile": "internal_logs/internal-nlog.txt",
    "targets": {
      "file": {
        "type": "File",
        "fileName": "logs/app-log-${shortdate}.log",
        "layout": "${longdate} ${uppercase:${level}} ${logger} - ${message}${exception:format=tostring}",
        "archiveAboveSize": 1048576,
        "maxArchiveFiles": 7
      }
    },
    "rules": [
      {
        "logger": "*.Controllers.*",
        "minlevel": "Info",
        "writeTo": "file"
      },
      {
        "logger": "*.Handlers.*",
        "minlevel": "Info",
        "writeTo": "file"
      },
      {
        "logger": "*.Repositories.*",
        "minlevel": "Info",
        "writeTo": "file"
      }
    ]
  },

  // ... other appsettings configurations
}

```

### Contribution guidelines ###

* Writing tests
* Code review
* Other guidelines

### Who do I talk to? ###

* Repo owner or admin
* Other community or team contact
