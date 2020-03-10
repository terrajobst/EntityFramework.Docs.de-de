---
title: Testen von Komponenten mit EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412795"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="0782b-102">Testen von Komponenten mit EF Core</span><span class="sxs-lookup"><span data-stu-id="0782b-102">Testing components using EF Core</span></span>

<span data-ttu-id="0782b-103">Sie können Komponenten testen, indem Sie Tools verwenden, die ohne den Overhead der eigentlichen I/O-Datenbankvorgänge eine Verbindung mit der realen Datenbank herstellen.</span><span class="sxs-lookup"><span data-stu-id="0782b-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="0782b-104">Es gibt zwei wesentliche Optionen für diese Vorgänge:</span><span class="sxs-lookup"><span data-stu-id="0782b-104">There are two main options for doing this:</span></span>

* <span data-ttu-id="0782b-105">Der [SQLite-In-Memory-Modus](sqlite.md) erlaubt es Ihnen, effiziente Tests für einen Anbieter zu schreiben, der sich wie eine relationale Datenbank verhält.</span><span class="sxs-lookup"><span data-stu-id="0782b-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
* <span data-ttu-id="0782b-106">[Der InMemory-Anbieter](in-memory.md) ist ein einfacher Anbieter, der minimale Abhängigkeiten aufweist, sich aber nicht immer wie eine relationale Datenbank verhält.</span><span class="sxs-lookup"><span data-stu-id="0782b-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
