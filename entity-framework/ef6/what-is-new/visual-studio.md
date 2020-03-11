---
title: Visual Studio-Releases EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: 16bcdc6d0e7c5632d4f4c06ba285a7a666f24204
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414356"
---
# <a name="visual-studio-releases"></a>Visual Studio-Releases

Es wird empfohlen, immer die neueste Version von Visual Studio zu verwenden, da Sie die neuesten Tools für .net, nuget und Entity Framework enthält.
In den verschiedenen Beispielen und exemplarischen Vorgehensweisen in der Entity Framework-Dokumentation wird davon ausgegangen, dass Sie eine aktuelle Version von Visual Studio verwenden.

Es ist jedoch möglich, ältere Versionen von Visual Studio mit unterschiedlichen Versionen von Entity Framework zu verwenden, solange Sie einige Unterschiede berücksichtigen:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15,7 und höher

- Diese Version von Visual Studio enthält die neueste Version von Entity Framework Tools und die EF 6,2-Laufzeit und erfordert keine zusätzlichen Einrichtungsschritte.
Weitere Informationen zu diesen Releases finden Sie unter [Neuerungen](~/ef6/what-is-new/index.md) .
- Durch das Hinzufügen von Entity Framework zu neuen Projekten mit den EF-Tools wird das nuget-Paket EF 6,2 automatisch hinzugefügt.
Sie können ein beliebiges EF-nuget-Paket, das Online verfügbar ist, manuell installieren oder aktualisieren.
- Standardmäßig ist die SQL Server-Instanz, die mit dieser Version von Visual Studio verfügbar ist, eine localdb-Instanz mit dem Namen "mssqllocaldb".
Der Server Abschnitt der Verbindungs Zeichenfolge, die Sie verwenden sollten, ist "(localdb)\\mssqllocaldb".
Denken Sie daran, eine ausführliche Zeichenfolge zu verwenden, die `@` oder doppelten rückstrichen "\\\\" vorangestellt ist, wenn C# Sie eine Verbindungs Zeichenfolge im Code angeben.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 zu Visual Studio 2017 15,6

- Diese Versionen von Visual Studio enthalten Entity Framework Tools und Lauf Zeit 6.1.3.
Weitere Informationen zu diesen Releases finden Sie unter [frühere Versionen](~/ef6/what-is-new/past-releases.md#ef-613) .
- Durch das Hinzufügen von Entity Framework zu neuen Projekten mithilfe der EF-Tools wird das nuget-Paket EF 6.1.3 automatisch hinzugefügt.
Sie können ein beliebiges EF-nuget-Paket, das Online verfügbar ist, manuell installieren oder aktualisieren.
- Standardmäßig ist die SQL Server-Instanz, die mit dieser Version von Visual Studio verfügbar ist, eine localdb-Instanz mit dem Namen "mssqllocaldb".
Der Server Abschnitt der Verbindungs Zeichenfolge, die Sie verwenden sollten, ist "(localdb)\\mssqllocaldb".
Denken Sie daran, eine ausführliche Zeichenfolge zu verwenden, die `@` oder doppelten rückstrichen "\\\\" vorangestellt ist, wenn C# Sie eine Verbindungs Zeichenfolge im Code angeben.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Diese Version von Visual Studio enthält und eine ältere Version von Entity Framework Tools und Runtime.
Es wird empfohlen, dass Sie ein Upgrade auf Entity Framework Tools 6.1.3 durchführen, indem Sie das im Microsoft Download Center verfügbare [Installations](https://www.microsoft.com/download/details.aspx?id=40762) Programm verwenden.
Weitere Informationen zu diesen Releases finden Sie unter [frühere Versionen](~/ef6/what-is-new/past-releases.md#ef-613) .
- Durch das Hinzufügen von Entity Framework zu neuen Projekten mit den aktualisierten EF-Tools wird das nuget-Paket EF 6.1.3 automatisch hinzugefügt.
Sie können ein beliebiges EF-nuget-Paket, das Online verfügbar ist, manuell installieren oder aktualisieren.
- Standardmäßig ist die SQL Server-Instanz, die mit dieser Version von Visual Studio verfügbar ist, eine localdb-Instanz mit dem Namen "mssqllocaldb".
Der Server Abschnitt der Verbindungs Zeichenfolge, die Sie verwenden sollten, ist "(localdb)\\mssqllocaldb".
Denken Sie daran, eine ausführliche Zeichenfolge zu verwenden, die `@` oder doppelten rückstrichen "\\\\" vorangestellt ist, wenn C# Sie eine Verbindungs Zeichenfolge im Code angeben.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Diese Version von Visual Studio enthält und eine ältere Version von Entity Framework Tools und Runtime.
Es wird empfohlen, dass Sie ein Upgrade auf Entity Framework Tools 6.1.3 durchführen, indem Sie das im Microsoft Download Center verfügbare [Installations](https://www.microsoft.com/download/details.aspx?id=40762) Programm verwenden.
Weitere Informationen zu diesen Releases finden Sie unter [frühere Versionen](~/ef6/what-is-new/past-releases.md#ef-613) .
- Durch das Hinzufügen von Entity Framework zu neuen Projekten mit den aktualisierten EF-Tools wird das nuget-Paket EF 6.1.3 automatisch hinzugefügt.
Sie können ein beliebiges EF-nuget-Paket, das Online verfügbar ist, manuell installieren oder aktualisieren.
- Standardmäßig ist die SQL Server-Instanz, die mit dieser Version von Visual Studio verfügbar ist, eine localdb-Instanz mit dem Namen v 11.0.
Der Server Abschnitt der Verbindungs Zeichenfolge, die Sie verwenden sollten, ist "(localdb)\\v 11.0".
Denken Sie daran, eine ausführliche Zeichenfolge zu verwenden, die `@` oder doppelten rückstrichen "\\\\" vorangestellt ist, wenn C# Sie eine Verbindungs Zeichenfolge im Code angeben.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- Die Version von Entity Framework Tools, die in dieser Version von Visual Studio verfügbar ist, ist nicht mit der Laufzeit von Entity Framework 6 kompatibel und kann nicht aktualisiert werden.
- Standardmäßig werden den Projekten die Entity Framework Tools Entity Framework 4,0 hinzugefügt.
Um Anwendungen mit neueren Versionen von EF zu erstellen, müssen Sie zunächst die [nuget-Paket-Manager-Erweiterung](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager)installieren.
- Standardmäßig basiert die gesamte Codegenerierung in der-Version der EF-Tools auf EntityObject und Entity Framework 4.
Es wird empfohlen, dass Sie die Codegenerierung so ändern, dass Sie auf dbcontext und Entity Framework 5 basiert, indem Sie die dbcontext [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) -Code Generierungs Vorlagen für oder [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET)installieren.
- Nachdem Sie die Erweiterungen für den nuget-Paket-Manager installiert haben, können Sie ein beliebiges EF-nuget-Paket, das Online verfügbar ist, manuell installieren oder aktualisieren und EF6 mit Code First verwenden, für das kein Designer erforderlich ist.
- Standardmäßig ist die SQL Server-Instanz, die mit dieser Version von Visual Studio verfügbar ist, SQL Server Express SQLExpress.
Der Server Abschnitt der Verbindungs Zeichenfolge, die Sie verwenden sollten, ist ".\\SQLExpress ".
Denken Sie daran, eine ausführliche Zeichenfolge zu verwenden, die `@` oder doppelten rückstrichen "\\\\" vorangestellt ist, wenn C# Sie eine Verbindungs Zeichenfolge im Code angeben.
