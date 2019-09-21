---
title: In-Memory Database-Anbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 28f5f262b41cbc1f196e41d75c8b88ca60e678fe
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149240"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="b11dd-102">EF Core-In-Memory Database-Anbieter</span><span class="sxs-lookup"><span data-stu-id="b11dd-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="b11dd-103">Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b11dd-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="b11dd-104">Dies kann für Tests nützlich sein, obwohl der SQLite-Anbieter im In-Memory-Modus für Testersatz für relationale Datenbanken besser geeignet sein kann.</span><span class="sxs-lookup"><span data-stu-id="b11dd-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="b11dd-105">Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="b11dd-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="b11dd-106">Installieren</span><span class="sxs-lookup"><span data-stu-id="b11dd-106">Install</span></span>

<span data-ttu-id="b11dd-107">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.InMemory“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="b11dd-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="b11dd-108">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="b11dd-108">Get Started</span></span>

<span data-ttu-id="b11dd-109">Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:</span><span class="sxs-lookup"><span data-stu-id="b11dd-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="b11dd-110">Testen mit InMemory</span><span class="sxs-lookup"><span data-stu-id="b11dd-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="b11dd-111">Tests der UnicornStore-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="b11dd-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="b11dd-112">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="b11dd-112">Supported Database Engines</span></span>

* <span data-ttu-id="b11dd-113">Integrierte In-Memory Database (nur für Testzwecke konzipiert)</span><span class="sxs-lookup"><span data-stu-id="b11dd-113">Built-in in-memory database (designed for testing purposes only)</span></span>
