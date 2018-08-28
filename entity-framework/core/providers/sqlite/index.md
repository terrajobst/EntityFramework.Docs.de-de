---
title: SQLite-Datenbankanbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 31de8449a12a10d4f98ebb4bb6125389606e9bbd
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994001"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="164b4-102">SQLite-EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="164b4-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="164b4-103">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit SQLite.</span><span class="sxs-lookup"><span data-stu-id="164b4-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="164b4-104">Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="164b4-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="164b4-105">Installieren</span><span class="sxs-lookup"><span data-stu-id="164b4-105">Install</span></span>

<span data-ttu-id="164b4-106">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.Sqlite“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="164b4-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="164b4-107">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="164b4-107">Get Started</span></span>

<span data-ttu-id="164b4-108">Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:</span><span class="sxs-lookup"><span data-stu-id="164b4-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="164b4-109">Lokaler SQLite-Anbieter in UWP</span><span class="sxs-lookup"><span data-stu-id="164b4-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="164b4-110">.NET Core-Anwendung für die neue SQLite-Datenbank</span><span class="sxs-lookup"><span data-stu-id="164b4-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="164b4-111">Unicorn Clicker-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="164b4-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="164b4-112">Unicorn Packer-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="164b4-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="164b4-113">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="164b4-113">Supported Database Engines</span></span>

* <span data-ttu-id="164b4-114">SQLite (3.7 oder höher)</span><span class="sxs-lookup"><span data-stu-id="164b4-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="164b4-115">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="164b4-115">Supported Platforms</span></span>

* <span data-ttu-id="164b4-116">.NET Framework (4.5.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="164b4-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="164b4-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="164b4-117">.NET Core</span></span>

* <span data-ttu-id="164b4-118">Mono (4.2.0 oder höher)</span><span class="sxs-lookup"><span data-stu-id="164b4-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="164b4-119">Universelle Windows-Plattform</span><span class="sxs-lookup"><span data-stu-id="164b4-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="164b4-120">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="164b4-120">Limitations</span></span>

<span data-ttu-id="164b4-121">Einige wichtige Einschränkungen zum SQLite-Anbieter finden Sie unter [Einschränkungen zu SQLite](limitations.md).</span><span class="sxs-lookup"><span data-stu-id="164b4-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
