---
title: SQLite-Datenbank-Anbieter - Einschränkungen – EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: eaa7d5b1496172e4f3821433a1cd098ee7e8b737
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394803"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF Core-Datenbank-Anbieter-Einschränkungen

Der SQLite-Anbieter hat einige Einschränkungen für Migrationen. Die meisten dieser Einschränkungen sind ein Ergebnis von Einschränkungen in der zugrunde liegenden SQLite-Datenbank-Engine und sind nicht spezifisch für EF.

## <a name="modeling-limitations"></a>Modellierungseinschränkungen

Die allgemeine relationale-Bibliothek, die (vom Anbieter für Entity Framework relationaler Datenbanken freigegeben) definiert die APIs für die Modellierung der Konzepte, die für die meisten relationalen Datenbank-Engines gelten. Eine Reihe von diese Konzepte werden von der SQLite-Anbieter nicht unterstützt.

* Schemata
* Sequenzen
* Berechnete Spalten

## <a name="query-limitations"></a>Abfrage-Einschränkungen

SQLite bietet keine native Unterstützung für die folgenden Datentypen. EF Core kann lesen und Schreiben Sie Werte für diese Typen und Abfragen hinsichtlich ihrer Gleichheit (`where e.Property == value`) wird ebenfalls unterstützt. Andere Vorgänge, wie jedoch Vergleich aus, und Sortierung ist die Bewertung auf dem Client erforderlich.

* DateTimeOffset
* Decimal
* TimeSpan
* UInt64

Anstelle von `DateTimeOffset`, es wird empfohlen, DateTime-Werte. Bei der Behandlung von verschiedenen Zeitzonen wird empfohlen, beim Konvertieren der Werte in UTC, bevor Sie speichern, und klicken Sie dann zurück in die entsprechende Zeitzone konvertieren.

Die `Decimal` bietet ein hohes Maß an Genauigkeit. Wenn Sie diesen Grad an Genauigkeit nicht benötigen, wird jedoch empfohlen, stattdessen mit double. Sie können eine [Wertkonverter](../../modeling/value-conversions.md) weiterhin Dezimalzahl in Ihren Klassen.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a>Einschränkungen für Migrationen

Die SQLite-Datenbank-Engine unterstützt nicht mehrere Schemavorgänge, die von der Mehrheit der anderen relationalen Datenbanken unterstützt werden. Wenn Sie versuchen, eine nicht unterstützte Vorgänge auf eine SQLite-Datenbank anwenden und dann eine `NotSupportedException` ausgelöst.

| Vorgang            | Unterstützt? | Erfordert version |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| CreateTable          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DropTable            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✔          | 2.2.2            |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (ohne-Op)  | 2.0              |
| DropSchema           | ✔ (ohne-Op)  | 2.0              |
| Insert               | ✔          | 2.0              |
| Update               | ✔          | 2.0              |
| Löschen               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Migrationen Einschränkungen problemumgehung

Sie können einige umgehen dieser Einschränkungen durch Schreiben von Code manuell in Ihre Migrationen zu führen Sie eine Tabelle neu zu erstellen. Eine Tabellenneuerstellung umfasst Umbenennen der vorhandenen Tabelle, Erstellen einer neuen Tabelle, Kopieren von Daten in die neue Tabelle und Löschen der alten Tabelle. Sie benötigen, verwenden Sie die `Sql(string)` Methode, um einige dieser Schritte durchzuführen.

Finden Sie unter [machen andere Arten von Tabelle Schemaänderungen](http://sqlite.org/lang_altertable.html#otheralter) in der SQLite-Dokumentation für weitere Details.

In Zukunft EF einige dieser Operationen unterstützen möglicherweise mit den Ansatz mit Tabelle Neuerstellung im Hintergrund. Sie können [verfolgen Sie dieses Feature auf unserem GitHub-Projekt](https://github.com/aspnet/EntityFrameworkCore/issues/329).
