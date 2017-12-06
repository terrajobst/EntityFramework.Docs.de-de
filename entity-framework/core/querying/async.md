---
title: "Asynchrone Abfragen – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a>Asynchrone Abfragen

Asynchrone Abfragen vermeiden, einen Thread zu blockieren, während die Abfrage in der Datenbank ausgeführt wird. Dies kann nützlich, um zu vermeiden, fixieren die Benutzeroberfläche einer Anwendung dick-Client sein. Asynchrone Vorgänge können auch in einer Web-Anwendung Durchsatz zu erhöhen, in dem der Thread freigegeben werden kann, um andere Anforderungen zu verarbeiten, während die Datenbankvorgang abgeschlossen wird. Weitere Informationen finden Sie unter [asynchrone Programmierung in c#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core unterstützt nicht mehrere parallele Vorgänge, die auf dieselbe Kontextinstanz ausgeführt werden. Sie sollten immer vor dem nächsten Vorgang Abschluss eines Vorgangs zu warten. Dies erfolgt in der Regel mithilfe der `await` Schlüsselwort bei jedem asynchronen Vorgang.

Entity Framework Core bietet eine Reihe von asynchronen Erweiterungsmethoden, die verwendet werden können, als Alternative zu den LINQ-Methoden, die dazu führen, dass eine Abfrage ausgeführt werden und die Ergebnisse zurückgegeben. Beispiele hierfür sind `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`usw. Es sind nicht asynchronen Versionen der LINQ-Operatoren wie z. B. `Where(...)`, `OrderBy(...)`usw., da diese Methoden nur die LINQ-Ausdrucksbaumstruktur zu erstellen und führen nicht dazu, dass die Abfrage in der Datenbank ausgeführt werden.

> [!IMPORTANT]  
> Die EF Core Async-Erweiterungsmethoden werden definiert, der `Microsoft.EntityFrameworkCore` Namespace. Dieser Namespace muss für die Methoden zur Verfügung stehen, importiert werden.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
