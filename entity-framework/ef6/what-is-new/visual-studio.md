---
title: Visual Studio-Versionen – EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: 16bcdc6d0e7c5632d4f4c06ba285a7a666f24204
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022248"
---
# <a name="visual-studio-releases"></a>Visual Studio-Versionen

Es wird empfohlen, um immer die neueste Version von Visual Studio zu verwenden, da sie die neuesten Tools für .NET, NuGet und Entity Framework enthält.
In der Tat wird davon ausgegangen die verschiedene Beispiele und exemplarische Vorgehensweisen, in der Dokumentation zu Entity Framework, dass Sie eine aktuelle Version von Visual Studio verwenden.

Es ist möglich, jedoch mit ältere Versionen von Visual Studio mit unterschiedlichen Versionen von Entity Framework, solange Sie berücksichtigen einige Unterschiede berücksichtigt werden:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15.7 und höher

- Diese Version von Visual Studio umfasst die neueste Version von Entity Framework-Tools und die EF 6.2-Runtime, und es ist keine zusätzliche Installationsschritte erforderlich.
Finden Sie unter [neues](~/ef6/what-is-new/index.md) für Weitere Informationen zu diesen Versionen.
- Hinzufügen von Entity Framework auf neue Projekte, die mit den EF-Tools wird automatisch das EF 6.2-NuGet-Paket hinzugefügt.
Sie können manuell installieren oder aktualisieren Sie online auf EF-NuGet-Pakets zur Verfügung.
- Standardmäßig ist SQL Server-Instanz zur Verfügung, mit dieser Version von Visual Studio eine LocalDB-Instanz namens MSSQLLocalDB.
Der Serverabschnitt der Verbindungszeichenfolge, die Sie verwenden sollten ist "(Localdb)\\MSSQLLocalDB".
Denken Sie daran, verwenden Sie eine ausführliche Zeichenfolge, die mit dem Präfix `@` oder umgekehrter Schrägstriche doppelte "\\\\" Wenn Sie eine Verbindungszeichenfolge in der C#-Code angeben.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 zu Visual Studio 2017 15.6

- Diese Versionen von Visual Studio umfassen Entity Framework-Tools und Runtime 6.1.3.
Finden Sie unter [vergangenen Releases](~/ef6/what-is-new/past-releases.md#ef-613) für Weitere Informationen zu diesen Versionen.
- Entity Framework auf neue Projekte, die mit den EF-Tools automatisch hinzufügen wird EF 6.1.3 NuGet-Paket.
Sie können manuell installieren oder aktualisieren Sie online auf EF-NuGet-Pakets zur Verfügung.
- Standardmäßig ist SQL Server-Instanz zur Verfügung, mit dieser Version von Visual Studio eine LocalDB-Instanz namens MSSQLLocalDB.
Der Serverabschnitt der Verbindungszeichenfolge, die Sie verwenden sollten ist "(Localdb)\\MSSQLLocalDB".
Denken Sie daran, verwenden Sie eine ausführliche Zeichenfolge, die mit dem Präfix `@` oder umgekehrter Schrägstriche doppelte "\\\\" Wenn Sie eine Verbindungszeichenfolge in der C#-Code angeben.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Diese Version von Visual Studio enthält und die ältere Version von Entity Framework-Tools und Runtime.
Es wird empfohlen, ein upgrade auf Entity Framework-Tools 6.1.3, mithilfe von [Installationsprogramm](https://www.microsoft.com/download/details.aspx?id=40762) im Microsoft Download Center verfügbar.
Finden Sie unter [vergangenen Releases](~/ef6/what-is-new/past-releases.md#ef-613) für Weitere Informationen zu diesen Versionen.
- Entity Framework auf neue Projekte, die mit den aktualisierten EF-Tools automatisch hinzufügen wird EF 6.1.3 NuGet-Paket.
Sie können manuell installieren oder aktualisieren Sie online auf EF-NuGet-Pakets zur Verfügung.
- Standardmäßig ist SQL Server-Instanz zur Verfügung, mit dieser Version von Visual Studio eine LocalDB-Instanz namens MSSQLLocalDB.
Der Serverabschnitt der Verbindungszeichenfolge, die Sie verwenden sollten ist "(Localdb)\\MSSQLLocalDB".
Denken Sie daran, verwenden Sie eine ausführliche Zeichenfolge, die mit dem Präfix `@` oder umgekehrter Schrägstriche doppelte "\\\\" Wenn Sie eine Verbindungszeichenfolge in der C#-Code angeben.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Diese Version von Visual Studio enthält und die ältere Version von Entity Framework-Tools und Runtime.
Es wird empfohlen, ein upgrade auf Entity Framework-Tools 6.1.3, mithilfe von [Installationsprogramm](https://www.microsoft.com/download/details.aspx?id=40762) im Microsoft Download Center verfügbar.
Finden Sie unter [vergangenen Releases](~/ef6/what-is-new/past-releases.md#ef-613) für Weitere Informationen zu diesen Versionen.
- Entity Framework auf neue Projekte, die mit den aktualisierten EF-Tools automatisch hinzufügen wird EF 6.1.3 NuGet-Paket.
Sie können manuell installieren oder aktualisieren Sie online auf EF-NuGet-Pakets zur Verfügung.
- Standardmäßig ist SQL Server-Instanz zur Verfügung, mit dieser Version von Visual Studio eine LocalDB-Instanz namens V11. 0.
Der Serverabschnitt der Verbindungszeichenfolge, die Sie verwenden sollten ist "(Localdb)\\V11. 0".
Denken Sie daran, verwenden Sie eine ausführliche Zeichenfolge, die mit dem Präfix `@` oder umgekehrter Schrägstriche doppelte "\\\\" Wenn Sie eine Verbindungszeichenfolge in der C#-Code angeben.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- Die Version von Entity Framework-Tools zur Verfügung, mit dieser Version von Visual Studio ist nicht mit der Runtime für Entity Framework 6 kompatibel und kann nicht aktualisiert werden.
- Standardmäßig werden das Entity Framework-Tools Entity Framework 4.0 zu Ihren Projekten hinzufügen.
Um Anwendungen mit alle neueren Versionen von EF zu erstellen, müssen Sie zuerst zum Installieren der [NuGet-Paket-Manager-Erweiterung](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- In der Standardeinstellung ist alle codegenerierung in der Version von EF-Tools "EntityObject" und Entity Framework 4 abhängig.
Es wird empfohlen, dass Sie die codegenerierung für "DbContext" und Entity Framework 5, basieren soll, installieren Sie die "DbContext" Code generieren-Vorlagen für wechseln [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) oder [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- Nachdem Sie die NuGet-Paket-Manager-Erweiterungen installiert haben, können Sie manuell installieren oder aktualisieren Sie online auf EF-NuGet-Pakets zur Verfügung und wird mit EF6 Code First, das nicht mit einen Designer erfordert.
- Standardmäßig ist der SQL Server-Instanz, die mit dieser Version von Visual Studio verfügbaren SQL Server Express mit dem Namen ' SQLEXPRESS.
Ist der Server Teil der Verbindungszeichenfolge sollten Sie verwenden ". \\SQLEXPRESS ".
Denken Sie daran, verwenden Sie eine ausführliche Zeichenfolge, die mit dem Präfix `@` oder umgekehrter Schrägstriche doppelte "\\\\" Wenn Sie eine Verbindungszeichenfolge in der C#-Code angeben.
