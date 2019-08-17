---
title: Portieren von EF6 auf EF Core-Validate-Anforderungen
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565346"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Vor dem Portieren von EF6 auf EF Core: Überprüfen der Anforderungen Ihrer Anwendung

Bevor Sie den Portierungs Prozess starten, müssen Sie überprüfen, ob EF Core den Datenzugriffs Anforderungen für Ihre Anwendung entspricht.

## <a name="missing-features"></a>Fehlende Features

Stellen Sie sicher, dass EF Core über alle Funktionen verfügt, die Sie in Ihrer Anwendung verwenden müssen. Unter [Funktions Vergleich](../features.md) finden Sie einen ausführlichen Vergleich, wie die in EF Core festgelegte Funktion mit EF6 verglichen wird. Wenn erforderliche Features fehlen, stellen Sie sicher, dass das Fehlen dieser Features vor dem Portieren auf EF Core kompensiert werden kann.

## <a name="behavior-changes"></a>Verhaltensänderungen

Dies ist eine nicht vollständige Liste einiger Verhaltensänderungen zwischen EF6 und EF Core. Beachten Sie, dass Sie diese Vorgehensweise als Port für Ihre Anwendung behalten müssen, da Sie sich möglicherweise auf die Art und Weise verhält, in der sich Ihre Anwendung verhält, aber nicht als Kompilierungsfehler angezeigt wird, nachdem der Austausch auf EF Core

### <a name="dbsetaddattach-and-graph-behavior"></a>Dbset. Add/Attach und Graph-Verhalten

In EF6 ergibt das `DbSet.Add()` Aufrufen von für eine Entität eine rekursive Suche nach allen Entitäten, auf die in ihren Navigations Eigenschaften verwiesen wird. Alle Entitäten, die gefunden wurden und nicht bereits vom Kontext nachverfolgt wurden, werden ebenfalls als hinzugefügt gekennzeichnet. `DbSet.Attach()`verhält sich identisch, mit dem Unterschied, dass alle Entitäten als unverändert markiert sind.

**EF Core führt eine ähnliche rekursive Suche durch, aber mit etwas anderen Regeln.**

*  Die Stamm Entität befindet sich immer im angeforderten Zustand (für `DbSet.Add` hinzugefügt und `DbSet.Attach`unverändert).

*  **Für Entitäten, die während der rekursiven Suche von Navigations Eigenschaften gefunden werden:**

    *  **Wenn der Primärschlüssel der Entität Speicher generiert ist**

        * Wenn der Primärschlüssel nicht auf einen Wert festgelegt ist, wird der Zustand auf Added festgelegt. Der Primärschlüssel Wert wird als "nicht festgelegt" betrachtet, wenn ihm der CLR-Standardwert für den Eigenschaftentyp `0` zugewiesen wird `null` ( `string`z. b. für `int`, für usw.).

        * Wenn der Primärschlüssel auf einen Wert festgelegt ist, wird der Status unverändert festgelegt.

    *  Wenn der Primärschlüssel nicht von der Datenbank generiert wird, wird die Entität in denselben Zustand wie der Stamm eingefügt.

### <a name="code-first-database-initialization"></a>Code First Daten Bank Initialisierung

**EF6 hat eine beträchtliche Menge an Magie, die Sie beim Auswählen der Datenbankverbindung und beim Initialisieren der Datenbank durchführt. Zu diesen Regeln gehören:**

* Wenn keine Konfiguration durchgeführt wird, wählt EF6 eine Datenbank in SQL Express oder localdb aus.

* Wenn sich eine Verbindungs Zeichenfolge mit dem gleichen Namen wie der Kontext in `App/Web.config` der Anwendungsdatei befindet, wird diese Verbindung verwendet.

* Wenn die Datenbank nicht vorhanden ist, wird Sie erstellt.

* Wenn keine der Tabellen aus dem Modell in der Datenbank vorhanden ist, wird das Schema für das aktuelle Modell der Datenbank hinzugefügt. Wenn Migrationen aktiviert sind, werden Sie verwendet, um die Datenbank zu erstellen.

* Wenn die Datenbank vorhanden ist und das Schema bereits von EF6 erstellt wurde, wird das Schema auf Kompatibilität mit dem aktuellen Modell geprüft. Eine Ausnahme wird ausgelöst, wenn sich das Modell seit der Erstellung des Schemas geändert hat.

**EF Core führt diese Magie nicht aus.**

* Die Datenbankverbindung muss explizit im Code konfiguriert werden.

* Es wird keine Initialisierung ausgeführt. Sie müssen verwenden `DbContext.Database.Migrate()` , um Migrationen anzuwenden `DbContext.Database.EnsureCreated()` ( `EnsureDeleted()` oder und um die Datenbank ohne Verwendung von Migrationen zu erstellen bzw. zu löschen).

### <a name="code-first-table-naming-convention"></a>Benennungs Konvention für Code First Tabelle

EF6 führt den Entitäts Klassennamen über einen pluralisierungs Dienst aus, um den Standard Tabellennamen zu berechnen, dem die Entität zugeordnet ist.

EF Core verwendet den Namen `DbSet` der Eigenschaft, in der die Entität für den abgeleiteten Kontext verfügbar gemacht wird. Wenn die Entität nicht über eine `DbSet` -Eigenschaft verfügt, wird der Klassenname verwendet.
