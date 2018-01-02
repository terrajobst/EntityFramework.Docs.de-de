---
title: "FirebirdSQL-Datenbankanbieter – EF Core"
author: ralmsdeveloper
ms.author: ralmsdeveloper
ms.date: 11/22/2017
ms.assetid: d0168c04-d30d-4219-98f8-a54690cea3c6
ms.technology: entity-framework-core
uid: core/providers/firebird-community/index
ms.openlocfilehash: 682988a91ef04dbd552588a537f53124b931f17d
ms.sourcegitcommit: 1cbd3d3cd92bdaf8223b8821c58200bcfed10ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2017
---
# <a name="firebird-ef-core-database-provider"></a><span data-ttu-id="c6ce9-102">Firebird-EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="c6ce9-102">Firebird EF Core Database Provider</span></span>

<span data-ttu-id="c6ce9-103">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit FirebirdSQL.</span><span class="sxs-lookup"><span data-stu-id="c6ce9-103">This database provider allows Entity Framework Core to be used with FirebirdSQL.</span></span> <span data-ttu-id="c6ce9-104">Der Anbieter wird im Rahmen des [GitHub-Projekts „ralmsdeveloper/EntityFrameworkCore.FirebirdSQL“](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="c6ce9-104">The provider is maintained as part of the [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub Project](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="c6ce9-105">Dieser Anbieter wird nicht im Rahmen des Entity Framework Core-Projekts verwaltet.</span><span class="sxs-lookup"><span data-stu-id="c6ce9-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c6ce9-106">Wenn Sie einen Drittanbieter in Betracht ziehen, sollten Sie Aspekte wie Qualität, Lizenzierung und Support auswerten, um sicherzustellen, dass dieser Ihren Anforderungen entspricht.</span><span class="sxs-lookup"><span data-stu-id="c6ce9-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="c6ce9-107">Installieren</span><span class="sxs-lookup"><span data-stu-id="c6ce9-107">Install</span></span>

<span data-ttu-id="c6ce9-108">Installieren Sie das [NuGet-Paket „EntityFrameworkCore.FirebirdSQL“](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="c6ce9-108">Install the [EntityFrameworkCore.FirebirdSQL NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span></span>

``` powershell
Install-Package EntityFrameworkCore.FirebirdSQL
```

## <a name="get-started"></a><span data-ttu-id="c6ce9-109">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="c6ce9-109">Get Started</span></span>

<span data-ttu-id="c6ce9-110">Weitere Informationen finden Sie in der [Dokumentation mit den ersten Schritten auf der Projektwebsite](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki).</span><span class="sxs-lookup"><span data-stu-id="c6ce9-110">See the [getting started documentation on the project site](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="c6ce9-111">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="c6ce9-111">Supported Database Engines</span></span>

* <span data-ttu-id="c6ce9-112">FirebirdSql 2.5</span><span class="sxs-lookup"><span data-stu-id="c6ce9-112">FirebirdSql 2.5</span></span>
* <span data-ttu-id="c6ce9-113">FirebirdSql 3.X</span><span class="sxs-lookup"><span data-stu-id="c6ce9-113">FirebirdSql 3.X</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="c6ce9-114">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="c6ce9-114">Supported Platforms</span></span>

* <span data-ttu-id="c6ce9-115">.NET Framework (4.5.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="c6ce9-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="c6ce9-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6ce9-116">.NET Core</span></span>

* <span data-ttu-id="c6ce9-117">Mono (4.2.0 oder höher)</span><span class="sxs-lookup"><span data-stu-id="c6ce9-117">Mono (4.2.0 onwards)</span></span>
