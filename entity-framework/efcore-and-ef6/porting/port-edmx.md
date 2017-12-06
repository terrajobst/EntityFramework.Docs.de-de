---
title: Portieren von EF6 auf EF-Core - Portieren eine EDMX-basiertes Modell
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 1880d5ce49d4c9cb891d0e8fb230770bb44799e5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2017
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portieren einer EF6 EDMX-basiertes Modell EF-Kern

EF Core unterstützt nicht das Format der EDMX-Datei für Modelle. Die beste Option So portieren Sie diese Modelle werden aus der Datenbank für Ihre Anwendung ein neues Code basierende Modell generieren.

## <a name="install-ef-core-nuget-packages"></a>Installieren von EF Core NuGet-Pakete

Installieren der `Microsoft.EntityFrameworkCore.Tools` NuGet-Paket.

## <a name="regenerate-the-model"></a>Das Modell erneut generieren

Sie können jetzt die reverse Engineering-Funktionen verwenden, zum Erstellen eines Modells, basierend auf der vorhandenen Datenbank.

Führen den folgenden Befehl in der Paket-Manager-Konsole (Tools Klassenschlüssel NuGet Package Manager Klassenschlüssel Paket-Manager-Konsole). Finden Sie unter [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) für Befehlsoptionen, um das Gerüst für eine Teilmenge der Tabellen usw. erstellen.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Hier ist z. B. den Befehl aus, um ein Modell aus der Blogging-Datenbank auf der SQL Server-LocalDB-Instanz zu erstellen.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Entfernen von EF6-Modell

Das Modell EF6 würden jetzt von der Anwendung entfernt werden.

Es ist in Ordnung, um das EF6 NuGet-Paket (EntityFramework) installiert ist, zu lassen, als EF Core und EF6 verwendete Seite-an-Seite in derselben Anwendung sein können. Jedoch wenn Sie nicht sind EF6 in alle Bereiche der Anwendung zu verwenden, können klicken Sie dann das Paket deinstallieren Kompilierungsfehler auf Codeabschnitte, die Aufmerksamkeit erfordern.

## <a name="update-your-code"></a>Aktualisieren Sie den code

An diesem Punkt ist es nur wenige der Adressierung Kompilierungsfehler und Überprüfung von Code zu überprüfen, ob die verhaltensänderungen zwischen EF6 und EF Core auswirkt.

## <a name="test-the-port"></a>Testen Sie den port

Einfach, da die Anwendung kompiliert wird, bedeutet nicht, dass es erfolgreich zu EF Core portiert werden. Sie müssen zum Testen der Anwendung, um sicherzustellen, dass keines der verhaltensänderungen Ihrer Anwendung negativ beeinträchtigt, haben alle Bereiche.

> [!TIP]
> Finden Sie unter [erste Schritte mit EF Core unter ASP.NET Core mit einer vorhandenen Datenbank](xref:core/get-started/aspnetcore/existing-db) für ein zusätzlicher Verweis zum Arbeiten mit einer vorhandenen Datenbank 
