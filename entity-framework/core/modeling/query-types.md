---
title: Abfragetypen - EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: dfd08cd1c30debddc79740bbf05c39c22e973855
ms.sourcegitcommit: 01b5cf3b7c983bcced91e7cc4c78391ced2d2caa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="query-types"></a>Abfragetypen
> [!NOTE]
> Dieses Feature ist neu in EF Core 2.1

Abfragetypen können nur-Lese Abfrage Ergebnistypen, die dem EF-Core-Modell hinzugefügt werden können. Abfragetypen aktivieren die Ad-hoc-Abfragen (z. B. anonyme Typen), sondern sind flexibler, da sie die angegebene Zuordnungskonfiguration aufweisen können.

Sie sind konzeptionell identisch mit Entitätstypen:

- Sie sind POCO C#-Typen, die entweder im Modell hinzugefügt werden ```OnModelCreating``` mithilfe der ```ModelBuilder.Query``` -Methode, oder über eine DbContext ""-seteigenschaft (Abfrage eine solche Eigenschaft Datentypen als typisiert ist ```DbQuery<T>``` statt ```DbSet<T>```).
- Sie unterstützt viele der gleichen Mappingfunktionen als reguläre Entitätstypen. Z. B. Vererbungsmapping, Navigationen (siehe unten Limitiations) und auf den relationalen Speicher, die Möglichkeit, die Ziel-Datenbank-Schemaobjekten über konfigurieren ```ToTable```, ```HasColumn``` fluent-API-Methoden (oder datenanmerkungen).

Abfragetypen unterscheiden sich von der Entität Typen in, das sie:

- Benötigen Sie keinen Schlüssel definiert werden.
- Durch das System zur Änderungsnachverfolgung werden nie nachverfolgt werden.
- Gemäß der Konvention werden nie ermittelt werden.
- Unterstützen nur eine Teilmenge der Navigation Mappingfunktionen – insbesondere, die sie möglicherweise nie dienen als das prinzipalende der Beziehung.
- Zugeordnet werden kann, um eine _Abfrage definieren_ -eine definierende Abfrage ist eine sekundäre Abfrage, die eine Datenquelle für den Abfragetyp dient.

Einige der wichtigsten Verwendungsszenarien für Abfragetypen sind:

- Zuordnung zu Datenbanksichten.
- Zuordnung zu Tabellen, die nicht über einen definierten Primärschlüssel verfügen.
- Dient als Rückgabetyp für ad-hoc- ```FromSql()``` Abfragen.
- Zuordnen zu Abfragen, die im Modell definiert.

> [!TIP]
> Zuordnen von Abfragetyp zu einer Datenbankansicht erfolgt mithilfe der ```ToTable``` fluent-API.

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

Als Nächstes konfigurieren wir den Abfragetyp in _OnModelCreating_ mithilfe der ```modelBuilder.Query<T>``` API.
Wir verwenden standard fluent-Konfigurations-APIs, um die Zuordnung für den Abfragetyp zu konfigurieren:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Schließlich können wir die Datenbanksicht auf die übliche Weise Abfragen:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Beachten Sie, dass wir auch eine Ebene Abfrage Kontexteigenschaft (DbQuery) fungieren als Stamm für Abfragen auf diesen Typ definiert haben.
