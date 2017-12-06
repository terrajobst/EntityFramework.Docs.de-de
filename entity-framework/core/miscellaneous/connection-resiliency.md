---
title: Verbindungsresilienz - EF Core
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="connection-resiliency"></a>Verbindungsstabilität

Verbindungsresilienz führt automatisch Wiederholungsversuche für fehlerhafte Datenbankbefehle. Die Funktion kann mit einer beliebigen Datenbank verwendet werden, durch Angabe einer "Ausführungsstrategie", die die Logik zum Erkennen von Fehlern, und wiederholen die Befehle kapselt. EF-Core-Anbieter können auf ihre Datenbank fehlerbedingungen und optimale wiederholungsrichtlinien zugeschnittene Ausführungsstrategien angeben.

Beispielsweise enthält die SQL Server-Anbieter eine Ausführungsstrategie, die speziell auf SQL Server (einschließlich SQL Azure) zugeschnitten ist. Er erkennt die Ausnahmetypen, die wiederholt werden können und sensibler Standardwerte für die maximal Anzahl von Wiederholungen, Verzögerung zwischen Wiederholungen usw. enthält.

Eine Ausführungsstrategie wird angegeben, wenn Sie die Optionen für den Kontext zu konfigurieren. Dies ist in der Regel in der `OnConfiguring` Methode von den abgeleiteten Kontext oder `Startup.cs` für eine ASP.NET Core-Anwendung.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Benutzerdefinierte Ausführungsstrategie

Es ist ein Mechanismus zum Registrieren einer benutzerdefinierten Ausführungsstrategie Ihrer Wahl, wenn Sie die Standardwerte ändern möchten.

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

Eine Ausführungsstrategie, die automatisch auf Fehler versucht werden soll, muss in der Lage, jeden Vorgang in einen wiederholungsversuchblock wiederzugeben, die nicht. Wenn Wiederholungen aktiviert sind, wird jeder Vorgang, die Sie über EF Core ausführen eigener wiederholbar Vorgang, d. h. jede Abfrage und jeder Aufruf von `SaveChanges()` wird als eine Einheit wiederholt, wenn ein vorübergehender Fehler auftritt.

Jedoch wenn Codes initiiert eine Transaktion mit `BeginTransaction()` definieren Sie eine eigene Gruppe von Vorgängen, die als Einheit behandelt werden müssen, d. h. müssen alles innerhalb der Transaktion wiedergegeben werden soll ein Fehler auftreten. Sie erhalten eine der folgenden vergleichbare Ausnahme, wenn Sie versuchen, dieses Verfahren, wenn eine Ausführungsstrategie verwenden.

> InvalidOperationException: Die konfigurierte Ausführungsstrategie 'SqlServerRetryingExecutionStrategy' unterstützt keine vom Benutzer initiierten Transaktionen. Verwenden Sie die Ausführungsstrategie "DbContext.Database.CreateExecutionStrategy()" zurückgegebene, um alle Vorgänge in der Transaktion als Einheit mit möglichem auszuführen.

Die Lösung besteht darin, manuell aufrufen Ausführungsstrategie mit einen Delegaten, die alle Elemente darstellt, die ausgeführt werden muss. Um ein vorübergehender Fehler auftritt, wird die Ausführungsstrategie den Delegaten erneut aufrufen.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Fehler beim Commit der Transaktion und das Problem Idempotenz

Im Allgemeinen wird, wenn ein Verbindungsfehler vorhanden ist die aktuelle Transaktion ein Rollback ausgeführt. Jedoch wenn die Verbindung getrennt wird, während die Transaktion wird wird ein Commit der resultierende Status der Transaktion ist unbekannt. Lesen Sie diese [Blogbeitrag](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) Weitere Details.

Wird standardmäßig die Ausführungsstrategie wiederholt den Vorgang als ob Rollback der Transaktion, ist dies nicht der Fall Dies führt jedoch eine Ausnahme ausgelöst, wenn der neuen Datenbankstatus nicht kompatibel ist oder zur führen **datenbeschädigung** Wenn die Vorgang beruht nicht auf einem bestimmten Status, z. B. Wenn Sie eine neue Zeile mit automatisch generierten Schlüsselwerte einfügen.

Es gibt mehrere Möglichkeiten, zu diesem Zweck verfügt.

### <a name="option-1---do-almost-nothing"></a>Option 1 – nothing (fast)

Die Wahrscheinlichkeit für einen Verbindungsfehler während des Transaktionscommits ist gering, sodass sie möglicherweise für Ihre Anwendung nur fehlschlägt, wenn tatsächlich diese Bedingung tritt ein akzeptabel.

Allerdings müssen Sie zur Vermeidung des Speicher generierte Schlüssel, um sicherzustellen, dass eine Ausnahme ausgelöst wird, anstatt eine doppelte Zeile hinzuzufügen. Erwägen Sie eine Client-generierte GUID-Wert oder einen für die clientseitige-wertgenerator.

### <a name="option-2---rebuild-application-state"></a>Option 2: Rebuild-Anwendungsstatus

1. Verwerfen den aktuellen `DbContext`.
2. Erstellen Sie ein neues `DbContext` und Wiederherstellen des Zustands der Anwendung aus der Datenbank.
3. Dem Benutzer zu informieren, dass der letzte Vorgang nicht erfolgreich abgeschlossen wurden wurden möglicherweise.

### <a name="option-3---add-state-verification"></a>Option 3 - Status-Überprüfung hinzufügen

Für die meisten Vorgänge, die den Status einer Datenbank ändern, ist es möglich, Code hinzufügen, der überprüft, ob es erfolgreich ausgeführt wurde. EF bietet eine Erweiterungsmethode zur - Vereinfachung `IExecutionStrategy.ExecuteInTransaction`.

Diese Methode startet einen Commit für eine Transaktion und akzeptiert auch eine Funktion in der `verifySucceeded` Parameter, der aufgerufen wird, wenn ein vorübergehender Fehler beim Commit Transaktion auftritt.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Hier `SaveChanges` mit aufgerufen wird `acceptAllChangesOnSuccess` festgelegt `false` zu vermeiden, Ändern des Status der `Blog` Entität `Unchanged` Wenn `SaveChanges` erfolgreich ausgeführt wird. Dies ermöglicht um den gleichen Vorgang zu wiederholen, wenn das Commit schlägt fehl, und die Transaktion ein Rollback.

### <a name="option-4---manually-track-the-transaction"></a>Option 4: die Transaktion manuell nachverfolgen

Wenn Sie müssen Speicher generierte Schlüssel verwenden, oder benötigen eine generische Methode der Behandlung von commitfehlern, die nicht auf den ausgeführten Vorgang abhängig konnte jede Transaktion eine ID zugewiesen, die überprüft wird, wenn dies fehlschlägt.

1. Fügen Sie eine Tabelle mit der Datenbank verwendet, um den Status der Transaktionen nachzuverfolgen.
2. In der Tabelle am Anfang jeder Transaktion eine Zeile eingefügt werden.
3. Wenn die Verbindung während der Commit-fehlschlägt, überprüfen Sie das Vorhandensein der entsprechenden Zeile in der Datenbank.
4. Wenn das Commit erfolgreich ausgeführt wurde, löschen Sie die entsprechende Zeile aus, um die Vergrößerung der Tabelle zu vermeiden.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Stellen Sie sicher, dass der Kontext für die Überprüfung verwendet eine Ausführungsstrategie definiert, wie die Verbindung wahrscheinlich erneut bei der Überprüfung fehl ist, wenn während des Transaktionscommits aufgetreten ist.
