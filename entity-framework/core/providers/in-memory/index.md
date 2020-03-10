---
title: In-Memory Database-Anbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413005"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="ea78f-102">EF Core-In-Memory Database-Anbieter</span><span class="sxs-lookup"><span data-stu-id="ea78f-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="ea78f-103">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ea78f-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="ea78f-104">Dies kann für Tests nützlich sein, obwohl der SQLite-Anbieter im In-Memory-Modus für Testersatz für relationale Datenbanken besser geeignet sein kann.</span><span class="sxs-lookup"><span data-stu-id="ea78f-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="ea78f-105">Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="ea78f-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="ea78f-106">Installieren</span><span class="sxs-lookup"><span data-stu-id="ea78f-106">Install</span></span>

<span data-ttu-id="ea78f-107">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.InMemory“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="ea78f-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="ea78f-108">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="ea78f-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[<span data-ttu-id="ea78f-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea78f-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="ea78f-110">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="ea78f-110">Get Started</span></span>

<span data-ttu-id="ea78f-111">Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:</span><span class="sxs-lookup"><span data-stu-id="ea78f-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="ea78f-112">Testen mit InMemory</span><span class="sxs-lookup"><span data-stu-id="ea78f-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="ea78f-113">Tests der UnicornStore-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="ea78f-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="ea78f-114">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="ea78f-114">Supported Database Engines</span></span>

<span data-ttu-id="ea78f-115">In-Process-Speicherdatenbank (nur für Testzwecke konzipiert)</span><span class="sxs-lookup"><span data-stu-id="ea78f-115">In-process memory database (designed for testing purposes only)</span></span>
