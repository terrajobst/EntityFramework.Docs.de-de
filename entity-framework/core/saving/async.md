---
title: 'Asynchroner Speichervorgang: EF Core'
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413690"
---
# <a name="asynchronous-saving"></a>Asynchroner Speichervorgang

Mit dem asynchronen Speichervorgang wird vermieden, dass ein Thread blockiert wird, während die Änderungen in die Datenbank geschrieben werden. Dies hilft dabei, das Abstürzen der Benutzeroberfläche einer Thick-Client-Anwendung zu vermeiden. Asynchrone Vorgänge können außerdem den Durchsatz in Webanwendungen erhöhen, in denen der Thread für andere Anforderungen freigegeben werden kann, während der Datenbankvorgang abgeschlossen wird. Weitere Informationen finden Sie unter [Asynchrone Programmierung in C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core unterstützt nicht die Ausführung mehrerer paralleler Vorgänge, die auf derselben Kontextinstanz ausgeführt werden. Sie sollten immer auf den Abschluss eines Vorgangs warten, bevor Sie den nächsten starten. In der Regel erfolgt dies für alle asynchronen Vorgänge durch das Schlüsselwort `await`.

Entity Framework Core stellt `DbContext.SaveChangesAsync()` als asynchrone Alternative zu `DbContext.SaveChanges()` bereit.

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
