---
title: In-Memory Database-Anbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: ca802f55d973b9f79073c2507c1e0c7a853a1fce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993319"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="80e5a-102">EF Core-In-Memory Database-Anbieter</span><span class="sxs-lookup"><span data-stu-id="80e5a-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="80e5a-103">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="80e5a-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="80e5a-104">Dies kann für Tests nützlich sein, obwohl der SQLite-Anbieter im In-Memory-Modus für Testersatz für relationale Datenbanken besser geeignet sein kann.</span><span class="sxs-lookup"><span data-stu-id="80e5a-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="80e5a-105">Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="80e5a-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="80e5a-106">Installieren</span><span class="sxs-lookup"><span data-stu-id="80e5a-106">Install</span></span>

<span data-ttu-id="80e5a-107">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.InMemory“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="80e5a-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="80e5a-108">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="80e5a-108">Get Started</span></span>

<span data-ttu-id="80e5a-109">Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:</span><span class="sxs-lookup"><span data-stu-id="80e5a-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="80e5a-110">Testen mit InMemory</span><span class="sxs-lookup"><span data-stu-id="80e5a-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="80e5a-111">Tests der UnicornStore-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="80e5a-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="80e5a-112">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="80e5a-112">Supported Database Engines</span></span>

* <span data-ttu-id="80e5a-113">Integrierte In-Memory Database (nur für Testzwecke konzipiert)</span><span class="sxs-lookup"><span data-stu-id="80e5a-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="80e5a-114">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="80e5a-114">Supported Platforms</span></span>

* <span data-ttu-id="80e5a-115">.NET Framework (4.5.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="80e5a-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="80e5a-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="80e5a-116">.NET Core</span></span>

* <span data-ttu-id="80e5a-117">Mono (4.2.0 oder höher)</span><span class="sxs-lookup"><span data-stu-id="80e5a-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="80e5a-118">Universelle Windows-Plattform</span><span class="sxs-lookup"><span data-stu-id="80e5a-118">Universal Windows Platform</span></span>
