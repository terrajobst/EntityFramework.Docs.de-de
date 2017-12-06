---
title: Asynchrone speichern - EF Core
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-saving"></a>Asynchrone speichern

Asynchrone speichern vermeidet einen Thread blockiert, während die Änderungen in die Datenbank geschrieben werden. Dies kann nützlich, um zu vermeiden, fixieren die Benutzeroberfläche einer Anwendung dick-Client sein. Asynchrone Vorgänge können auch in einer Web-Anwendung Durchsatz zu erhöhen, in dem der Thread freigegeben werden kann, um andere Anforderungen zu verarbeiten, während die Datenbankvorgang abgeschlossen wird. Weitere Informationen finden Sie unter [asynchrone Programmierung in c#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core unterstützt nicht mehrere parallele Vorgänge, die auf dieselbe Kontextinstanz ausgeführt werden. Sie sollten immer vor dem nächsten Vorgang Abschluss eines Vorgangs zu warten. Dies erfolgt in der Regel mithilfe der `await` Schlüsselwort bei jedem asynchronen Vorgang.

Entity Framework Core bietet `DbContext.SaveChangesAsync()` als Alternative zur asynchronen `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
