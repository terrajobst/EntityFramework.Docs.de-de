---
title: Verbindungsresilienz – EF Core
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: dae646e39b4dbd96b34f47582f9b2aa531cf88a7
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614336"
---
# <a name="connection-resiliency"></a>Verbindungsresilienz

Verbindungsresilienz versucht automatisch fehlerhaften Datenbankbefehle auszuführen. Die Funktion kann mit einer beliebigen Datenbank verwendet werden, um durch Angabe einer "Ausführungsstrategie", die erforderliche Logik zum Erkennen von Fehlern aus, und wiederholen die Befehle kapselt. EF Core-Anbieter können Ausführungsstrategien maßgeschneidert für ihre Datenbank fehlerbedingungen und optimale wiederholungsrichtlinien angeben.

Beispielsweise enthält den SQL Server-Anbieter eine Ausführungsstrategie, die speziell auf SQL Server (einschließlich SQL Azure) zugeschnitten ist. Es erkennt die Ausnahmetypen, die wiederholt werden können und verfügt über sinnvolle Standardwerte für maximale Wiederholungsversuche, Verzögerung zwischen Wiederholungen usw.

Eine Ausführungsstrategie für die wird angegeben, wenn Sie die Optionen für den Kontext zu konfigurieren. Dies ist in der Regel in der `OnConfiguring` Methode, die von Ihrem abgeleiteten Kontext oder `Startup.cs` für eine ASP.NET Core-Anwendung.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Benutzerdefinierte Ausführungsstrategie

Es gibt ein Mechanismus zum Registrieren einer benutzerdefinierten Ausführungsstrategie selbst, wenn Sie die Standardwerte ändern möchten.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Ausführungsstrategien und Transaktionen

Eine Ausführungsstrategie, die automatisch bei Fehlern versucht werden soll, muss in der Lage, jeden Vorgang in einen wiederholungsversuchblock wiederzugeben, die nicht. Wenn Wiederholungen aktiviert sind, wird jeder Vorgang, die Sie über EF Core ausführen individuell wiederholbaren Vorgang. Das heißt, jede Abfrage und jeder Aufruf von `SaveChanges()` wird als eine Einheit wiederholt, wenn ein vorübergehender Fehler auftritt.

Aber wenn Ihr Code initiiert eine Transaktion mit `BeginTransaction()` definieren Sie Ihre eigene Gruppe von Vorgängen, die als Einheit behandelt werden müssen, und alles innerhalb der Transaktion wiedergegeben werden soll ein Fehler auftreten muss. Sie erhalten eine Ausnahme wie folgt, wenn Sie versuchen, dies zu tun, wenn Sie eine Ausführungsstrategie verwenden:

> "InvalidOperationException": Die konfigurierte Ausführungsstrategie "SqlServerRetryingExecutionStrategy" unterstützt keine vom Benutzer initiierte Transaktionen. Verwenden Sie die Ausführungsstrategie, die von „DbContext.Database.CreateExecutionStrategy()“ zurückgegeben wird, um alle Vorgänge in der Transaktion als wiederholbare Einheit auszuführen.

Die Lösung besteht darin, manuell die Ausführungsstrategie mit einem Delegaten, der alle Komponenten darstellt aufrufen, die ausgeführt werden muss. Die Ausführungsstrategie ruft den Delegaten erneut auf, wenn ein vorübergehender Fehler auftritt.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Fehler beim Commit der Transaktion und das Problem Idempotenz

Im Allgemeinen wird bei ein Verbindungsfehler die aktuelle Transaktion ein Rollback ausgeführt. Allerdings, wenn die Verbindung getrennt wird, während die Transaktion wird ein Commit der resultierende Status der Transaktion ist unbekannt. Finden Sie in diesem [Blogbeitrag](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) Weitere Details.

In der Standardeinstellung die Ausführungsstrategie wiederholt den Vorgang wurde ein Rollback für die Transaktion, wobei jedoch ist dies nicht der Fall dies zu einer Ausnahme führt, wenn der neue Datenbankstatus nicht kompatibel ist oder zu führen **datenbeschädigung** Wenn die Vorgang beruht nicht auf einem bestimmten Status, z. B. wenn eine neue Zeile mit automatisch generierter Schlüsselwerte einfügen.

Es gibt mehrere Möglichkeiten, diese verarbeiten.

### <a name="option-1---do-almost-nothing"></a>Option 1: nothing (fast)

Die Wahrscheinlichkeit für einen Verbindungsfehler während des Transaktionscommits ist gering, sodass es für die Anwendung nur fehlschlägt, wenn diese Bedingung, tatsächlich eintritt gültig sein kann.

Allerdings müssen Sie zu vermeiden, vom Speicher generierte Schlüssel verwenden, um sicherzustellen, dass eine Ausnahme ausgelöst wird, anstatt eine Zeile mit doppelte hinzuzufügen. Sollten Sie einen Client generierter GUID-Wert oder einen für die clientseitige-wertgenerator.

### <a name="option-2---rebuild-application-state"></a>Option 2 – Rebuild-Anwendungsstatus

1. Verwerfen der aktuellen `DbContext`.
2. Erstellen Sie ein neues `DbContext` und Wiederherstellen des Zustands Ihrer Anwendung aus der Datenbank.
3. Informieren Sie den Benutzer, dass der letzte Vorgang nicht erfolgreich abgeschlossen wurde haben kann.

### <a name="option-3---add-state-verification"></a>Option 3 - Status-Überprüfung hinzufügen

Für den Großteil der Vorgänge, die den Zustand der Datenbank zu ändern, ist es möglich, Code hinzuzufügen, die überprüft, ob es erfolgreich war. EF bietet eine Erweiterungsmethode zur - Vereinfachung `IExecutionStrategy.ExecuteInTransaction`.

Diese Methode beginnt und führt einen Commit für eine Transaktion und akzeptiert auch eine Funktion in der `verifySucceeded` Parameter, der aufgerufen wird, wenn ein vorübergehender Fehler, während das Commit der Transaktion auftritt.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Hier `SaveChanges` wird aufgerufen, mit `acceptAllChangesOnSuccess` festgelegt `false` um zu vermeiden, Ändern des Status der `Blog` Entität `Unchanged` Wenn `SaveChanges` erfolgreich ausgeführt wird. Dadurch können um den gleichen Vorgang zu wiederholen, wenn der Commit schlägt fehl, und die Transaktion ein Rollback.

### <a name="option-4---manually-track-the-transaction"></a>Option 4: die Transaktion manuell nachverfolgen

Wenn Sie müssen Speicher generierte Schlüssel oder eine generische Möglichkeit Commit Fehler zu beheben, die nicht von den ausgeführten Vorgang abhängig ist konnte jeder Transaktion eine ID zugewiesen werden, die überprüft wird, wenn der Commit schlägt fehl.

1. Fügen Sie eine Tabelle mit der Datenbank verwendet, um den Status der Transaktionen nachzuverfolgen.
2. Eine Zeile in der Tabelle am Anfang jeder Transaktion eingefügt.
3. Wenn die Verbindung während des Commits fehlschlägt, überprüfen Sie das Vorhandensein der entsprechenden Zeile in der Datenbank.
4. Wenn der Commit erfolgreich ausgeführt wird, löschen Sie die entsprechende Zeile aus, um die Vergrößerung der Tabelle zu vermeiden.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Stellen Sie sicher, dass der Kontext für die Überprüfung verwendet, eine Ausführungsstrategie definiert wurde, wie die Verbindung vermutlich erneut bei der Überprüfung fehlschlägt ist, wenn während des Transaktionscommits aufgetreten ist.
