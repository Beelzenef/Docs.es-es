---
title: Agregar, descargar y eliminar datos de usuario personalizada para la identidad en un proyecto de ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo agregar datos de usuario personalizada para la identidad en un proyecto de ASP.NET Core. Eliminar datos por GDPR.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
uid: security/authentication/add-user-data
ms.openlocfilehash: ecd0e6d1c71b24309fab70fbb06af7731463bb0e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271962"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="64972-104">Agregar, descargar y eliminar datos de usuario personalizada para la identidad en un proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64972-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="64972-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="64972-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="64972-106">Este artículo se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="64972-106">This article shows how to:</span></span>

* <span data-ttu-id="64972-107">Agregar datos de usuario personalizado a una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="64972-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="64972-108">Decorar el modelo de datos de usuario personalizada con el [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atributo para que esté disponible automáticamente para su eliminación y la descarga.</span><span class="sxs-lookup"><span data-stu-id="64972-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="64972-109">Hacer que los datos que se puede descargar y eliminar ayuda a cumplir [GDPR](xref:security/gdpr) requisitos.</span><span class="sxs-lookup"><span data-stu-id="64972-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="64972-110">Se crea el proyecto de ejemplo desde una aplicación web de las páginas de Razor, pero las instrucciones son similares para una aplicación web de MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="64972-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="64972-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="64972-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64972-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="64972-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="64972-113">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="64972-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="64972-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64972-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="64972-115">En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="64972-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="64972-116">Denomine el proyecto **WebApp1** si desea coincida con el espacio de nombres de la [Descargar ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) código.</span><span class="sxs-lookup"><span data-stu-id="64972-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="64972-117">Seleccione **aplicación Web de ASP.NET Core** > **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="64972-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="64972-118">Seleccione **ASP.NET Core 2.1** en la lista desplegable</span><span class="sxs-lookup"><span data-stu-id="64972-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="64972-119">Seleccione **aplicación Web**  > **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="64972-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="64972-120">Compile y ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="64972-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="64972-121">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="64972-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="64972-122">Ejecute al scaffolder de identidad</span><span class="sxs-lookup"><span data-stu-id="64972-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="64972-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64972-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="64972-124">De **el Explorador de soluciones**, haga doble clic en el proyecto > **agregar** > **nuevo elemento de scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="64972-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="64972-125">En el panel izquierdo de la **agregar scaffolding** cuadro de diálogo, seleccione **identidad** > **agregar**.</span><span class="sxs-lookup"><span data-stu-id="64972-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="64972-126">En el **Agregar identidad** cuadro de diálogo, las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="64972-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="64972-127">Seleccione el archivo de diseño existente *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="64972-127">Select your existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="64972-128">Seleccione los archivos siguientes para reemplazar:</span><span class="sxs-lookup"><span data-stu-id="64972-128">Select the following files to override:</span></span>
    * <span data-ttu-id="64972-129">**Cuenta/Register**</span><span class="sxs-lookup"><span data-stu-id="64972-129">**Account/Register**</span></span>
    * <span data-ttu-id="64972-130">**Cuenta/Administrar/índice**</span><span class="sxs-lookup"><span data-stu-id="64972-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="64972-131">Seleccione el **+** botón para crear un nuevo **clase de contexto de datos**.</span><span class="sxs-lookup"><span data-stu-id="64972-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="64972-132">Acepte el tipo (**WebApp1.Models.WebApp1Context** si denominado el proyecto **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="64972-132">Accept the type (**WebApp1.Models.WebApp1Context** if you named the project **WebApp1**).</span></span>
  * <span data-ttu-id="64972-133">Seleccione el **+** botón para crear un nuevo **User (clase)**.</span><span class="sxs-lookup"><span data-stu-id="64972-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="64972-134">Acepte el tipo (**WebApp1User** si denominado el proyecto **WebApp1**) > **agregar**.</span><span class="sxs-lookup"><span data-stu-id="64972-134">Accept the type (**WebApp1User** if you named the project **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="64972-135">Seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="64972-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="64972-136">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="64972-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="64972-137">Si no ha instalado previamente el scaffolder ASP.NET, instalarla ahora:</span><span class="sxs-lookup"><span data-stu-id="64972-137">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="64972-138">Agregue una referencia de paquete a [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al archivo de proyecto (.csproj).</span><span class="sxs-lookup"><span data-stu-id="64972-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="64972-139">Ejecute el siguiente comando en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="64972-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="64972-140">Ejecute el siguiente comando para mostrar las opciones de scaffolder de identidad:</span><span class="sxs-lookup"><span data-stu-id="64972-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="64972-141">En la carpeta del proyecto, ejecute al scaffolder de identidad:</span><span class="sxs-lookup"><span data-stu-id="64972-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="64972-142">Siga las instrucciones [UseAuthentication, migraciones y diseño](xref:security/authentication/scaffold-identity#efm) para realizar los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="64972-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="64972-143">Cree una migración y actualice la base de datos.</span><span class="sxs-lookup"><span data-stu-id="64972-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="64972-144">Agregue `UseAuthentication` a `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="64972-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="64972-145">Agregar `<partial name="_LoginPartial" />` para el archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="64972-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="64972-146">Pruebe la aplicación:</span><span class="sxs-lookup"><span data-stu-id="64972-146">Test the app:</span></span>
  * <span data-ttu-id="64972-147">Registrar un usuario</span><span class="sxs-lookup"><span data-stu-id="64972-147">Register a user</span></span>
  * <span data-ttu-id="64972-148">Seleccione el nuevo nombre de usuario (junto a la **Logout** vínculo).</span><span class="sxs-lookup"><span data-stu-id="64972-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="64972-149">Deberá expandir la ventana o seleccione el icono de la barra de navegación para mostrar el nombre de usuario y otros vínculos.</span><span class="sxs-lookup"><span data-stu-id="64972-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="64972-150">Seleccione el **datos personales** ficha.</span><span class="sxs-lookup"><span data-stu-id="64972-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="64972-151">Seleccione el **descargar** botón y examinar el *PersonalData.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="64972-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="64972-152">Prueba la **eliminar** botón, lo que elimina el inicio de sesión de usuario.</span><span class="sxs-lookup"><span data-stu-id="64972-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="64972-153">Agregar datos de usuario personalizado a la base de datos de identidad</span><span class="sxs-lookup"><span data-stu-id="64972-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="64972-154">Actualización de la `IdentityUser` deriva la clase con propiedades personalizadas.</span><span class="sxs-lookup"><span data-stu-id="64972-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="64972-155">Si con el nombre de su proyecto WebApp1, el archivo se denomina *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="64972-155">If you named your project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="64972-156">Actualice el archivo con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="64972-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="64972-157">Propiedades decorada con el [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atributo son:</span><span class="sxs-lookup"><span data-stu-id="64972-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="64972-158">Cuando elimina el *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor página llama `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="64972-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="64972-159">Incluido en los datos descargados por el *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* página Razor.</span><span class="sxs-lookup"><span data-stu-id="64972-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="64972-160">Actualizar la página Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="64972-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="64972-161">Actualización de la `InputModel` en *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* aparece resaltado este código con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="64972-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="64972-162">Actualización de la *Areas/Identity/Pages/Account/Manage/Index.cshtml* con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="64972-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="64972-163">Actualizar la página Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="64972-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="64972-164">Actualización de la `InputModel` en *Areas/Identity/Pages/Account/Register.cshtml.cs* aparece resaltado este código con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="64972-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="64972-165">Actualización de la *Areas/Identity/Pages/Account/Register.cshtml* con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="64972-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="64972-166">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="64972-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="64972-167">Agregar una migración de los datos de usuario personalizada</span><span class="sxs-lookup"><span data-stu-id="64972-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="64972-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64972-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="64972-169">En Visual Studio **consola de administrador de paquetes**:</span><span class="sxs-lookup"><span data-stu-id="64972-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="64972-170">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="64972-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="64972-171">Prueba de crear, ver, descargar, eliminar datos de usuario personalizada</span><span class="sxs-lookup"><span data-stu-id="64972-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="64972-172">Pruebe la aplicación:</span><span class="sxs-lookup"><span data-stu-id="64972-172">Test the app:</span></span>

* <span data-ttu-id="64972-173">Registrar un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="64972-173">Register a new user.</span></span>
* <span data-ttu-id="64972-174">Ver los datos de usuario personalizada en el `/Identity/Account/Manage` página.</span><span class="sxs-lookup"><span data-stu-id="64972-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="64972-175">Descargar y ver los datos personales de los usuarios de la `/Identity/Account/Manage/PersonalData` página.</span><span class="sxs-lookup"><span data-stu-id="64972-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>