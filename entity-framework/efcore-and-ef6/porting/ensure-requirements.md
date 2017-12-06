---
title: "Portieren von EF6 auf EF-Core - Anforderungen zu überprüfen"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Vor dem Portieren von EF6 auf EF Core: Überprüfen Sie den Anforderungen der Anwendung

Bevor Sie mit der Portierung von beginnen ist es wichtig zu überprüfen, ob EF Core Anforderungen der Daten für Ihre Anwendung entspricht.

## <a name="missing-features"></a>Fehlende Funktionen

Stellen Sie sicher, dass EF Core alle Funktionen verfügt, die Sie in Ihrer Anwendung verwenden müssen. Finden Sie unter [Funktionsvergleich](../features.md) für einen ausführlichen Vergleich wie die Funktionen von EF-Core mit EF6 verglichen. Wenn alle erforderlichen Funktionen fehlen, stellen Sie sicher, dass für das Fehlen dieser Funktionen vor dem Portieren auf EF Core kompensiert werden können.

## <a name="behavior-changes"></a>Verändertes Programmverhalten

Dies ist eine nicht erschöpfende Liste einiger Änderungen im Verhalten zwischen EF6 und EF Core. Es ist wichtig zu beachten Sie den Port die Anwendung halten sich ändernder wie können die Anwendung verhält sich aber wird nicht angezeigt werden, als Kompilierungsfehler nach dem Austausch und EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet.Add/Attach und Graph-Verhalten

In EF6 Aufrufen von `DbSet.Add()` für eine Entität führt in eine rekursive Suche für alle Entitäten, die in die Navigationseigenschaften verwiesen wird. Alle benutzerentitäten, die gefunden werden, und nicht bereits vom Kontext, nachverfolgt werden auch als hinzugefügt markiert werden. `DbSet.Attach()`verhält sich, mit der Ausnahme alle Entitäten gekennzeichnet sind als unverändert.

**EF Core führt eine rekursive Suche in ähnlichen, jedoch mit einigen etwas anderen Regeln.**

*  Die Stammentität befindet sich immer im angeforderten Status (für hinzugefügt `DbSet.Add` und unverändert `DbSet.Attach`).

*  **Für Entitäten, die während der rekursiven Suche von Navigationseigenschaften gefunden werden:**

    *  **Wenn der Primärschlüssel der Entität speichern generierter ist**

        * Wenn der Primärschlüssel nicht auf einen Wert festgelegt ist, wird der Status zu hinzugefügte festgelegt. Der Primärschlüsselwert gilt als "nicht konfiguriert", wenn es der CLR-Standardwert für den Eigenschaftentyp zugewiesen ist (d. h. `0` für `int`, `null` für `string`usw..).

        * Wenn der Primärschlüssel auf einen Wert festgelegt ist, wird der Status auf unverändert festgelegt.

    *  Wenn der Primärschlüssel nicht datenbankgeneriert ist, wird die Entität im gleichen Zustand wie der Stamm gespeichert.

### <a name="code-first-database-initialization"></a>Initialisierung der ersten Datenbank Code

**EF6 verfügt über eine beträchtliche Menge an Magic, die ausgeführt wird, um die Verbindung mit der Datenbank auswählen und zum Initialisieren der Datenbank. Diese Regeln gehören:**

* Wenn keine Konfiguration ausgeführt wird, wählt EF6 eine Datenbank auf SQL Express oder LocalDb.

* Wenn eine Verbindungszeichenfolge mit dem gleichen Namen wie der Kontext in den Clientanwendungen ist `App/Web.config` -Datei wird diese Verbindung verwendet werden.

* Wenn die Datenbank nicht vorhanden ist, wird er erstellt.

* Wenn keine der Tabellen aus dem Modell in der Datenbank vorhanden sind, wird das Schema für das aktuelle Modell mit der Datenbank hinzugefügt. Wenn es sich bei Migrationen aktiviert sind, werden sie verwendet, zum Erstellen der Datenbank.

* Wenn die Datenbank vorhanden ist und EF6 hatte zuvor das Schema erstellt, wird das Schema für die Kompatibilität mit dem aktuellen Modell überprüft. Eine Ausnahme wird ausgelöst, wenn das Modell geändert hat, seit der Erstellung des Schemas.

**EF Core führt keine dieser Magic.**

* Verbindung mit der Datenbank muss explizit in Code konfiguriert werden.

* Keine Initialisierung ausgeführt. Verwenden Sie `DbContext.Database.Migrate()` anzuwendende Migrationen (oder `DbContext.Database.EnsureCreated()` und `EnsureDeleted()` zu erstellen und die Datenbank löschen ohne Verwendung von Migrationen).

### <a name="code-first-table-naming-convention"></a>Erste Tabelle Namenskonvention

EF6, wird der Klassenname für die Entität über ein Pluralisierung-Dienst, um den Standardnamen für die Tabelle zu berechnen, dem die Entität zugeordnet ist.

EF Core verwendet den Namen des der `DbSet` -Eigenschaft, die die Entität für den abgeleiteten Kontext verfügbar gemacht werden. Wenn die Entität keine `DbSet` -Eigenschaft, und klicken Sie dann auf den Klassennamen verwendet wird.
