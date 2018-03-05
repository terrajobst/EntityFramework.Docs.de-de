---
title: "Parallelitätskonflikte - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a>Behandlung von Parallelitätskonflikten

> [!NOTE]
> Auf dieser Seite werden die Funktionsweise von Parallelität in EF Core und Parallelitätskonflikte in Ihrer Anwendung zu behandeln. Finden Sie unter [Parallelitätstoken](xref:core/modeling/concurrency) Weitere Informationen zum Konfigurieren von parallelitätstoken in Ihrem Modell.

> [!TIP]
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) finden Sie auf GitHub.

_Datenbankparallelität_ bezieht sich auf Situationen, in dem mehrere Prozesse oder Benutzer Zugriff auf ein, oder ändern die gleichen Daten in einer Datenbank zur gleichen Zeit. _Parallelitätssteuerung_ bezieht sich auf bestimmte Mechanismen verwendet, um die Datenkonsistenz in Anwesenheit von gleichzeitigen Änderungen sicherzustellen.

EF Core implementiert _Steuerung durch vollständige Parallelität_, d. h., es mehrere Prozesse können oder Änderungen für Benutzer unabhängig ohne den Aufwand für die Synchronisierung oder sperren. Im Idealfall werden diese Änderungen wirkt sich nicht miteinander und aus diesem Grund werden in der Lage erfolgreich ausgeführt werden kann. Im schlimmsten Fall kann zwei oder mehr Prozesse versucht, die in Konflikt stehenden Änderungen vornehmen, und nur eine Kopie sollte erfolgreich sein.

## <a name="how-concurrency-control-works-in-ef-core"></a>Funktionsweise der parallelitätssteuerung in EF Core

Eigenschaften, die als parallelitätstoken verwendet werden, um die Steuerung durch vollständige Parallelität implementieren konfiguriert: immer ein Update- oder Delete-Vorgang ausgeführt wird, während der `SaveChanges`, der Wert des parallelitätstokens für die Datenbank mit der ursprünglichen verglichen wird der Wert von EF Core gelesen werden.

- Wenn die Werte übereinstimmen, kann der Vorgang abgeschlossen.
- Wenn die Werte nicht übereinstimmen, wird EF Core davon ausgegangen, dass ein anderer Benutzer einen Konflikt beteiligte Vorgang ausgeführt hat und bricht die aktuelle Transaktion ab.

Die Situation, wenn ein anderer Benutzer einen Vorgang, der in Konflikt steht, mit der aktuelle Vorgang ausgeführt hat, wird als bezeichnet _Parallelitätskonflikt_.

Datenbankanbieter sind verantwortlich für den Vergleich von Tokenwerten Parallelität implementieren.

Für relationale Datenbanken EF Core umfasst eine Überprüfung der für den Wert des parallelitätstokens in der `WHERE` Klausel beliebiger `UPDATE` oder `DELETE` Anweisungen. Nach dem Ausführen der Anweisungen, liest EF Core die Anzahl der Zeilen, die betroffen sind.

Wenn keine Zeilen betroffen sind, ein Parallelitätskonflikt erkannt wird, und EF Core löst `DbUpdateConcurrencyException`.

Angenommen, wir möchten konfigurieren `LastName` auf `Person` ein parallelitätstoken ist. Alle Update-Vorgang auf Person die Überprüfung auf Parallelität in einschließen, wird die `WHERE` Klausel:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Auflösen von Parallelitätskonflikten

Im vorherigen Beispiel fortsetzen, wenn ein Benutzer versucht, speichern einige Änderungen an einer `Person`, jedoch bereits einem anderen Benutzer geändert wurde die `LastName` der wird eine Ausnahme ausgelöst werden.

Die Anwendung konnte zu diesem Zeitpunkt einfach informiert den Benutzer darüber, dass das Update nicht aufgrund von in Konflikt stehenden Änderungen erfolgreich war und verschieben. Aber es kann wünschenswert sein, fordert den Benutzer aus, um sicherzustellen, dass dieser Datensatz weiterhin dieselbe tatsächlichen Person darstellt, und wiederholen den Vorgang.

Dieser Vorgang ist ein Beispiel für _Auflösen von einem Parallelitätskonflikt_.

Lösen eines Parallelitätskonflikts umfasst das Zusammenführen von die ausstehenden Änderungen aus dem aktuellen `DbContext` mit den Werten in der Datenbank. Welche Werte zusammengeführt abrufen variiert, je nachdem, auf die Anwendung, und durch die Benutzereingabe gerichtet sein.

**Es gibt drei Gruppen von Werten verfügbar, in denen einen Parallelitätskonflikt gelöst wird:**

* **Aktuelle Werte** sind die Werte, die die Anwendung versucht hat, in die Datenbank geschrieben.

* **Ursprüngliche Werte** sind die Werte, die ursprünglich aus der Datenbank abgerufen wurden, bevor Änderungen vorgenommen wurden.

* **Datenbank-Werte** sind die Werte, die derzeit in der Datenbank gespeichert.

Die allgemeine Vorgehensweise zum Behandeln einer Parallelitätskonflikte ist:

1. Abfangen `DbUpdateConcurrencyException` während `SaveChanges`.
2. Verwendung `DbUpdateConcurrencyException.Entries` So bereiten Sie einen neuen Satz von Änderungen für die betroffenen Entitäten vor.
3. Aktualisieren Sie die ursprünglichen Werte der im parallelitätstoken, die aktuellen Werte in der Datenbank widerspiegeln.
4. Wiederholen Sie den Prozess aus, bis keine Konflikte auftreten.

Im folgenden Beispiel `Person.FirstName` und `Person.LastName` Setup als parallelitätstoken sind. Es ist ein `// TODO:` Kommentar in den Speicherort, bei denen Sie enthalten anwendungsspezifische Logik zum Auswählen des Wert gespeichert werden soll.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
