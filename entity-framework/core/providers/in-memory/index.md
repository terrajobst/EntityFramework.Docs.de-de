---
title: "In-Memory Database-Anbieter – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="ff4ae-102">EF Core-In-Memory Database-Anbieter</span><span class="sxs-lookup"><span data-stu-id="ff4ae-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="ff4ae-103">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ff4ae-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="ff4ae-104">Dies ist beim Testen von Code nützlich, der Entity Framework Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="ff4ae-104">This is useful when testing code that uses Entity Framework Core.</span></span> <span data-ttu-id="ff4ae-105">Der Anbieter wird im Rahmen des [GitHub-Projekts „EntityFramework“](https://github.com/aspnet/EntityFramework) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="ff4ae-105">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="ff4ae-106">Installieren</span><span class="sxs-lookup"><span data-stu-id="ff4ae-106">Install</span></span>

<span data-ttu-id="ff4ae-107">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.InMemory“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="ff4ae-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="ff4ae-108">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="ff4ae-108">Get Started</span></span>

<span data-ttu-id="ff4ae-109">Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:</span><span class="sxs-lookup"><span data-stu-id="ff4ae-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="ff4ae-110">Testen mit InMemory</span><span class="sxs-lookup"><span data-stu-id="ff4ae-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="ff4ae-111">Tests der UnicornStore-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="ff4ae-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="ff4ae-112">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="ff4ae-112">Supported Database Engines</span></span>

* <span data-ttu-id="ff4ae-113">Integrierte In-Memory Database (nur für Testzwecke konzipiert)</span><span class="sxs-lookup"><span data-stu-id="ff4ae-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="ff4ae-114">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="ff4ae-114">Supported Platforms</span></span>

* <span data-ttu-id="ff4ae-115">.NET Framework (4.5.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="ff4ae-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="ff4ae-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff4ae-116">.NET Core</span></span>

* <span data-ttu-id="ff4ae-117">Mono (4.2.0 oder höher)</span><span class="sxs-lookup"><span data-stu-id="ff4ae-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="ff4ae-118">Universelle Windows-Plattform</span><span class="sxs-lookup"><span data-stu-id="ff4ae-118">Universal Windows Platform</span></span>
