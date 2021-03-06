---
title: Agregar un campo nuevo a una página de Razor en ASP.NET Core
author: rick-anderson
description: Muestra cómo agregar un nuevo campo a una página de Razor con Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: e280bc9553113982a1f1a77eabab32575c905237
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862296"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a>Agregar un campo nuevo a una página de Razor en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/rp/download.md)]

En esta sección, Migraciones de [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First se utiliza para:

* Agregar un campo nuevo al modelo.
* Migrar el cambio de esquema del campo nuevo a la base de datos.

Al usar EF Code First para crear una base de datos automáticamente, Code First hace lo siguiente:

* Agrega una tabla a la base de datos para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo a partir del que se ha generado.
* Si las clases del modelo no están sincronizadas con la base de datos, EF produce una excepción.

La comprobación automática de la sincronización del esquema/modelo facilita la detección de problemas de código o base de datos incoherentes.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Abra el archivo *Models/Movie.cs* y agregue una propiedad `Rating`:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

Compile la aplicación.

Edite *Pages/Movies/Index.cshtml* y agregue un campo `Rating`:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

Actualice las páginas siguientes:

* Agregue el campo `Rating` a las páginas Delete y Details.
* Actualice [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) con un campo `Rating`.
* Agregue el campo `Rating` a la página de edición.

La aplicación no funciona hasta que la base de datos se actualiza para incluir el nuevo campo. Si se ejecuta ahora, la aplicación produce una `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Este error se debe a que la clase del modelo Movie actualizado es diferente al esquema de la tabla Movie de la base de datos. (No hay ninguna columna `Rating` en la tabla de la base de datos).

Este error se puede resolver de varias maneras:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear con el nuevo esquema de la clase del modelo. Este enfoque resulta conveniente al principio del ciclo de desarrollo; permite desarrollar a la vez y de manera rápida el esquema del modelo y la base de datos. El inconveniente es que se pierden los datos existentes en la base de datos. No use este enfoque en una base de datos de producción. El quitar la base de datos en los cambios de esquema y el usar un inicializador para inicializar automáticamente la base de datos con datos de prueba suele ser una forma productiva de desarrollar una aplicación.

2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.

3. Use Migraciones de Code First para actualizar el esquema de la base de datos.

Para este tutorial, use Migraciones de Code First.

Actualice la clase `SeedData` para que proporcione un valor para la nueva columna. A continuación se muestra un cambio de ejemplo, aunque es conveniente realizar este cambio para cada bloque `new Movie`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

Vea el [archivo completado SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).

Compile la solución.

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a>Agregar una migración para el campo de clasificación

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet > Consola del Administrador de paquetes**.
En PCM, escriba los siguientes comandos:

```powershell
Add-Migration Rating
Update-Database
```

El comando `Add-Migration` indica al marco de trabajo que:

* Compare el modelo `Movie` con el esquema de base de datos `Movie`.
* Cree código para migrar el esquema de la base de datos al nuevo modelo.

El nombre "Rating" es arbitrario y se usa para asignar nombre al archivo de migración. Resulta útil emplear un nombre descriptivo para el archivo de migración.

<a name="ssox"></a>

Si elimina todos los registros de la base de datos, el inicializador inicializará la base de datos e incluirá el campo `Rating`. Puede hacerlo con los vínculos de eliminación en el explorador o desde el [Explorador de objetos de SQL Server](xref:tutorials/razor-pages/sql#ssox) (SSOX). Para eliminar la base de datos desde SSOX:

* Seleccione la base de datos en SSOX.
* Haga clic con el botón derecho en la base de datos y seleccione *Eliminar*.
* Active **Cerrar las conexiones existentes**.
* Seleccione **Aceptar**.
* En la [PMC](xref:tutorials/razor-pages/new-field#pmc), actualice la base de datos:

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- copy/paste this tab to the next. Not worth an include  --> SQLite no es compatible con las migraciones.

* Elimine la base de datos o cambie su nombre en el archivo *appsettings.json*.
* Elimine la carpeta *Migraciones* (y todos los archivos que contenga).

Ejecute los siguientes comandos CLI de .NET Core:

```console
dotnet ef migrations add Rating
dotnet ef database update
```

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

SQLite no es compatible con las migraciones.

* Elimine la base de datos o cambie su nombre en el archivo *appsettings.json*.
* Elimine la carpeta *Migraciones* (y todos los archivos que contenga).

Ejecute los siguientes comandos CLI de .NET Core:

```console
dotnet ef migrations add Rating
dotnet ef database update
```

---  
<!-- End of VS tabs -->

Ejecute la aplicación y compruebe que puede crear, editar o mostrar películas con un campo `Rating`. Si la base de datos no se ha propagado, establezca un punto de interrupción en el método `SeedData.Initialize`.

> [!div class="step-by-step"]
> [Anterior: Adición de búsqueda](xref:tutorials/razor-pages/search)
> [Siguiente: Adición de una validación](xref:tutorials/razor-pages/validation)
