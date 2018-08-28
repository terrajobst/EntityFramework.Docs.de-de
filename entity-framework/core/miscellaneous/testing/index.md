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
# <a name="testing"></a>Test

Sie können Komponenten testen, indem Sie Tools verwenden, die ohne den Overhead der eigentlichen I/O-Datenbankvorgänge eine Verbindung mit der realen Datenbank herstellen.

Es gibt zwei wesentliche Optionen für diese Vorgänge:
 * Der [SQLite-In-Memory-Modus](sqlite.md) erlaubt es Ihnen, effiziente Tests für einen Anbieter zu schreiben, der sich wie eine relationale Datenbank verhält.
 * [Der InMemory-Anbieter](in-memory.md) ist ein einfacher Anbieter, der minimale Abhängigkeiten aufweist, sich aber nicht immer wie eine relationale Datenbank verhält.
