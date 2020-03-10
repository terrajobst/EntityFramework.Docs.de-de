---
title: SQLite-Datenbankanbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e8c3d675322b163fdf1e2e7e01f3815e28f427a2
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413135"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="1e5d4-102">SQLite-EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="1e5d4-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="1e5d4-103">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit SQLite.</span><span class="sxs-lookup"><span data-stu-id="1e5d4-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="1e5d4-104">Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="1e5d4-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="1e5d4-105">Installieren</span><span class="sxs-lookup"><span data-stu-id="1e5d4-105">Install</span></span>

<span data-ttu-id="1e5d4-106">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.Sqlite“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="1e5d4-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="1e5d4-107">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="1e5d4-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="1e5d4-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e5d4-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="1e5d4-109">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="1e5d4-109">Supported Database Engines</span></span>

* <span data-ttu-id="1e5d4-110">SQLite (3.7 oder höher)</span><span class="sxs-lookup"><span data-stu-id="1e5d4-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="1e5d4-111">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="1e5d4-111">Limitations</span></span>

<span data-ttu-id="1e5d4-112">Einige wichtige Einschränkungen zum SQLite-Anbieter finden Sie unter [Einschränkungen zu SQLite](limitations.md).</span><span class="sxs-lookup"><span data-stu-id="1e5d4-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
