---
title: 'Asynchrone Abfragen: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052680"
---
# <a name="asynchronous-queries"></a>Asynchrone Abfragen

Asynchrone Abfragen vermeiden, dass ein Thread blockiert wird, während die Abfrage in der Datenbank ausgeführt wird. Dies hilft dabei, das Abstürzen der Benutzeroberfläche einer Thick-Client-Anwendung zu vermeiden. Asynchrone Vorgänge können außerdem den Durchsatz in Webanwendungen erhöhen, in denen der Thread für andere Anforderungen freigegeben werden kann, während der Datenbankvorgang abgeschlossen wird. Weitere Informationen finden Sie unter [Asynchrone Programmierung in C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core unterstützt nicht die Ausführung mehrerer paralleler Vorgänge, die auf derselben Kontextinstanz ausgeführt werden. Sie sollten immer auf den Abschluss eines Vorgangs warten, bevor Sie den nächsten starten. In der Regel erfolgt dies für alle asynchronen Vorgänge durch das Schlüsselwort `await`.

Entity Framework Core stellt eine Reihe von asynchronen Erweiterungsmethoden bereit, die alternativ zu den LINQ-Methoden verwendet werden können, die die Ausführung einer Abfrage und die Rückgabe von Ergebnissen auslösen. Beispiele dafür sind `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()` usw. Es gibt nicht asynchrone Versionen von LINQ-Operatoren, z.B. `Where(...)`, `OrderBy(...)` usw., da diese Methoden nur die LINQ-Ausdrucksbaumstruktur erstellen und nicht die Abfrage in der Datenbank auslösen.

> [!IMPORTANT]  
> Die asynchronen Erweiterungsmethoden von EF Core werden im Namespace `Microsoft.EntityFrameworkCore` definiert. Dieser Namespace muss importiert werden, damit die Methoden verfügbar sind.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
