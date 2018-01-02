---
title: "IBM Data Server-Datenbankanbieter (DB2) – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a><span data-ttu-id="be3ec-102">IBM Data Server-EF Core-Datenbankanbieter (DB2)</span><span class="sxs-lookup"><span data-stu-id="be3ec-102">IBM Data Server (DB2) EF Core Database Providers</span></span>

<span data-ttu-id="be3ec-103">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit IBM Data Server.</span><span class="sxs-lookup"><span data-stu-id="be3ec-103">This database provider allows Entity Framework Core to be used with IBM Data Server.</span></span> <span data-ttu-id="be3ec-104">Der Anbieter wird von IBM verwaltet.</span><span class="sxs-lookup"><span data-stu-id="be3ec-104">The provider is maintained by IBM.</span></span>

> [!NOTE]  
> <span data-ttu-id="be3ec-105">Dieser Anbieter wird nicht im Rahmen des Entity Framework Core-Projekts verwaltet.</span><span class="sxs-lookup"><span data-stu-id="be3ec-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="be3ec-106">Wenn Sie einen Drittanbieter in Betracht ziehen, sollten Sie Aspekte wie Qualität, Lizenzierung und Support auswerten, um sicherzustellen, dass dieser Ihren Anforderungen entspricht.</span><span class="sxs-lookup"><span data-stu-id="be3ec-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="be3ec-107">Installieren</span><span class="sxs-lookup"><span data-stu-id="be3ec-107">Install</span></span>

<span data-ttu-id="be3ec-108">Um mit IBM Data Server unter Windows arbeiten zu können, installieren Sie das [NuGet-Paket „IBM.EntityFrameworkCore“](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="be3ec-108">To work with IBM Data Server on Windows, install the [IBM.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore
```

<span data-ttu-id="be3ec-109">Um mit IBM Data Server unter Linux arbeiten zu können, installieren Sie das [NuGet-Paket „IBM.EntityFrameworkCore-lnx“](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span><span class="sxs-lookup"><span data-stu-id="be3ec-109">To work with IBM Data Server on Linux, install the [IBM.EntityFrameworkCore-lnx NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a><span data-ttu-id="be3ec-110">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="be3ec-110">Get Started</span></span>

[<span data-ttu-id="be3ec-111">Getting started with IBM .NET Provider for .NET Core (Erste Schritte mit dem IBM .NET-Anbieter für .NET Core)</span><span class="sxs-lookup"><span data-stu-id="be3ec-111">Getting started with IBM .NET Provider for .NET Core</span></span>](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a><span data-ttu-id="be3ec-112">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="be3ec-112">Supported Database Engines</span></span>

* <span data-ttu-id="be3ec-113">zOS</span><span class="sxs-lookup"><span data-stu-id="be3ec-113">zOS</span></span>
* <span data-ttu-id="be3ec-114">LUW, einschließlich IBM dashDB</span><span class="sxs-lookup"><span data-stu-id="be3ec-114">LUW including IBM dashDB</span></span>
* <span data-ttu-id="be3ec-115">IBM I</span><span class="sxs-lookup"><span data-stu-id="be3ec-115">IBM I</span></span>
* <span data-ttu-id="be3ec-116">Informix</span><span class="sxs-lookup"><span data-stu-id="be3ec-116">Informix</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="be3ec-117">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="be3ec-117">Supported Platforms</span></span>

* <span data-ttu-id="be3ec-118">.NET Framework (4.6 oder höher)</span><span class="sxs-lookup"><span data-stu-id="be3ec-118">.NET Framework (4.6 onwards)</span></span>
* <span data-ttu-id="be3ec-119">.NET Core</span><span class="sxs-lookup"><span data-stu-id="be3ec-119">.NET Core</span></span>
