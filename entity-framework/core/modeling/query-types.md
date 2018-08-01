---
title: Abfragetypen – EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 5a2cd451da8833daf2c315419559eb4a2c705b13
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388480"
---
# <a name="query-types"></a>Abfragetypen
> [!NOTE]
> Dieses Feature ist neu in EF Core 2.1

Zusätzlich zu den Entitätstypen, kann ein EF Core-Modell enthalten _Abfragetypen_, die zum Datenbankabfragen für Daten ausführen, auf denen die Entitätstypen zugeordnet wird nicht verwendet werden können.

Abfragetypen haben viele ähnlichkeiten mit Entitätstypen:

- Sie können auch hinzugefügt werden, das Modell entweder in `OnModelCreating`, oder über eine "set"-Eigenschaft in einer abgeleiteten _"DbContext"_.
- Sie unterstützen viele der gleichen Funktionen für die Zuordnung, wie Vererbung Zuordnung Navigationseigenschaften (Siehe Einschränkungen unten) und relationale Speicher, die Möglichkeit, die Ziel-Datenbank-Objekte und Spalten über die Methoden der fluent-API oder datenanmerkungen zu konfigurieren.

Aber Typen diese Entität unterscheiden in, das sie:

- Erfordern Sie keinen Schlüssel definiert werden.
- Werden nie damit Änderungen nachverfolgt, auf die _"DbContext"_ und daher nie eingefügt, aktualisiert oder gelöscht werden in der Datenbank.
- Gemäß der Konvention werden nicht ermittelt werden.
- Unterstützen Sie nur eine Teilmenge der Funktionen für die Zuordnung von Navigation - insbesondere:
  - Sie können nicht als das prinzipalende der Beziehung fungieren.
  - Sie können nur auf Entitäten verweisen verweisnavigationseigenschaften enthalten.
  - Entitäten können keine Navigationseigenschaften für Abfragetypen enthalten.
- Adressiert werden, auf die _ModelBuilder_ mithilfe der `Query` Methode anstelle der `Entity` Methode.
- Zugeordnet sind, auf die _"DbContext"_ über Eigenschaften vom Typ `DbQuery<T>` statt `DbSet<T>`
- Datenbankobjekte mit zugeordnet sind die `ToView` Methode statt `ToTable`.
- Zugeordnet werden kann eine _Abfrage definiert_ : Definieren der Abfrage ist eine sekundäre Abfrage, die im Modell, die eine Datenquelle für den Abfragetyp dient deklariert.

Einige der wichtigsten Verwendungsszenarien für Abfragetypen sind:

- Dient als Rückgabetyp für ad-hoc- `FromSql()` Abfragen.
- Zuordnung zu Datenbankansichten.
- Zuordnung zu Tabellen, die nicht über einen definierten Primärschlüssel verfügen.
- Zuordnen zu Abfragen, die im Modell definiert.

> [!TIP]
> Zuordnen der Abfragetyp auf ein Datenbankobjekt wird erreicht, indem die `ToView` fluent-API. Aus der Perspektive von EF Core, das Datenbankobjekt, das in dieser Methode wird ein _Ansicht_, was bedeutet, dass es als eine nur-Lese Query-Datenquelle behandelt wird kann nicht Ziel von Updates, insert oder delete-Operationen. Dies bedeutet jedoch nicht, dass das Datenbankobjekt tatsächlich erforderlich ist ist, eine Datenbanksicht sein - es kann auch eine Datenbanktabelle, die als schreibgeschützt behandelt werden. Im Gegensatz dazu für Entitätstypen, EF Core geht davon aus, dass ein Datenbankobjekt in angegeben die `ToTable` Methode behandelt werden kann, als eine _Tabelle_, was bedeutet, dass er kann als ein Query-Datenquelle verwendet werden, sondern auch das Ziel aktualisieren, löschen und einfügen Vorgänge. In der Tat können Sie angeben, den Namen einer Datenbanksicht in `ToTable` und alles sollte gut funktionieren, solange die Ansicht für die für die Datenbank aktualisierbar sein, konfiguriert ist.

## <a name="example"></a>Beispiel

Das folgende Beispiel zeigt, wie Sie den Abfragetyp zu verwenden, um eine Datenbankansicht abzufragen.

> [!TIP]
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes) finden Sie auf GitHub.

Zuerst definieren wir ein einfaches Blog und Post-Modell:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

Als Nächstes definieren wir eine einfache Datenbank-Ansicht, die wir Abfragen die Anzahl an Beiträgen, die jeden Blog zugeordnet werden kann:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

Als Nächstes definieren wir eine Klasse, um das Ergebnis aus der Datenbankansicht enthalten soll:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

Als Nächstes konfigurieren wir den Abfragetyp in _"onmodelcreating"_ mithilfe der `modelBuilder.Query<T>` API.
Verwenden wir standard fluent-Konfigurations-APIs, um die Zuordnung für den Abfragetyp zu konfigurieren:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

Schließlich können wir die Datenbanksicht auf die übliche Weise Abfragen:

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Beachten Sie, dass wir auch eine Ebene Abfrageeigenschaft (DbQuery) fungieren als Stamm für Abfragen gegen diese Art von Kontext definiert haben.
