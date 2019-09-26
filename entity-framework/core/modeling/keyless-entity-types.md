---
title: Typen von Keyless-Entitäten-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: e78b9f91fd2505de300ced7b5e73291b5d1ad3b4
ms.sourcegitcommit: 7bc43f21e7bdd64926314ea949aae689f1911956
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266774"
---
# <a name="keyless-entity-types"></a>Schlüssellose Entitätstypen
> [!NOTE]
> Diese Funktion wurde in EF Core 2,1 unter dem Namen der Abfrage Typen hinzugefügt. In EF Core 3,0 wurde das Konzept in schlüssellose Entitäts Typen umbenannt.

Zusätzlich zu regulären Entitäts Typen kann ein EF Core Modell _Schlüssel-Entitäts Typen_enthalten, die verwendet werden können, um Datenbankabfragen für Daten auszuführen, die keine Schlüsselwerte enthalten.

## <a name="keyless-entity-types-characteristics"></a>Merkmale von Keyless-Entitäts Typen

Schlüssellose Entitäts Typen unterstützen viele der gleichen Zuordnungs Funktionen wie reguläre Entitäts Typen wie Vererbungs Zuordnung und Navigations Eigenschaften. Für relationale Speicher können sie die Ziel-Datenbank-Objekte und Spalten über die Methoden der fluent-API oder datenanmerkungen konfigurieren.

Sie unterscheiden sich jedoch von regulären Entitäts Typen darin, dass Sie:

- Ein Schlüssel kann nicht definiert werden.
- Werden nie nach Änderungen im _dbcontext_ nachverfolgt und daher nie in der Datenbank eingefügt, aktualisiert oder gelöscht.
- Gemäß der Konvention werden nicht ermittelt werden.
- Unterstützt nur eine Teilmenge der Navigations Zuordnungsfunktionen, insbesondere:
  - Sie können nicht als das prinzipalende der Beziehung fungieren.
  - Sie haben möglicherweise keine Navigation zu besitzende Entitäten.
  - Sie können nur Verweis Navigations Eigenschaften enthalten, die auf reguläre Entitäten verweisen.
  - Entitäten können keine Navigations Eigenschaften für Entitäts Typen ohne Schlüssel enthalten.
- Muss mit `.HasNoKey()` Methoden aufzurufen konfiguriert werden.
- Kann einer _definierenden Abfrage_zugeordnet werden. Eine definierende Abfrage ist eine im Modell deklarierte Abfrage, die als Datenquelle für einen schlüssellosen Entitätstyp fungiert.

## <a name="usage-scenarios"></a>Verwendungsszenarien

Einige der wichtigsten Verwendungs Szenarien für Typen von schlüssellosen Entitäten sind:

- Fungieren als Rückgabetyp für unformatierte [SQL-Abfragen](xref:core/querying/raw-sql).
- Zuordnung zu Daten Bank Sichten, die keinen Primärschlüssel enthalten.
- Zuordnung zu Tabellen, die nicht über einen definierten Primärschlüssel verfügen.
- Zuordnen zu Abfragen, die im Modell definiert.

## <a name="mapping-to-database-objects"></a>Zuordnen von Datenbankobjekten

Die Zuordnung eines Entitäts Typs mit einer anderen Tastatur zu einem Daten Bank `ToTable` Objekt `ToView` erfolgt mithilfe der-oder-überflüssigen API. Aus der Perspektive von EF Core, das Datenbankobjekt, das in dieser Methode wird ein _Ansicht_, was bedeutet, dass es als eine nur-Lese Query-Datenquelle behandelt wird kann nicht Ziel von Updates, insert oder delete-Operationen. Dies bedeutet jedoch nicht, dass das Datenbankobjekt tatsächlich eine Daten Bank Sicht sein muss. Alternativ kann es sich um eine Datenbanktabelle handeln, die als schreibgeschützt behandelt wird. Im Gegensatz dazu geht EF Core bei regulären Entitäts Typen davon aus, dass ein in `ToTable` der-Methode angegebenes Datenbankobjekt als _Tabelle_behandelt werden kann. Dies bedeutet, dass es als Abfrage Quelle, aber auch als Ziel für Update-, DELETE-und INSERT-Vorgänge verwendet werden kann. In der Tat können Sie angeben, den Namen einer Datenbanksicht in `ToTable` und alles sollte gut funktionieren, solange die Ansicht für die für die Datenbank aktualisierbar sein, konfiguriert ist.

> [!NOTE]
> `ToView`geht davon aus, dass das Objekt bereits in der Datenbank vorhanden ist, und wird nicht durch Migrationen erstellt.

## <a name="example"></a>Beispiel

Das folgende Beispiel zeigt, wie Sie schlüssellose Entitäts Typen verwenden, um eine Daten Bank Sicht abzufragen.

> [!TIP]
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) finden Sie auf GitHub.

Zuerst definieren wir ein einfaches Blog und Post-Modell:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

Als Nächstes definieren wir eine einfache Datenbank-Ansicht, die wir Abfragen die Anzahl an Beiträgen, die jeden Blog zugeordnet werden kann:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

Als Nächstes definieren wir eine Klasse, um das Ergebnis aus der Datenbankansicht enthalten soll:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

Als Nächstes konfigurieren wir den Entitätstyp "Schlüssel" in " _onmodelcreating_ " mithilfe der `HasNoKey` API.
Wir verwenden die fließende Konfigurations-API, um die Zuordnung für den Entitätstyp "Schlüssel" zu konfigurieren:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Als Nächstes konfigurieren wir den `DbContext` so, dass `DbSet<T>`er folgendes einschließt:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Schließlich können wir die Datenbanksicht auf die übliche Weise Abfragen:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Beachten Sie, dass wir auch eine Kontext Stufen-Abfrage Eigenschaft (dbset) definiert haben, die als Stamm für Abfragen dieses Typs fungiert.
