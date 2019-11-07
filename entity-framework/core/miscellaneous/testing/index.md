---
title: Testen von Komponenten mit EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655786"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="b09e9-102">Testen von Komponenten mit EF Core</span><span class="sxs-lookup"><span data-stu-id="b09e9-102">Testing components using EF Core</span></span>

<span data-ttu-id="b09e9-103">Sie können Komponenten testen, indem Sie Tools verwenden, die ohne den Overhead der eigentlichen I/O-Datenbankvorgänge eine Verbindung mit der realen Datenbank herstellen.</span><span class="sxs-lookup"><span data-stu-id="b09e9-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="b09e9-104">Es gibt zwei wesentliche Optionen für diese Vorgänge:</span><span class="sxs-lookup"><span data-stu-id="b09e9-104">There are two main options for doing this:</span></span>

* <span data-ttu-id="b09e9-105">Der [SQLite-In-Memory-Modus](sqlite.md) erlaubt es Ihnen, effiziente Tests für einen Anbieter zu schreiben, der sich wie eine relationale Datenbank verhält.</span><span class="sxs-lookup"><span data-stu-id="b09e9-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
* <span data-ttu-id="b09e9-106">[Der InMemory-Anbieter](in-memory.md) ist ein einfacher Anbieter, der minimale Abhängigkeiten aufweist, sich aber nicht immer wie eine relationale Datenbank verhält.</span><span class="sxs-lookup"><span data-stu-id="b09e9-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
