---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: "Crear una restricción de ruta (VB) | Documentos de Microsoft"
author: StephenWalther
description: "En este tutorial, Stephen Walther se muestra cómo se puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de restricciones de la ruta con expresiones regulares."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 67ff2666f4558abd4f8d9bddffd7aef8bb68d7bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-route-constraint-vb"></a><span data-ttu-id="425fe-103">Crear una restricción de ruta (VB)</span><span class="sxs-lookup"><span data-stu-id="425fe-103">Creating a Route Constraint (VB)</span></span>
====================
<span data-ttu-id="425fe-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="425fe-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="425fe-105">En este tutorial, Stephen Walther se muestra cómo se puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de restricciones de la ruta con expresiones regulares.</span><span class="sxs-lookup"><span data-stu-id="425fe-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="425fe-106">Utilizar restricciones de la ruta para restringir las solicitudes del explorador que coinciden con una ruta determinada.</span><span class="sxs-lookup"><span data-stu-id="425fe-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="425fe-107">Puede usar una expresión regular para especificar una restricción de ruta.</span><span class="sxs-lookup"><span data-stu-id="425fe-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="425fe-108">Por ejemplo, imagine que ha definido la ruta en el listado 1 en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="425fe-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="425fe-109">**Lista 1 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="425fe-109">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="425fe-110">Lista 1 contiene una ruta con el nombre de producto.</span><span class="sxs-lookup"><span data-stu-id="425fe-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="425fe-111">Puede usar la ruta de producto para asignar las solicitudes del explorador a la ProductController contenido en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="425fe-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="425fe-112">**La lista 2 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="425fe-112">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="425fe-113">Observe que la acción de Details() expuesta por el controlador del producto acepta un solo parámetro denominado productId.</span><span class="sxs-lookup"><span data-stu-id="425fe-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="425fe-114">Este parámetro es un parámetro de número entero.</span><span class="sxs-lookup"><span data-stu-id="425fe-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="425fe-115">La ruta definida en la lista 1 coincide con cualquiera de las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="425fe-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="425fe-116">/ Productos/23</span><span class="sxs-lookup"><span data-stu-id="425fe-116">/Product/23</span></span>
- <span data-ttu-id="425fe-117">/ / De producto 7</span><span class="sxs-lookup"><span data-stu-id="425fe-117">/Product/7</span></span>

<span data-ttu-id="425fe-118">Desafortunadamente, la ruta también coincidirá con las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="425fe-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="425fe-119">/ Productos/bLa, bla</span><span class="sxs-lookup"><span data-stu-id="425fe-119">/Product/blah</span></span>
- <span data-ttu-id="425fe-120">/ Productos/apple</span><span class="sxs-lookup"><span data-stu-id="425fe-120">/Product/apple</span></span>

<span data-ttu-id="425fe-121">Dado que la acción Details() espera un parámetro de número entero, que realiza una solicitud que contiene un valor distinto de un valor entero se producirá un error.</span><span class="sxs-lookup"><span data-stu-id="425fe-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="425fe-122">Por ejemplo, si escribe la dirección URL /Product/apple en el explorador, a continuación, obtendrá la página de error en la figura 1.</span><span class="sxs-lookup"><span data-stu-id="425fe-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="425fe-123">[![El cuadro de diálogo nuevo proyecto](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="425fe-123">[![The New Project dialog box](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span></span>

<span data-ttu-id="425fe-124">**Figura 01**: ver una página seccionar ([haga clic aquí para ver la imagen a tamaño completo](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="425fe-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>


<span data-ttu-id="425fe-125">¿Qué desea hacer en realidad es solo coincidirá con direcciones URL que contienen un productId entero adecuado.</span><span class="sxs-lookup"><span data-stu-id="425fe-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="425fe-126">Puede usar una restricción al definir una ruta para restringir las direcciones URL que coinciden con la ruta.</span><span class="sxs-lookup"><span data-stu-id="425fe-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="425fe-127">La ruta de producto modificada en el listado 3 contiene una restricción de expresión regular que coincida solo con enteros.</span><span class="sxs-lookup"><span data-stu-id="425fe-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="425fe-128">**El listado 3 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="425fe-128">**Listing 3 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="425fe-129">La expresión regular \d+ coincide con uno o más números enteros.</span><span class="sxs-lookup"><span data-stu-id="425fe-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="425fe-130">Esta restricción hace que la ruta de producto para que coincida con las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="425fe-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="425fe-131">/ Productos/3</span><span class="sxs-lookup"><span data-stu-id="425fe-131">/Product/3</span></span>
- <span data-ttu-id="425fe-132">/ Productos/8999</span><span class="sxs-lookup"><span data-stu-id="425fe-132">/Product/8999</span></span>

<span data-ttu-id="425fe-133">Pero no las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="425fe-133">But not the following URLs:</span></span>

- <span data-ttu-id="425fe-134">/ Productos/apple</span><span class="sxs-lookup"><span data-stu-id="425fe-134">/Product/apple</span></span>
- <span data-ttu-id="425fe-135">/ Producto</span><span class="sxs-lookup"><span data-stu-id="425fe-135">/Product</span></span>

<span data-ttu-id="425fe-136">Estas solicitudes de explorador será procesadas por otra ruta o, si no hay ninguna ruta coincidente, un *no se pudo encontrar el recurso* se devolverá el error.</span><span class="sxs-lookup"><span data-stu-id="425fe-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="425fe-137">[Anterior](creating-custom-routes-vb.md)
[Siguiente](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="425fe-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>