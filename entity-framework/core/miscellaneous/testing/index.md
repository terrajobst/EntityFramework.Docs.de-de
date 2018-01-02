---
title: "Testen von Komponenten mit Entity Framework – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="testing"></a><span data-ttu-id="4261a-102">Test</span><span class="sxs-lookup"><span data-stu-id="4261a-102">Testing</span></span>

<span data-ttu-id="4261a-103">Sie können Komponenten testen, indem Sie Tools verwenden, die ohne den Overhead der eigentlichen I/O-Datenbankvorgänge eine Verbindung mit der realen Datenbank herstellen.</span><span class="sxs-lookup"><span data-stu-id="4261a-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="4261a-104">Es gibt zwei wesentliche Optionen für diese Vorgänge:</span><span class="sxs-lookup"><span data-stu-id="4261a-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="4261a-105">Der [SQLite-In-Memory-Modus](sqlite.md) erlaubt es Ihnen, effiziente Tests für einen Anbieter zu schreiben, der sich wie eine relationale Datenbank verhält.</span><span class="sxs-lookup"><span data-stu-id="4261a-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="4261a-106">[Der InMemory-Anbieter](in-memory.md) ist ein einfacher Anbieter, der minimale Abhängigkeiten aufweist, sich aber nicht immer wie eine relationale Datenbank verhält.</span><span class="sxs-lookup"><span data-stu-id="4261a-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
