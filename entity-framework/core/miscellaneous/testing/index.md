---
title: Testen von Komponenten mit Entity Framework – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997836"
---
# <a name="testing"></a><span data-ttu-id="47a2c-102">Test</span><span class="sxs-lookup"><span data-stu-id="47a2c-102">Testing</span></span>

<span data-ttu-id="47a2c-103">Sie können Komponenten testen, indem Sie Tools verwenden, die ohne den Overhead der eigentlichen I/O-Datenbankvorgänge eine Verbindung mit der realen Datenbank herstellen.</span><span class="sxs-lookup"><span data-stu-id="47a2c-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="47a2c-104">Es gibt zwei wesentliche Optionen für diese Vorgänge:</span><span class="sxs-lookup"><span data-stu-id="47a2c-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="47a2c-105">Der [SQLite-In-Memory-Modus](sqlite.md) erlaubt es Ihnen, effiziente Tests für einen Anbieter zu schreiben, der sich wie eine relationale Datenbank verhält.</span><span class="sxs-lookup"><span data-stu-id="47a2c-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="47a2c-106">[Der InMemory-Anbieter](in-memory.md) ist ein einfacher Anbieter, der minimale Abhängigkeiten aufweist, sich aber nicht immer wie eine relationale Datenbank verhält.</span><span class="sxs-lookup"><span data-stu-id="47a2c-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
