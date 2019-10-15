---
title: 'Asynchrone Abfragen: EF Core'
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181824"
---
# <a name="asynchronous-queries"></a>Asynchrone Abfragen

Asynchrone Abfragen vermeiden, dass ein Thread blockiert wird, während die Abfrage in der Datenbank ausgeführt wird. Asynchrone Abfragen sind wichtig, um die Reaktionsfähigkeit der Benutzeroberfläche in Thick-Clientanwendungen zu gewährleisten. Sie können auch den Durchsatz von Webanwendungen erhöhen, indem sie den Thread für die Behandlung anderer Anforderungen in Webanwendungen freigegeben. Weitere Informationen finden Sie unter [Asynchrone Programmierung in C#](/dotnet/csharp/async).

> [!WARNING]  
> EF Core unterstützt nicht die Ausführung mehrerer paralleler Vorgänge, die auf derselben Kontextinstanz ausgeführt werden. Sie sollten immer auf den Abschluss eines Vorgangs warten, bevor Sie den nächsten starten. In der Regel erfolgt dies für alle asynchronen Vorgänge durch das Schlüsselwort `await`.

Entity Framework Core stellt asynchrone Erweiterungsmethoden bereit, die den LINQ-Methoden ähneln, die eine Abfrage ausführen und Ergebnisse zurückgeben. Beispiele dafür sind. `ToListAsync()`, `ToArrayAsync()` und `SingleAsync()`. Für einige LINQ-Operatoren, z. B. `Where(...)` oder `OrderBy(...)`, gibt es keine asynchronen Versionen, da diese Methoden nur die LINQ-Ausdrucksbaumstruktur erstellen und nicht die Ausführung der Abfrage in der Datenbank auslösen.

> [!IMPORTANT]  
> Die asynchronen Erweiterungsmethoden von EF Core werden im Namespace `Microsoft.EntityFrameworkCore` definiert. Dieser Namespace muss importiert werden, damit die Methoden verfügbar sind.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
