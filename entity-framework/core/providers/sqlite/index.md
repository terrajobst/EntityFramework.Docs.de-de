---
title: SQLite-Datenbankanbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 4637044b1712280d3cdca6bcca1ca61564ff78ea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656069"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="5b4db-102">SQLite-EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="5b4db-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="5b4db-103">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit SQLite.</span><span class="sxs-lookup"><span data-stu-id="5b4db-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="5b4db-104">Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="5b4db-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="5b4db-105">Installieren</span><span class="sxs-lookup"><span data-stu-id="5b4db-105">Install</span></span>

<span data-ttu-id="5b4db-106">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.Sqlite“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="5b4db-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="5b4db-107">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="5b4db-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="5b4db-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b4db-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="5b4db-109">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="5b4db-109">Supported Database Engines</span></span>

* <span data-ttu-id="5b4db-110">SQLite (3.7 oder höher)</span><span class="sxs-lookup"><span data-stu-id="5b4db-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="5b4db-111">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="5b4db-111">Limitations</span></span>

<span data-ttu-id="5b4db-112">Einige wichtige Einschränkungen zum SQLite-Anbieter finden Sie unter [Einschränkungen zu SQLite](limitations.md).</span><span class="sxs-lookup"><span data-stu-id="5b4db-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
