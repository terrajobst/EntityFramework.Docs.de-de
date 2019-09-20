---
title: Portieren von EF6 auf EF Core portieren eines edmx-basierten Modells
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: a1c978e141f47a39fff59eb5fc417a63afd37b04
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149000"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portieren eines EF6 edmx-basierten Modells auf EF Core

EF Core unterstützt das edmx-Dateiformat für Modelle nicht. Die beste Option zum Portieren dieser Modelle besteht darin, ein neues Code basiertes Modell aus der Datenbank für Ihre Anwendung zu generieren.

## <a name="install-ef-core-nuget-packages"></a>Installieren von EF Core nuget-Paketen

Installieren Sie das NuGet-Paket `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Erneutes Generieren des Modells

Sie können jetzt die Reverse-Engineering-Funktion verwenden, um ein Modell auf der Grundlage der vorhandenen Datenbank zu erstellen.

Führen Sie den folgenden Befehl in der Paket-Manager-Konsole aus (Tools – > nuget-Paket-Manager – > Paket-Manager-Konsole). In der [Paket-Manager-Konsole (Visual Studio)](../../core/miscellaneous/cli/powershell.md) finden Sie Befehlsoptionen zum Gerüstbau einer Teilmenge von Tabellen usw.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Hier sehen Sie z. b. den Befehl, um ein Modell aus der BLOB-Datenbank auf Ihrer SQL Server localdb-Instanz zu Gerüstbau.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Entfernen des EF6-Modells

Nun entfernen Sie das EF6-Modell aus der Anwendung.

Es ist in Ordnung, das EF6 nuget-Paket (EntityFramework) installiert zu lassen, da EF Core und EF6 parallel in der gleichen Anwendung verwendet werden können. Wenn Sie jedoch nicht beabsichtigen, EF6 in einem Bereich Ihrer Anwendung zu verwenden, kann das Deinstallieren des Pakets dabei helfen, Kompilier Fehler für Code Elemente zu erhalten, die eine Aufmerksamkeit erfordern.

## <a name="update-your-code"></a>Aktualisieren Ihres Codes

An dieser Stelle ist es wichtig, Kompilierungsfehler zu beheben und Code zu überprüfen, um festzustellen, ob sich die Verhaltensänderungen zwischen EF6 und EF Core auf Sie auswirken.

## <a name="test-the-port"></a>Testen des Ports

Nur weil Ihre Anwendung kompiliert, bedeutet nicht, dass Sie erfolgreich in EF Core portiert wurde. Sie müssen alle Bereiche Ihrer Anwendung testen, um sicherzustellen, dass keine der Verhaltensänderungen Ihre Anwendung beeinträchtigt hat.
