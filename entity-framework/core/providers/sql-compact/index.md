---
title: "Microsoft SQL Server Compact Edition-Datenbankanbieter – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a><span data-ttu-id="a67a0-102">Microsoft SQL Server Compact Edition-EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="a67a0-102">Microsoft SQL Server Compact Edition EF Core Database Provider</span></span>

<span data-ttu-id="a67a0-103">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="a67a0-103">This database provider allows Entity Framework Core to be used with SQL Server Compact Edition.</span></span> <span data-ttu-id="a67a0-104">Der Anbieter wird im Rahmen des [GitHub-Projekts „ErikEJ/EntityFramework.SqlServerCompact“](https://github.com/ErikEJ/EntityFramework.SqlServerCompact) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="a67a0-104">The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span>

> [!NOTE]  
> <span data-ttu-id="a67a0-105">Dieser Anbieter wird nicht im Rahmen des Entity Framework Core-Projekts verwaltet.</span><span class="sxs-lookup"><span data-stu-id="a67a0-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="a67a0-106">Wenn Sie einen Drittanbieter in Betracht ziehen, sollten Sie Aspekte wie Qualität, Lizenzierung und Support auswerten, um sicherzustellen, dass dieser Ihren Anforderungen entspricht.</span><span class="sxs-lookup"><span data-stu-id="a67a0-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="a67a0-107">Installieren</span><span class="sxs-lookup"><span data-stu-id="a67a0-107">Install</span></span>

<span data-ttu-id="a67a0-108">Installieren Sie zum Arbeiten mit SQL Server Compact Edition 4.0 das [NuGet-Paket „EntityFrameworkCore.SqlServerCompact40“](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span><span class="sxs-lookup"><span data-stu-id="a67a0-108">To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

<span data-ttu-id="a67a0-109">Installieren Sie zum Arbeiten mit SQL Server Compact Edition 3.5 das Paket [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span><span class="sxs-lookup"><span data-stu-id="a67a0-109">To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a><span data-ttu-id="a67a0-110">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="a67a0-110">Get Started</span></span>

<span data-ttu-id="a67a0-111">Weitere Informationen finden Sie in der [Dokumentation zu den ersten Schritten auf der Projektwebsite](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications).</span><span class="sxs-lookup"><span data-stu-id="a67a0-111">See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="a67a0-112">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="a67a0-112">Supported Database Engines</span></span>

* <span data-ttu-id="a67a0-113">SQL Server Compact Edition 3.5</span><span class="sxs-lookup"><span data-stu-id="a67a0-113">SQL Server Compact Edition 3.5</span></span>

* <span data-ttu-id="a67a0-114">SQL Server Compact Edition 4.0</span><span class="sxs-lookup"><span data-stu-id="a67a0-114">SQL Server Compact Edition 4.0</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a67a0-115">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="a67a0-115">Supported Platforms</span></span>

* <span data-ttu-id="a67a0-116">.NET Framework (4.5.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="a67a0-116">.NET Framework (4.5.1 onwards)</span></span>
