---
title: Cliente de SignalR JavaScript Core ASP.NET
author: rachelappel
description: Información general de cliente de ASP.NET Core SignalR JavaScript.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/06/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: cca1a523bd536f4365e54c196f6c9024779d5d76
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="908ec-103">Cliente de SignalR JavaScript Core ASP.NET</span><span class="sxs-lookup"><span data-stu-id="908ec-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="908ec-104">Por [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="908ec-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="908ec-105">La biblioteca de cliente de ASP.NET Core SignalR JavaScript permite a los desarrolladores llamar a código de concentrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="908ec-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="908ec-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="908ec-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="908ec-107">Instale el paquete de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="908ec-107">Install the SignalR client package</span></span>

<span data-ttu-id="908ec-108">La biblioteca de cliente de SignalR JavaScript se entrega como un [npm](https://www.npmjs.com/) paquete.</span><span class="sxs-lookup"><span data-stu-id="908ec-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="908ec-109">Si está utilizando Visual Studio, ejecute `npm install` desde el **Package Manager Console** mientras se encuentre en la carpeta raíz.</span><span class="sxs-lookup"><span data-stu-id="908ec-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="908ec-110">Para el código de Visual Studio, ejecute el comando desde el **Terminal integrado**.</span><span class="sxs-lookup"><span data-stu-id="908ec-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="908ec-111">NPM instala el contenido del paquete en el *node_modules\\ @aspnet\signalr\dist\browser*  carpeta.</span><span class="sxs-lookup"><span data-stu-id="908ec-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="908ec-112">Cree una carpeta nueva denominada *signalr* en el *wwwroot\\lib* carpeta.</span><span class="sxs-lookup"><span data-stu-id="908ec-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="908ec-113">Copia la *signalr.js* del archivo a la *wwwroot\lib\signalr* carpeta.</span><span class="sxs-lookup"><span data-stu-id="908ec-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="908ec-114">Usar al cliente de SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="908ec-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="908ec-115">Referencia de cliente de SignalR JavaScript de la `<script>` elemento.</span><span class="sxs-lookup"><span data-stu-id="908ec-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="908ec-116">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="908ec-116">Connect to a hub</span></span>

<span data-ttu-id="908ec-117">El código siguiente se crea e inicia una conexión.</span><span class="sxs-lookup"><span data-stu-id="908ec-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="908ec-118">Nombre del concentrador distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="908ec-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=1-2,18)]

### <a name="cross-origin-connections"></a><span data-ttu-id="908ec-119">Conexiones entre orígenes</span><span class="sxs-lookup"><span data-stu-id="908ec-119">Cross-origin connections</span></span>

<span data-ttu-id="908ec-120">Por lo general, los exploradores cargar las conexiones desde el mismo dominio que la página solicitada.</span><span class="sxs-lookup"><span data-stu-id="908ec-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="908ec-121">Sin embargo, hay ocasiones cuando se requiere una conexión a otro dominio.</span><span class="sxs-lookup"><span data-stu-id="908ec-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="908ec-122">Para evitar que un sitio malintencionado pueda leer los datos confidenciales desde otro sitio, [las conexiones entre orígenes](xref:security/cors) están deshabilitadas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="908ec-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="908ec-123">Para permitir que una solicitud entre orígenes, habilitarla en la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="908ec-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-34,55)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="908ec-124">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="908ec-124">Call hub methods from client</span></span>

<span data-ttu-id="908ec-125">Los clientes de JavaScript llama al métodos públicos en concentradores por usando `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="908ec-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="908ec-126">El `invoke` método acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="908ec-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="908ec-127">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="908ec-127">The name of the hub method.</span></span> <span data-ttu-id="908ec-128">En el ejemplo siguiente, el nombre de la base de datos central es `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="908ec-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="908ec-129">Los argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="908ec-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="908ec-130">En el ejemplo siguiente, el nombre del argumento es `message`.</span><span class="sxs-lookup"><span data-stu-id="908ec-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=14)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="908ec-131">Llamar a métodos de cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="908ec-131">Call client methods from hub</span></span>

<span data-ttu-id="908ec-132">Para recibir mensajes desde el concentrador, defina un método mediante el `connection.on` método.</span><span class="sxs-lookup"><span data-stu-id="908ec-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="908ec-133">El nombre del método de cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="908ec-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="908ec-134">En el ejemplo siguiente, el nombre de método es `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="908ec-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="908ec-135">Argumentos de que la central se pasa al método.</span><span class="sxs-lookup"><span data-stu-id="908ec-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="908ec-136">En el ejemplo siguiente, el valor del argumento es `message`.</span><span class="sxs-lookup"><span data-stu-id="908ec-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=4-9)]

<span data-ttu-id="908ec-137">El código anterior en `connection.on` se ejecuta cuando llama a código del lado servidor mediante el `SendAsync` método.</span><span class="sxs-lookup"><span data-stu-id="908ec-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-javascript[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="908ec-138">SignalR determina a qué método de cliente para llamar a haciendo coincidir el nombre de método y los argumentos definan en `SendAsync` y `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="908ec-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="908ec-139">Como práctica recomendada, llame a `connection.start` después `connection.on` por lo que los controladores están registrados antes de que se reciben los mensajes.</span><span class="sxs-lookup"><span data-stu-id="908ec-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="908ec-140">Registro y control de errores</span><span class="sxs-lookup"><span data-stu-id="908ec-140">Error handling and logging</span></span>

<span data-ttu-id="908ec-141">Cadena de un `catch` método al final de la `connection.start` método para controlar los errores de cliente.</span><span class="sxs-lookup"><span data-stu-id="908ec-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="908ec-142">Use `console.error` para errores de salida a la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="908ec-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=18)]

<span data-ttu-id="908ec-143">Configurar la traza de registro del lado cliente pasando un registrador y el tipo de evento que se registran cuando se realiza la conexión.</span><span class="sxs-lookup"><span data-stu-id="908ec-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="908ec-144">Los mensajes se registran con el nivel de registro especificado y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="908ec-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="908ec-145">Niveles de registro disponibles son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="908ec-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="908ec-146">`signalR.LogLevel.Error` : Mensajes de error.</span><span class="sxs-lookup"><span data-stu-id="908ec-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="908ec-147">Registros `Error` sólo mensajes.</span><span class="sxs-lookup"><span data-stu-id="908ec-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="908ec-148">`signalR.LogLevel.Warning` : Mensajes de advertencia acerca de posibles errores.</span><span class="sxs-lookup"><span data-stu-id="908ec-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="908ec-149">Registros de `Warning`, y `Error` mensajes.</span><span class="sxs-lookup"><span data-stu-id="908ec-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="908ec-150">`signalR.LogLevel.Information` : Mensajes de estado sin errores.</span><span class="sxs-lookup"><span data-stu-id="908ec-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="908ec-151">Registros de `Information`, `Warning`, y `Error` mensajes.</span><span class="sxs-lookup"><span data-stu-id="908ec-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="908ec-152">`signalR.LogLevel.Trace` : Mensajes de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="908ec-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="908ec-153">Registra todo, incluidos los datos transportados entre cliente y concentrador.</span><span class="sxs-lookup"><span data-stu-id="908ec-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="908ec-154">Pase el registrador para la conexión para iniciar el registro.</span><span class="sxs-lookup"><span data-stu-id="908ec-154">Pass the logger to the connection to start logging.</span></span> <span data-ttu-id="908ec-155">Herramientas de desarrollo de explorador normalmente contienen una consola que muestra los mensajes.</span><span class="sxs-lookup"><span data-stu-id="908ec-155">Browser developer tools typically contain a console that displays the messages.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=1-2)]

## <a name="related-resources"></a><span data-ttu-id="908ec-156">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="908ec-156">Related resources</span></span>

* [<span data-ttu-id="908ec-157">Concentradores de SignalR Core de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="908ec-157">ASP.NET Core SignalR Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="908ec-158">Permitir solicitudes entre orígenes (CORS) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="908ec-158">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)