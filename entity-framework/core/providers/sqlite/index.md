---
title: "SQLite-Datenbankanbieter – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="bf792-102">SQLite-EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="bf792-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="bf792-103">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit SQLite.</span><span class="sxs-lookup"><span data-stu-id="bf792-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="bf792-104">Der Anbieter wird im Rahmen des [GitHub-Projekts „EntityFramework“](https://github.com/aspnet/EntityFramework) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="bf792-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="bf792-105">Installieren</span><span class="sxs-lookup"><span data-stu-id="bf792-105">Install</span></span>

<span data-ttu-id="bf792-106">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.Sqlite“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="bf792-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="bf792-107">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="bf792-107">Get Started</span></span>

<span data-ttu-id="bf792-108">Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:</span><span class="sxs-lookup"><span data-stu-id="bf792-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="bf792-109">Lokaler SQLite-Anbieter in UWP</span><span class="sxs-lookup"><span data-stu-id="bf792-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="bf792-110">.NET Core-Anwendung für die neue SQLite-Datenbank</span><span class="sxs-lookup"><span data-stu-id="bf792-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="bf792-111">Unicorn Clicker-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="bf792-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="bf792-112">Unicorn Packer-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="bf792-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="bf792-113">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="bf792-113">Supported Database Engines</span></span>

* <span data-ttu-id="bf792-114">SQLite (3.7 oder höher)</span><span class="sxs-lookup"><span data-stu-id="bf792-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="bf792-115">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="bf792-115">Supported Platforms</span></span>

* <span data-ttu-id="bf792-116">.NET Framework (4.5.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="bf792-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="bf792-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf792-117">.NET Core</span></span>

* <span data-ttu-id="bf792-118">Mono (4.2.0 oder höher)</span><span class="sxs-lookup"><span data-stu-id="bf792-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="bf792-119">Universelle Windows-Plattform</span><span class="sxs-lookup"><span data-stu-id="bf792-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="bf792-120">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="bf792-120">Limitations</span></span>

<span data-ttu-id="bf792-121">Einige wichtige Einschränkungen zum SQLite-Anbieter finden Sie unter [Einschränkungen zu SQLite](limitations.md).</span><span class="sxs-lookup"><span data-stu-id="bf792-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
