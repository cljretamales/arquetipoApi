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
5. [Agregar Clase Seguridad HeaderValidationAttribute](#agregar-clase-seguridad-headervalidationattribute)

***
## ```Requisitos Previos```

Para crear una Web API en .NET Core 6 en Windows, debes cumplir con los siguientes requisitos previos:

1. Instalar ```.NET Core 6 SDK```: Debes instalar el ```.NET Core 6 SDK``` en tu máquina. Puedes descargarlo desde el sitio web oficial de .NET: https://dotnet.microsoft.com/download/dotnet/6.0. Asegúrate de seleccionar el instalador adecuado para tu versión de Windows (x64 o ARM64).

2. Instalar ```Visual Studio Code```: Para desarrollar aplicaciones en ```.NET Core 6```, necesitarás un entorno de desarrollo. Puedes utilizar ```Visual Studio Code```., que es multiplataforma y más ligero que Visual Studio 2022. Descarga e instala desde la siguiente ubicación:
    * Visual Studio Code: https://code.visualstudio.com/download

    También deberás instalar la ```extensión "C#"``` para obtener soporte para el ```lenguaje C#``` y las herramientas de ```.NET```: https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp

***
## ```Creación Proyecto WebApi Clean```

Abra la terminal (```línea de comandos```, ```PowerShell```, etc.) y navegue hasta la carpeta donde desea crear la solución y los proyectos.

Ejecute el siguiente comando para crear una nueva solución llamada ```Arquetipo```:

```C#
dotnet new sln -n Arquetipo
```

A continuación, cree tres carpetas para los proyectos:

```C#
mkdir Arquetipo.Api
mkdir Arquetipo.Tests
mkdir Arquetipo.IntegrationTests
```

Navegue a cada una de las carpetas recién creadas y cree los proyectos correspondientes:

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

Agregue los proyectos a la solución y establezca las relaciones entre ellos:
```C#
dotnet sln Arquetipo.sln add Arquetipo.Api/Arquetipo.Api.csproj
dotnet sln Arquetipo.sln add Arquetipo.Tests/Arquetipo.Tests.csproj
dotnet sln Arquetipo.sln add Arquetipo.IntegrationTests/Arquetipo.IntegrationTests.csproj

cd Arquetipo.Tests dotnet add reference ../Arquetipo.Api/Arquetipo.Api.csproj
cd ..

cd Arquetipo.IntegrationTests dotnet add reference ../Arquetipo.Api/Arquetipo.Api.csproj
cd ..
```

Siguiendo estos pasos, se creará una solución con tres proyectos: ```Arquetipo.Api``` (Web API), ```Arquetipo.Tests``` (pruebas unitarias con xUnit) y ```Arquetipo.IntegrationTests``` (pruebas de integración con xUnit). Además, se establecerán las relaciones entre los proyectos, permitiendo a los desarrolladores ejecutar pruebas que dependen del proyecto Web API.

***
## Distribución Carpetas ```Arquetipo Api```

En el siguiente esquema se muestra la distribución de ```carpetas``` y ```namespaces``` para un proyecto ```Web API``` en ```.Net6```

Aca encontraras la definicion y la funcionalidad de la palabra reservada [namespace](https://learn.microsoft.com/es-es/dotnet/csharp/language-reference/language-specification/namespaces)

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

## Configuracion Proyecto ```Arquetipo Api csproj```

Agruegue los siguientes elementos dentro de la etiqueta ```<PropertyGroup>``` en el archivo ```Arquetipo.Api.csproj``` y luego ```"Guarde"``` los cambios ingresados en el archivo ```*.csproj```:

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

* ```TargetFramework``` : Especifica la versión de ```.NET``` que se utilizará para ```compilar``` y ```ejecutar``` el proyecto
* ```RootNamespace``` : Define el espacio de nombres raíz predeterminado para el proyecto.
* ```Nullable``` : Indica que las anotaciones de ```referencia nula``` están ```deshabilitadas```.
* ```ImplicitUsings``` : Habilita los ```"usings"``` implícitos en el proyecto.
* ```GenerateDocumentationFile``` : Indica que el compilador generará un archivo de ```documentación XML``` para el proyecto. Este archivo contiene información sobre las ```clases```, ```métodos``` y ```propiedades``` del ```Arquetipo.Api```, basada en los ```comentarios``` del código. Es ```"REQUERIDO"``` para la ```Documentacion de los EnPoints``` del ```Arquetipo.Api``` en ```Swagger```.
* ```NoWarn``` : Suprime las advertencias del compilador asociadas con el código de advertencia ```CS1591```. La advertencia se produce cuando los elementos públicos o protegidos del código ```no tienen``` comentarios de ```documentación XML```.

A Continuacón se deben agregar los ```packages``` de ```Nugget``` que necesita el proyecto para su correcto funcionamiento.
Para esto existe dos forma, la primera es instalando los packages por terminal y la otra es modificando el archivo ```Arquetipo.Api.csproj```.

1. Instala los siguientes packegs desde tu terminal ejecutando los siguientes scripts en la raiz del proyecto ```Arquetipo.Api```:

* ```Microsoft.AspNetCore.Mvc.Versioning```: Este paquete proporciona funcionalidades de ```control de versiones``` para las ```API```. Permite manejar diferentes ```versiones``` de una ```API``` de manera más organizada y estructurada. Para instalar ejecute el siguiente script:
```C#
dotnet add package Microsoft.AspNetCore.Mvc.Versioning
```

* ```Microsoft.AspNetCore.Mvc.Versioning.ApiExplorer``` : Este paquete complementa al paquete de ```control de versiones``` mencionado anteriormente y proporciona compatibilidad con ```API Explorer``` para las ```API``` de ```ASP.NET Core``` con ```versionamiento```. Para instalar ejecute el siguiente script:

```C#
dotnet add package Microsoft.AspNetCore.Mvc.Versioning.ApiExplorer
```

*

## Agregar Clase Seguridad ```HeaderValidationAttribute```

Para validar en


# Configuración ```Program``` y Creación ```Startup```  #

Remplece el codigo de Program.cs por el siguiente codigo:


```C#
using Microsoft.Extensions.Logging.EventLog;
using Microsoft.Extensions.Logging;
using NLog.Extensions.Logging;
using NLog.Web;
using Arquetipo.MS;
using Microsoft.AspNetCore.Mvc.ApiExplorer;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

builder.Host.ConfigureLogging((hostingContext, logging) =>
{
    var environmentName = hostingContext.HostingEnvironment.EnvironmentName;

    logging.ClearProviders();

    if (environmentName.Equals("Development", StringComparison.OrdinalIgnoreCase))
    {
        logging.AddConsole();
        logging.AddNLog(hostingContext.Configuration.GetSection("NLog"));
        // logging.AddEventLog(new EventLogSettings
        // {
        //     SourceName = "ArquetipoMs",
        //     LogName = "ArquetipoMS"
        // });
    }
    else
    {
        logging.AddConsole();
        logging.AddNLog(hostingContext.Configuration.GetSection("NLog"));
    }
});

var startup = new Startup(builder.Configuration);

startup.ConfigureServices(builder.Services);

var app = builder.Build();

var provider = app.Services.GetRequiredService<IApiVersionDescriptionProvider>();

startup.Configure(app, app.Environment, provider);

app.Run();
```
Implementaciones:

* ```builder.Host.ConfigureLogging``` : implemtacion multiples clientes Log.
* ```logging.ClearProviders()``` : limpia posibles clientes de Log.
* ```logging.AddConsole();``` : se iniciliza cliente Log de Consola.
* ```logging.AddNLog();``` : se iniciliza cliente Log NLog.
* ```logging.AddNLog();``` : se iniciliza cliente Log NLog.

### Contribution guidelines ###

* Writing tests
* Code review
* Other guidelines

### Who do I talk to? ###

* Repo owner or admin
* Other community or team contact
