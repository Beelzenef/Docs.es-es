---
title: Abrir la interfaz Web para .NET (OWIN)
author: ardalis
description: "Introducción a abrir la interfaz Web para .NET (OWIN)."
keywords: "Núcleo de ASP.NET, la interfaz Web abierta para. NET, OWIN"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 796d075d4d0c6b888be4fc2225fc482acdbad498
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="a33ce-104">Introducción a abrir la interfaz Web para .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="a33ce-104">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="a33ce-105">Por [Steve Smith](https://ardalis.com/) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a33ce-105">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a33ce-106">ASP.NET Core es compatible con la interfaz Web abierta para .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="a33ce-106">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="a33ce-107">OWIN permite que las aplicaciones web se desacople de servidores web.</span><span class="sxs-lookup"><span data-stu-id="a33ce-107">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="a33ce-108">Define un método estándar para middleware para usarse en una canalización para controlar las solicitudes y respuestas asociadas.</span><span class="sxs-lookup"><span data-stu-id="a33ce-108">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="a33ce-109">El software intermedio y las aplicaciones de ASP.NET Core pueden interoperar con middleware, servidores y aplicaciones basadas en OWIN.</span><span class="sxs-lookup"><span data-stu-id="a33ce-109">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="a33ce-110">OWIN proporciona una capa de desacoplamiento que permite a dos marcos con modelos de objetos dispares para utilizarse conjuntamente.</span><span class="sxs-lookup"><span data-stu-id="a33ce-110">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="a33ce-111">El `Microsoft.AspNetCore.Owin` paquete proporciona dos implementaciones del adaptador:</span><span class="sxs-lookup"><span data-stu-id="a33ce-111">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="a33ce-112">Núcleo de ASP.NET para OWIN</span><span class="sxs-lookup"><span data-stu-id="a33ce-112">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="a33ce-113">OWIN a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a33ce-113">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="a33ce-114">Esto permite que ASP.NET Core se hospede en un servidor compatible con OWIN/host, o para otros componentes compatibles de OWIN para ejecutarse sobre ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a33ce-114">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="a33ce-115">Nota: Uso de estos adaptadores viene con un costo de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="a33ce-115">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="a33ce-116">Las aplicaciones que usan solo los componentes principales de ASP.NET no deben usar el paquete de Owin o adaptadores.</span><span class="sxs-lookup"><span data-stu-id="a33ce-116">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

[<span data-ttu-id="a33ce-117">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="a33ce-117">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="a33ce-118">Ejecuta el middleware de OWIN de la canalización ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a33ce-118">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="a33ce-119">Compatibilidad con las OWIN del núcleo de ASP.NET se implementa como parte de la `Microsoft.AspNetCore.Owin` paquete.</span><span class="sxs-lookup"><span data-stu-id="a33ce-119">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="a33ce-120">Puede importar compatibilidad con OWIN en el proyecto mediante la instalación de este paquete.</span><span class="sxs-lookup"><span data-stu-id="a33ce-120">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="a33ce-121">Middleware de OWIN que se ajusta a la [especificación de OWIN](http://owin.org/spec/spec/owin-1.0.0.html), lo que requiere un `Func<IDictionary<string, object>, Task>` interfaz y determinadas claves establecerse (como `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="a33ce-121">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="a33ce-122">El siguiente middleware de OWIN simple muestra "Hello World":</span><span class="sxs-lookup"><span data-stu-id="a33ce-122">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}


```

<span data-ttu-id="a33ce-123">La firma de ejemplo devuelve un `Task` y acepta un `IDictionary<string, object>` según lo solicitado por OWIN.</span><span class="sxs-lookup"><span data-stu-id="a33ce-123">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="a33ce-124">El código siguiente muestra cómo agregar la `OwinHello` middleware (se muestra arriba) a la canalización ASP.NET con la `UseOwin` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="a33ce-124">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/OwinSample/Startup.cs"} -->

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}


```

<span data-ttu-id="a33ce-125">Puede configurar otras acciones para tener lugar dentro de la canalización OWIN.</span><span class="sxs-lookup"><span data-stu-id="a33ce-125">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="a33ce-126">Encabezados de respuesta sólo deben modificarse antes de la primera escritura en la secuencia de respuesta.</span><span class="sxs-lookup"><span data-stu-id="a33ce-126">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="a33ce-127">Varias llamadas a `UseOwin` se desaconseja por motivos de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="a33ce-127">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="a33ce-128">Componentes de OWIN funcionará mejor si no se agrupan juntos.</span><span class="sxs-lookup"><span data-stu-id="a33ce-128">OWIN components will operate best if grouped together.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name=hosting-on-owin></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="a33ce-129">Uso de hospedaje de ASP.NET en un servidor basado en OWIN</span><span class="sxs-lookup"><span data-stu-id="a33ce-129">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="a33ce-130">Servidores basados en OWIN pueden hospedar aplicaciones ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a33ce-130">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="a33ce-131">Un servidor de este tipo es [Nowin](https://github.com/Bobris/Nowin), un servidor web de .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="a33ce-131">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="a33ce-132">En el ejemplo de este artículo, he incluido un proyecto que hace referencia a Nowin y lo usa para crear un `IServer` capaz de autohospedaje ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a33ce-132">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

<span data-ttu-id="a33ce-133">[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]</span><span class="sxs-lookup"><span data-stu-id="a33ce-133">[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]</span></span>

<span data-ttu-id="a33ce-134">`IServer`es una interfaz que requiera un `Features` propiedad y un `Start` método.</span><span class="sxs-lookup"><span data-stu-id="a33ce-134">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="a33ce-135">`Start`es responsable de configurar e iniciar el servidor, que en este caso, se realiza a través de una serie de llamadas API fluidas que establecer direcciones analizadas desde el IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="a33ce-135">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="a33ce-136">Tenga en cuenta que la configuración fluida de la `_builder` variable especifica que las solicitudes se controlarán mediante el `appFunc` definido anteriormente en el método.</span><span class="sxs-lookup"><span data-stu-id="a33ce-136">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="a33ce-137">Esto `Func` se llama en cada solicitud para procesar las solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="a33ce-137">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="a33ce-138">Agregaremos también un `IWebHostBuilder` extensión para que sea fácil de agregar y configurar el servidor Nowin.</span><span class="sxs-lookup"><span data-stu-id="a33ce-138">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [11], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/NowinWebHostBuilderExtensions.cs"} -->

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

<span data-ttu-id="a33ce-139">Teniendo esto en su lugar, todo lo que se necesita para ejecutar una aplicación de ASP.NET mediante este servidor personalizada para llamar a la extensión en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a33ce-139">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [15], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/Program.cs"} -->

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

<span data-ttu-id="a33ce-140">Obtener más información sobre ASP.NET [servidores](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="a33ce-140">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="a33ce-141">Ejecute ASP.NET Core en un servidor basado en OWIN y utilice su compatibilidad con WebSockets</span><span class="sxs-lookup"><span data-stu-id="a33ce-141">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="a33ce-142">Otro ejemplo de características de servidores basada en OWIN cómo se puede utilizar con ASP.NET Core es el acceso a características como WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a33ce-142">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="a33ce-143">El servidor de web .NET OWIN usado en el ejemplo anterior es compatible con Sockets Web integrada, que se pueden utilizar con una aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a33ce-143">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="a33ce-144">El ejemplo siguiente muestra una aplicación web simple que admite Sockets Web y vuelve de todo lo que se envían al servidor a través de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a33ce-144">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [7, 9, 10], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": true, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinWebSockets/Startup.cs"} -->

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

<span data-ttu-id="a33ce-145">Esto [ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) está configurado con el mismo `NowinServer` como lo anterior: la única diferencia es cómo está configurada la aplicación en su `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="a33ce-145">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="a33ce-146">Una prueba mediante [un cliente simple websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) se muestra la aplicación:</span><span class="sxs-lookup"><span data-stu-id="a33ce-146">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Cliente de prueba de Socket Web](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="a33ce-148">Entorno OWIN</span><span class="sxs-lookup"><span data-stu-id="a33ce-148">OWIN environment</span></span>

<span data-ttu-id="a33ce-149">Puede construir un entorno de OWIN utilizando el `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="a33ce-149">You can construct a OWIN environment using the `HttpContext`.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="a33ce-150">Claves OWIN</span><span class="sxs-lookup"><span data-stu-id="a33ce-150">OWIN keys</span></span>

<span data-ttu-id="a33ce-151">Depende de OWIN un `IDictionary<string,object>` objeto para comunicar la información a lo largo de un intercambio de solicitud/respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a33ce-151">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="a33ce-152">ASP.NET Core implementa las claves que se enumeran a continuación.</span><span class="sxs-lookup"><span data-stu-id="a33ce-152">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="a33ce-153">Consulte la [especificación principal, las extensiones de](http://owin.org/#spec), y [OWIN clave directrices y claves comunes](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="a33ce-153">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="a33ce-154">Datos de la solicitud (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="a33ce-154">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="a33ce-155">Key</span><span class="sxs-lookup"><span data-stu-id="a33ce-155">Key</span></span>               | <span data-ttu-id="a33ce-156">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="a33ce-156">Value (type)</span></span> | <span data-ttu-id="a33ce-157">Descripción</span><span class="sxs-lookup"><span data-stu-id="a33ce-157">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a33ce-158">owin. RequestScheme</span><span class="sxs-lookup"><span data-stu-id="a33ce-158">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="a33ce-159">owin. RequestMethod</span><span class="sxs-lookup"><span data-stu-id="a33ce-159">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="a33ce-160">owin. RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="a33ce-160">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="a33ce-161">owin. RequestPath</span><span class="sxs-lookup"><span data-stu-id="a33ce-161">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="a33ce-162">owin. RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="a33ce-162">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="a33ce-163">owin. RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="a33ce-163">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="a33ce-164">owin. RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="a33ce-164">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="a33ce-165">owin. RequestBody</span><span class="sxs-lookup"><span data-stu-id="a33ce-165">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="a33ce-166">Datos de la solicitud (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="a33ce-166">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="a33ce-167">Key</span><span class="sxs-lookup"><span data-stu-id="a33ce-167">Key</span></span>               | <span data-ttu-id="a33ce-168">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="a33ce-168">Value (type)</span></span> | <span data-ttu-id="a33ce-169">Descripción</span><span class="sxs-lookup"><span data-stu-id="a33ce-169">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a33ce-170">owin. RequestId</span><span class="sxs-lookup"><span data-stu-id="a33ce-170">owin.RequestId</span></span> | `String` | <span data-ttu-id="a33ce-171">Optional</span><span class="sxs-lookup"><span data-stu-id="a33ce-171">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="a33ce-172">Datos de respuesta (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="a33ce-172">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="a33ce-173">Key</span><span class="sxs-lookup"><span data-stu-id="a33ce-173">Key</span></span>               | <span data-ttu-id="a33ce-174">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="a33ce-174">Value (type)</span></span> | <span data-ttu-id="a33ce-175">Descripción</span><span class="sxs-lookup"><span data-stu-id="a33ce-175">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a33ce-176">owin. ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="a33ce-176">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="a33ce-177">Optional</span><span class="sxs-lookup"><span data-stu-id="a33ce-177">Optional</span></span> |
| <span data-ttu-id="a33ce-178">owin. ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="a33ce-178">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="a33ce-179">Optional</span><span class="sxs-lookup"><span data-stu-id="a33ce-179">Optional</span></span> |
| <span data-ttu-id="a33ce-180">owin. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="a33ce-180">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="a33ce-181">owin. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="a33ce-181">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="a33ce-182">Otros datos (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="a33ce-182">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="a33ce-183">Key</span><span class="sxs-lookup"><span data-stu-id="a33ce-183">Key</span></span>               | <span data-ttu-id="a33ce-184">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="a33ce-184">Value (type)</span></span> | <span data-ttu-id="a33ce-185">Descripción</span><span class="sxs-lookup"><span data-stu-id="a33ce-185">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a33ce-186">owin. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="a33ce-186">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="a33ce-187">owin. Versión</span><span class="sxs-lookup"><span data-stu-id="a33ce-187">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="a33ce-188">Claves comunes</span><span class="sxs-lookup"><span data-stu-id="a33ce-188">Common Keys</span></span>

| <span data-ttu-id="a33ce-189">Key</span><span class="sxs-lookup"><span data-stu-id="a33ce-189">Key</span></span>               | <span data-ttu-id="a33ce-190">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="a33ce-190">Value (type)</span></span> | <span data-ttu-id="a33ce-191">Descripción</span><span class="sxs-lookup"><span data-stu-id="a33ce-191">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a33ce-192">SSL. ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="a33ce-192">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="a33ce-193">SSL. LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="a33ce-193">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="a33ce-194">servidor. DirecciónIPRemota</span><span class="sxs-lookup"><span data-stu-id="a33ce-194">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="a33ce-195">servidor. PuertoRemoto</span><span class="sxs-lookup"><span data-stu-id="a33ce-195">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="a33ce-196">servidor. LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="a33ce-196">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="a33ce-197">servidor. LocalPort</span><span class="sxs-lookup"><span data-stu-id="a33ce-197">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="a33ce-198">servidor. IsLocal</span><span class="sxs-lookup"><span data-stu-id="a33ce-198">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="a33ce-199">servidor. OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="a33ce-199">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="a33ce-200">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="a33ce-200">SendFiles v0.3.0</span></span>

| <span data-ttu-id="a33ce-201">Key</span><span class="sxs-lookup"><span data-stu-id="a33ce-201">Key</span></span>               | <span data-ttu-id="a33ce-202">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="a33ce-202">Value (type)</span></span> | <span data-ttu-id="a33ce-203">Descripción</span><span class="sxs-lookup"><span data-stu-id="a33ce-203">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a33ce-204">SendFile. SendAsync</span><span class="sxs-lookup"><span data-stu-id="a33ce-204">sendfile.SendAsync</span></span> | <span data-ttu-id="a33ce-205">Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a33ce-205">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="a33ce-206">Por solicitud</span><span class="sxs-lookup"><span data-stu-id="a33ce-206">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="a33ce-207">V0.3.0 opaco</span><span class="sxs-lookup"><span data-stu-id="a33ce-207">Opaque v0.3.0</span></span>

| <span data-ttu-id="a33ce-208">Key</span><span class="sxs-lookup"><span data-stu-id="a33ce-208">Key</span></span>               | <span data-ttu-id="a33ce-209">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="a33ce-209">Value (type)</span></span> | <span data-ttu-id="a33ce-210">Descripción</span><span class="sxs-lookup"><span data-stu-id="a33ce-210">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a33ce-211">opaco. Versión</span><span class="sxs-lookup"><span data-stu-id="a33ce-211">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="a33ce-212">opaco. Actualización</span><span class="sxs-lookup"><span data-stu-id="a33ce-212">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="a33ce-213">Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a33ce-213">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="a33ce-214">opaco. Secuencia</span><span class="sxs-lookup"><span data-stu-id="a33ce-214">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="a33ce-215">opaco. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="a33ce-215">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="a33ce-216">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="a33ce-216">WebSocket v0.3.0</span></span>

| <span data-ttu-id="a33ce-217">Key</span><span class="sxs-lookup"><span data-stu-id="a33ce-217">Key</span></span>               | <span data-ttu-id="a33ce-218">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="a33ce-218">Value (type)</span></span> | <span data-ttu-id="a33ce-219">Descripción</span><span class="sxs-lookup"><span data-stu-id="a33ce-219">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="a33ce-220">websocket. Versión</span><span class="sxs-lookup"><span data-stu-id="a33ce-220">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="a33ce-221">websocket. Aceptar</span><span class="sxs-lookup"><span data-stu-id="a33ce-221">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="a33ce-222">Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a33ce-222">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="a33ce-223">websocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="a33ce-223">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="a33ce-224">Especificación no</span><span class="sxs-lookup"><span data-stu-id="a33ce-224">Non-spec</span></span> |
| <span data-ttu-id="a33ce-225">websocket. Subprotocolo</span><span class="sxs-lookup"><span data-stu-id="a33ce-225">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="a33ce-226">Vea [RFC6455 sección 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) paso 5.5</span><span class="sxs-lookup"><span data-stu-id="a33ce-226">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="a33ce-227">websocket. SendAsync</span><span class="sxs-lookup"><span data-stu-id="a33ce-227">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="a33ce-228">Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a33ce-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="a33ce-229">websocket. ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="a33ce-229">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="a33ce-230">Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a33ce-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="a33ce-231">websocket. CloseAsync</span><span class="sxs-lookup"><span data-stu-id="a33ce-231">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="a33ce-232">Vea [firma de delegado](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="a33ce-232">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="a33ce-233">websocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="a33ce-233">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="a33ce-234">websocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="a33ce-234">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="a33ce-235">Optional</span><span class="sxs-lookup"><span data-stu-id="a33ce-235">Optional</span></span> |
| <span data-ttu-id="a33ce-236">websocket. ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="a33ce-236">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="a33ce-237">Optional</span><span class="sxs-lookup"><span data-stu-id="a33ce-237">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="a33ce-238">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a33ce-238">Additional Resources</span></span>

* [<span data-ttu-id="a33ce-239">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="a33ce-239">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="a33ce-240">Servidores</span><span class="sxs-lookup"><span data-stu-id="a33ce-240">Servers</span></span>](servers/index.md)