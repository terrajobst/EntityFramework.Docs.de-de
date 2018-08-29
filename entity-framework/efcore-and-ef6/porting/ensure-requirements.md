---
title: 'Portieren von EF6 auf EF Core: Überprüfen von Anforderungen'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: fd378c72a3c8de4a441861b3c52b94eba5f7e5b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993439"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Vor dem Portieren von EF 6 nach EF Core: Überprüfen Sie die Anforderungen Ihrer Anwendung

Vor der Installation des portiervorgang ist es wichtig, um sicherzustellen, dass EF Core die Anforderungen für den Datenzugriff für Ihre Anwendung erfüllt.

## <a name="missing-features"></a>Fehlende features

Stellen Sie sicher, dass EF Core verfügt über alle Funktionen, die Sie in Ihrer Anwendung verwenden müssen. Finden Sie unter [Funktionsvergleich](../features.md) für einen detaillierten Vergleich wie der Features in EF Core im Vergleich zu EF6. Wenn alle erforderlichen Features fehlen, stellen Sie sicher, dass Sie den Mangel an diese Funktionen vor dem Portieren nach EF Core beheben können.

## <a name="behavior-changes"></a>Verhaltensänderungen

Dies ist eine nicht erschöpfende Liste einiger Änderungen im Verhalten zwischen EF6 und EF Core. Es ist wichtig zu beachten Sie dies als den Port der Anwendung, wie sie die Art ändern können, die Ihre Anwendung verhält sich, die jedoch nicht angezeigt wie die Kompilierungsfehler nach dem Austausch zu EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet.Add/Attach und Graph-Verhalten

In EF6 Aufrufen `DbSet.Add()` auf eine Entität in einer rekursiven Suche für alle Entitäten, die in die Navigationseigenschaften verwiesen wird. Alle Entitäten, die gefunden werden und nicht bereits vom Kontext, nachverfolgt werden, sind auch markiert werden, wenn hinzugefügt. `DbSet.Attach()` verhält sich identisch, außer der alle Entitäten markiert sind als nicht geändert.

**EF Core führt eine rekursive Suche nach ähnlichen, jedoch mit einigen etwas anderen Regeln.**

*  Die Stammentität ist immer im angeforderten Zustand (für hinzugefügt `DbSet.Add` und unverändert für `DbSet.Attach`).

*  **Für Entitäten, die während der rekursiven Suche von Navigationseigenschaften gefunden werden:**

    *  **Wenn der Primärschlüssel der Entität speichern generierter ist**

        * Wenn der Primärschlüssel nicht auf einen Wert festgelegt ist, wird der Status zu hinzugefügten festgelegt. Wert des Primärschlüssels gilt als "nicht festgelegt", wenn es der CLR-Standardwert für den Eigenschaftentyp zugewiesen ist (z. B. `0` für `int`, `null` für `string`usw..).

        * Wenn der primäre Schlüssel auf einen Wert festgelegt ist, wird der Status auf unchanged festgelegt.

    *  Wenn der Primärschlüssel nicht generierten Datenbank ist, wird die Entität im den gleichen Zustand wie der Stamm gespeichert.

### <a name="code-first-database-initialization"></a>Code des ersten Datenbankinitialisierung

**EF6 weist eine erhebliche Menge an Magic, die sie für das Auswählen von Verbindung mit der Datenbank, und Initialisieren der Datenbank ausführt. Zu diesen Regeln gehören:**

* Wenn keine Konfiguration ausgeführt wird, wählt EF6 eine Datenbank auf SQL Express oder LocalDb.

* Wenn eine Verbindungszeichenfolge mit dem gleichen Namen wie der Kontext in den Anwendungen ist `App/Web.config` -Datei diese Verbindung verwendet werden.

* Wenn die Datenbank nicht vorhanden ist, wird es erstellt.

* Wenn keine der Tabellen aus dem Modell in der Datenbank vorhanden sind, wird das Schema für das aktuelle Modell der Datenbank hinzugefügt. Wenn Migrations aktiviert sind, werden sie verwendet, um die Datenbank zu erstellen.

* Wenn die Datenbank vorhanden ist, und das Schema von EF6 hatten zuvor erstellt haben, ist Klicken Sie dann das Schema für die Kompatibilität mit dem aktuellen Modell aktiviert. Eine Ausnahme wird ausgelöst, wenn das Modell geändert wurde, da das Schema erstellt wurde.

**EF Core führt nicht zu, dass keines dieser Magic-Befehl.**

* Die datenbankverbindung muss explizit im Code konfiguriert werden.

* Es wird keine Initialisierung ausgeführt. Verwenden Sie `DbContext.Database.Migrate()` zum Anwenden von Migrationen (oder `DbContext.Database.EnsureCreated()` und `EnsureDeleted()` zu erstellen und die Datenbank löschen ohne Migrationen zu verwenden).

### <a name="code-first-table-naming-convention"></a>Erste Tabelle Benennungskonvention

EF6 wird der Name der Entitätsklasse über eine pluralisierungsdienst den Standardnamen für die Tabelle zu berechnen, dem die Entität zugeordnet ist.

EF Core verwendet den Namen der `DbSet` -Eigenschaft, die die Entität für den abgeleiteten Kontext verfügbar gemacht wird. Wenn die Entität nicht verfügt eine `DbSet` -Eigenschaft, und klicken Sie dann auf den Namen der Klasse wird verwendet.
