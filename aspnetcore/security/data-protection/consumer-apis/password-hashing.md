---
title: "Hash de contraseña"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 982a1eb2-1e6f-4909-896f-82784364472d
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: d4b39bf7feba8a29e0b5c9e56d53a85b82977b7e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="password-hashing"></a><span data-ttu-id="3eb79-103">Hash de contraseña</span><span class="sxs-lookup"><span data-stu-id="3eb79-103">Password Hashing</span></span>

<span data-ttu-id="3eb79-104">La base de código de protección de datos incluye un paquete *Microsoft.AspNetCore.Cryptography.KeyDerivation* que contiene funciones de derivación de claves criptográficas.</span><span class="sxs-lookup"><span data-stu-id="3eb79-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="3eb79-105">Este paquete es un componente independiente y no tiene ninguna dependencia en el resto del sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="3eb79-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="3eb79-106">Se puede utilizar completamente por separado.</span><span class="sxs-lookup"><span data-stu-id="3eb79-106">It can be used completely independently.</span></span> <span data-ttu-id="3eb79-107">El origen coexista con el código de protección de datos base para su comodidad.</span><span class="sxs-lookup"><span data-stu-id="3eb79-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="3eb79-108">Actualmente, el paquete ofrece un método `KeyDerivation.Pbkdf2` que permite que se aplican algoritmos hash a una contraseña mediante el [PBKDF2 algoritmo](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="3eb79-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="3eb79-109">Esta API es muy similar a existente de .NET Framework [Rfc2898DeriveBytes tipo](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), pero hay tres diferencias importantes:</span><span class="sxs-lookup"><span data-stu-id="3eb79-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="3eb79-110">El `KeyDerivation.Pbkdf2` método compatible con el uso de varios PRFs (actualmente `HMACSHA1`, `HMACSHA256`, y `HMACSHA512`), mientras que la `Rfc2898DeriveBytes` escriba sólo admite `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="3eb79-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="3eb79-111">El `KeyDerivation.Pbkdf2` método detecta el sistema operativo actual e intenta elegir la implementación de la rutina, proporcionando un rendimiento mucho mejor en ciertos casos más optimizada.</span><span class="sxs-lookup"><span data-stu-id="3eb79-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="3eb79-112">(En Windows 8, que ofrece aproximadamente 10 veces el rendimiento de `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="3eb79-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="3eb79-113">El `KeyDerivation.Pbkdf2` método requiere que el llamador especificar todos los parámetros (sal, PRF y número de iteraciones).</span><span class="sxs-lookup"><span data-stu-id="3eb79-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="3eb79-114">El `Rfc2898DeriveBytes` tipo proporciona valores predeterminados para estos.</span><span class="sxs-lookup"><span data-stu-id="3eb79-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

<span data-ttu-id="3eb79-115">[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]</span><span class="sxs-lookup"><span data-stu-id="3eb79-115">[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]</span></span>

<span data-ttu-id="3eb79-116">Vea el código fuente de ASP.NET Core Identity `PasswordHasher` caso de uso de tipo para un mundo real.</span><span class="sxs-lookup"><span data-stu-id="3eb79-116">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>