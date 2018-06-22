---
title: In-Memory Database-Anbieter – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678985"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="3d469-102">EF Core-In-Memory Database-Anbieter</span><span class="sxs-lookup"><span data-stu-id="3d469-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="3d469-103">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="3d469-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="3d469-104">Dies kann für Tests nützlich sein, obwohl der SQLite-Anbieter im In-Memory-Modus für Testersatz für relationale Datenbanken besser geeignet sein kann.</span><span class="sxs-lookup"><span data-stu-id="3d469-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="3d469-105">Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="3d469-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="3d469-106">Installieren</span><span class="sxs-lookup"><span data-stu-id="3d469-106">Install</span></span>

<span data-ttu-id="3d469-107">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.InMemory“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="3d469-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="3d469-108">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="3d469-108">Get Started</span></span>

<span data-ttu-id="3d469-109">Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:</span><span class="sxs-lookup"><span data-stu-id="3d469-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="3d469-110">Testen mit InMemory</span><span class="sxs-lookup"><span data-stu-id="3d469-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="3d469-111">Tests der UnicornStore-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="3d469-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="3d469-112">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="3d469-112">Supported Database Engines</span></span>

* <span data-ttu-id="3d469-113">Integrierte In-Memory Database (nur für Testzwecke konzipiert)</span><span class="sxs-lookup"><span data-stu-id="3d469-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="3d469-114">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="3d469-114">Supported Platforms</span></span>

* <span data-ttu-id="3d469-115">.NET Framework (4.5.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="3d469-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="3d469-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d469-116">.NET Core</span></span>

* <span data-ttu-id="3d469-117">Mono (4.2.0 oder höher)</span><span class="sxs-lookup"><span data-stu-id="3d469-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="3d469-118">Universelle Windows-Plattform</span><span class="sxs-lookup"><span data-stu-id="3d469-118">Universal Windows Platform</span></span>
