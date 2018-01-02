---
title: "Npgsql-Datenbankanbieter für PostgreSQL – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="53873-102">Npgsql-EF Core-Datenbankanbieter für PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="53873-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="53873-103">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="53873-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="53873-104">Der Anbieter wird im Rahmen des [Npgsql-Projekts](http://www.npgsql.org) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="53873-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="53873-105">Dieser Anbieter wird nicht im Rahmen des Entity Framework Core-Projekts verwaltet.</span><span class="sxs-lookup"><span data-stu-id="53873-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="53873-106">Wenn Sie einen Drittanbieter in Betracht ziehen, sollten Sie Aspekte wie Qualität, Lizenzierung und Support auswerten, um sicherzustellen, dass dieser Ihren Anforderungen entspricht.</span><span class="sxs-lookup"><span data-stu-id="53873-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="53873-107">Installieren</span><span class="sxs-lookup"><span data-stu-id="53873-107">Install</span></span>

<span data-ttu-id="53873-108">Installieren Sie das [NuGet-Paket „Npgsql.EntityFrameworkCore.PostgreSQL“](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="53873-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="53873-109">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="53873-109">Get Started</span></span>

<span data-ttu-id="53873-110">Informationen zu den ersten Schritten finden Sie in der [Npgsql-Dokumentation](http://www.npgsql.org/efcore/index.html).</span><span class="sxs-lookup"><span data-stu-id="53873-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="53873-111">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="53873-111">Supported Database Engines</span></span>

* <span data-ttu-id="53873-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="53873-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="53873-113">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="53873-113">Supported Platforms</span></span>

* <span data-ttu-id="53873-114">.NET Framework (4.5.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="53873-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="53873-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="53873-115">.NET Core</span></span>

* <span data-ttu-id="53873-116">Mono (4.2.0 oder höher)</span><span class="sxs-lookup"><span data-stu-id="53873-116">Mono (4.2.0 onwards)</span></span>
