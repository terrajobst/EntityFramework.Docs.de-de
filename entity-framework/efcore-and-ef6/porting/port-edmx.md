---
title: Portieren von EF6 auf EF Core – Portieren eines EDMX-basierten Modells
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997410"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portieren eines EDMX-Datei-basierten Modells mit EF6 zu EF Core

EF Core unterstützt nicht das EDMX-Dateiformat für Modelle. So portieren Sie diese Modelle, die beste Option ist ein neues, codebasierte Modell aus der Datenbank für Ihre Anwendung zu generieren.

## <a name="install-ef-core-nuget-packages"></a>Installieren von EF Core NuGet-Paketen

Installieren Sie die `Microsoft.EntityFrameworkCore.Tools` NuGet-Paket.

## <a name="regenerate-the-model"></a>Das Modell erneut generieren

Sie können jetzt die reverse Engineering-Funktionen verwenden, zum Erstellen eines Modells, basierend auf Ihrer vorhandenen Datenbank.

Führen den folgenden Befehl in der Paket-Manager-Konsole (Tools Memberauswahloperator NuGet-Paket-Manager-Paket-Manager-Konsole Memberauswahloperator). Finden Sie unter [-Paket-Manager-Konsole (Visual Studio)](../../core/miscellaneous/cli/powershell.md) für Befehlsoptionen, um eine Teilmenge der Tabellen usw. zu erstellen.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Hier ist z. B. der Befehl zum Erstellen des Gerüsts für ein Modell aus der Blogging-Datenbank auf der SQL Server-LocalDB-Instanz.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Entfernen von EF6-Modell

Nun möchten, das EF6-Modell von der Anwendung entfernen.

Es ist in Ordnung, das EF6-NuGet-Paket ("EntityFramework"), die installiert ist, zu verlassen, da EF Core und EF6 verwendete Seite-an-Seite in der gleichen Anwendung sein kann. Jedoch wenn Sie nicht sind EF6 in alle Bereiche der Anwendung verwenden, können klicken Sie dann das Paket deinstallieren Kompilierungsfehler auf Teile des Codes, die Ihre Aufmerksamkeit erfordern.

## <a name="update-your-code"></a>Aktualisieren Sie Ihren code

An diesem Punkt ist es eine Frage der Adressierung Kompilierungsfehler und Überprüfung von Code zu überprüfen, ob die verhaltensänderungen zwischen EF6 und EF Core Auswirkungen informiert.

## <a name="test-the-port"></a>Testen Sie den port

Nur weil die Anwendung kompiliert wird, bedeutet nicht, dass es erfolgreich in EF Core portiert werden. Sie benötigen, um alle Bereiche der Anwendung, um sicherzustellen, dass keine der Änderungen des Verhaltens Ihrer Anwendung beeinträchtigt haben zu testen.

> [!TIP]
> Finden Sie unter [erste Schritte mit EF Core in ASP.NET Core mit einer vorhandenen Datenbank](xref:core/get-started/aspnetcore/existing-db) für einen zusätzlichen Verweis auf das Arbeiten mit einer vorhandenen Datenbank 
