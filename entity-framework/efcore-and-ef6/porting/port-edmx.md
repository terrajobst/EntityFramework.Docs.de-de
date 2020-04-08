---
title: 'Portieren von EF6 auf EF Core: Portieren eines EDMX-basierten Modells'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413528"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portieren eines EDMX-basierten EF6-Modells auf EF Core

EF Core unterstützt das EDMX-Dateiformat nicht für Modelle. Der beste Ansatz zum Portieren dieser Modelle besteht darin, ein neues codebasiertes Modell aus der Datenbank für Ihre Anwendung zu generieren.

## <a name="install-ef-core-nuget-packages"></a>Installieren von EF Core-NuGet-Paketen

Installieren Sie das NuGet-Paket `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Erneutes Generieren des Modells

Sie können jetzt die Reverse-Engineering-Funktion verwenden, um ein Modell auf Grundlage der vorhandenen Datenbank zu erstellen.

Führen Sie in der Paket-Manager-Konsole (Extras > NuGet-Paket-Manager > Paket-Manager-Konsole) den folgenden Befehl aus. Im Artikel zur [Paket-Manager-Konsole in Visual Studio](../../core/miscellaneous/cli/powershell.md) finden Sie Befehlsoptionen für das Erstellen eines Gerüsts für eine Teilmenge von Tabellen oder anderen Komponenten.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Hier sehen Sie z. B. den Befehl, um das Gerüst eines Modell aus der Blogging-Datenbank auf Ihrer SQL Server-LocalDB-Instanz zu erstellen.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Entfernen des EF6-Modells

An dieser Stelle entfernen Sie das EF6-Modell aus der Anwendung.

Sie müssen das EF6-NuGet-Paket (EntityFramework) nicht deinstallieren, da EF Core und EF6 parallel in derselben Anwendung verwendet werden können. Wenn Sie jedoch nicht beabsichtigen, EF6 in Ihrer Anwendung zu verwenden, kann das Deinstallieren des Pakets dazu beitragen, dass nur zu den Codebestandteilen Kompilierfehler ausgegeben werden, die wirklich Ihre Aufmerksamkeit erfordern.

## <a name="update-your-code"></a>Aktualisieren Ihres Codes

An dieser Stelle ist es wichtig, Kompilierfehler zu beheben und ein Code Review durchzuführen, um festzustellen, ob die Verhaltensänderungen zwischen EF6 und EF Core eine Auswirkung zeigen.

## <a name="test-the-port"></a>Testen des Ports

Nur weil Ihre Anwendung erfolgreich kompiliert wird, bedeutet das nicht, dass sie auch erfolgreich auf EF Core portiert wurde. Sie müssen alle Bereiche Ihrer Anwendung testen, um sicherzustellen, dass keine der Verhaltensänderungen Ihre Anwendung beeinträchtigt hat.
