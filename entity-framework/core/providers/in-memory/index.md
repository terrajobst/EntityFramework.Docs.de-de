---
title: In-Memory Database-Anbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 4b35e8c4b29a951449d4a26c6e274eb3015069bc
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656021"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="891a9-102">EF Core-In-Memory Database-Anbieter</span><span class="sxs-lookup"><span data-stu-id="891a9-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="891a9-103">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="891a9-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="891a9-104">Dies kann für Tests nützlich sein, obwohl der SQLite-Anbieter im In-Memory-Modus für Testersatz für relationale Datenbanken besser geeignet sein kann.</span><span class="sxs-lookup"><span data-stu-id="891a9-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="891a9-105">Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="891a9-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="891a9-106">Installieren</span><span class="sxs-lookup"><span data-stu-id="891a9-106">Install</span></span>

<span data-ttu-id="891a9-107">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.InMemory“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="891a9-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="891a9-108">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="891a9-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="891a9-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="891a9-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="891a9-110">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="891a9-110">Get Started</span></span>

<span data-ttu-id="891a9-111">Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:</span><span class="sxs-lookup"><span data-stu-id="891a9-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="891a9-112">Testen mit InMemory</span><span class="sxs-lookup"><span data-stu-id="891a9-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="891a9-113">Tests der UnicornStore-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="891a9-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="891a9-114">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="891a9-114">Supported Database Engines</span></span>

<span data-ttu-id="891a9-115">In-Process-Speicherdatenbank (nur für Testzwecke konzipiert)</span><span class="sxs-lookup"><span data-stu-id="891a9-115">In-process memory database (designed for testing purposes only)</span></span>
