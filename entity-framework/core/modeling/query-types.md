---
title: Abfragetypen - EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: f16e3a130f3a4f92b2bf6014f2df0ca4eec56a25
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163173"
---
# <a name="query-types"></a>Abfragetypen
> [!NOTE]
> Dieses Feature ist neu in EF Core 2.1

Zusätzlich zu den Entitätstypen, kann ein Modell EF Core enthalten _Abfragen Typen_, die zum Ausführen von Datenbankabfragen für Daten, die Entitätstypen zugeordnet wird nicht verwendet werden können.

Abfragetypen haben viele ähnlichkeiten mit Entitätstypen:

- Sie können auch hinzugefügt werden, das Modell entweder in `OnModelCreating`, oder über eine "set"-Eigenschaft auf eine abgeleitete _DbContext_.
- Sie unterstützen viele derselben Mappingfunktionen, wie Vererbung, Zuordnen von Navigationseigenschaften (siehe unten Einschränkungen) und auf den relationalen Speicher, die Möglichkeit, die Ziel-Datenbankobjekte und Spalten über die fluent-API-Methoden oder datenanmerkungen konfigurieren.

Jedoch Typen verschieden von Entität in, das sie:

- Benötigen Sie keinen Schlüssel definiert werden.
- Nie für Änderungen nachverfolgt werden, auf die _DbContext_ und daher nie eingefügt, aktualisiert oder gelöscht werden in der Datenbank.
- Gemäß der Konvention werden nie ermittelt werden.
- Nur unterstützen Sie eine Teilmenge der Navigation Mappingfunktionen - insbesondere:
  - Sie können nie als das prinzipalende der Beziehung fungieren.
  - Sie können nur Verweis Navigationseigenschaften verweist auf Entitäten enthalten.
  - Entitäten darf keine Navigationseigenschaften Abfragetypen enthalten.
- Adressiert sind, auf die _ModelBuilder_ mithilfe der `Query` Methode statt über das `Entity` Methode.
- Zugeordnet sind, auf die _DbContext_ über Eigenschaften des Typs `DbQuery<T>` statt `DbSet<T>`
- Zugeordnet sind, auf der Datenbankobjekte, die mit der `ToView` -Methode, anstatt `ToTable`.
- Zugeordnet werden kann ein _Abfrage definieren_ – eine Abfrage definieren einer sekundären Abfrage, die im Modell, die eine Datenquelle für den Abfragetyp fungiert deklariert ist.

Einige der wichtigsten Verwendungsszenarien für Abfragetypen sind:

- Dient als Rückgabetyp für ad-hoc- `FromSql()` Abfragen.
- Zuordnung zu Datenbanksichten.
- Zuordnung zu Tabellen, die nicht über einen definierten Primärschlüssel verfügen.
- Zuordnen zu Abfragen, die im Modell definiert.

> [!TIP]
> Zuordnungstyp eine Abfrage mit einem Datenbankobjekt erfolgt mithilfe der `ToView` fluent-API. Aus der Perspektive des Kerns EF wird in dieser Methode angegebene Datenbankobjekt ist eine _Ansicht_, dies bedeutet, dass es als Abfragequelle für nur-Lese behandelt kann nicht Ziel von Updates, insert oder delete-Operationen. Dies bedeutet jedoch nicht, dass das Datenbankobjekt tatsächlich erforderlich ist, damit eine Datenbanksicht werden - es kann auch eine Datenbanktabelle, die als schreibgeschützt behandelt werden. Umgekehrt für Entitätstypen, EF Core setzt voraus, dass ein Datenbankobjekt in angegeben die `ToTable` Methode behandelt werden kann, als ein _Tabelle_, was bedeutet, dass es als Abfragequelle für verwendet werden können, aber auch das Ziel aktualisieren, löschen und einfügen DDL-Vorgänge. Sie können in der Tat Geben Sie den Namen einer Datenbanksicht in `ToTable` und alles sollte einwandfrei funktionieren, solange die Ansicht für die Datenbank aktualisierbar konfiguriert ist.

## <a name="example"></a>Beispiel

Im folgende Beispiel wird gezeigt, wie Abfragetyp zum Abfragen einer Datenbankansicht verwendet wird.

> [!TIP]
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) finden Sie auf GitHub.

Zuerst definieren wir ein einfache Blog und Post-Modell:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

Als Nächstes definieren wir eine einfache Datenbank-Sicht, die uns, Fragen Sie die Anzahl der Beiträge, die jeden Blog zugeordnet werden kann:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

Als Nächstes definieren wir eine Klasse, um das Ergebnis aus der Datenbankansicht aufnehmen:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

Als Nächstes konfigurieren wir den Abfragetyp in _OnModelCreating_ mithilfe der `modelBuilder.Query<T>` API.
Wir verwenden standard fluent-Konfigurations-APIs, um die Zuordnung für den Abfragetyp zu konfigurieren:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Schließlich können wir die Datenbanksicht auf die übliche Weise Abfragen:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Beachten Sie, dass wir auch eine Ebene Abfrage Kontexteigenschaft (DbQuery) fungieren als Stamm für Abfragen auf diesen Typ definiert haben.
