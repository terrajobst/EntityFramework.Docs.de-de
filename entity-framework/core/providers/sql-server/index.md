---
title: Microsoft SQL Server-Datenbankanbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 1e75bc4bf334b1a60d13a2ec9ef314e3afcf0273
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812100"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="70e71-102">Microsoft SQL Server-EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="70e71-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="70e71-103">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit Microsoft SQL Server (einschließlich SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="70e71-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="70e71-104">Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="70e71-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="70e71-105">Installieren</span><span class="sxs-lookup"><span data-stu-id="70e71-105">Install</span></span>

<span data-ttu-id="70e71-106">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.SqlServer“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="70e71-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="70e71-107">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="70e71-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="70e71-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="70e71-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="70e71-109">Seit Version 3.0.0 verweist der Anbieter auf Microsoft.Data.SqlClient (vorherige Versionen waren von System.Data.SqlClient abhängig).</span><span class="sxs-lookup"><span data-stu-id="70e71-109">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="70e71-110">Wenn Ihr Projekt direkt von SqlClient abhängt, stellen Sie sicher, dass es auf das richtige Paket verweist.</span><span class="sxs-lookup"><span data-stu-id="70e71-110">If your project takes a direct dependency on SqlClient, make sure it references the correct package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="70e71-111">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="70e71-111">Supported Database Engines</span></span>

* <span data-ttu-id="70e71-112">Microsoft SQL Server (2012 oder höher)</span><span class="sxs-lookup"><span data-stu-id="70e71-112">Microsoft SQL Server (2012 onwards)</span></span>
