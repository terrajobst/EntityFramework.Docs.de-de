---
title: SQLite-Datenbank-Anbieter - Einschränkungen – EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 69c40fcd8b7ddb925728b1bad9992ad2a81e7540
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994663"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF Core-Datenbank-Anbieter-Einschränkungen

Der SQLite-Anbieter hat einige Einschränkungen für Migrationen. Die meisten dieser Einschränkungen sind ein Ergebnis von Einschränkungen in der zugrunde liegenden SQLite-Datenbank-Engine und sind nicht spezifisch für EF.

## <a name="modeling-limitations"></a>Modellierungseinschränkungen

Die allgemeine relationale-Bibliothek, die (vom Anbieter für Entity Framework relationaler Datenbanken freigegeben) definiert die APIs für die Modellierung der Konzepte, die für die meisten relationalen Datenbank-Engines gelten. Eine Reihe von diese Konzepte werden von der SQLite-Anbieter nicht unterstützt.

* Schemata
* Sequenzen

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
| "RenameColumn"         | ✗          |                  |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (ohne-Op)  | 2.0              |
| DropSchema           | ✔ (ohne-Op)  | 2.0              |
| Insert               | ✔          | 2.0              |
| Update               | ✔          | 2.0              |
| Löschen               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Migrationen Einschränkungen problemumgehung

Sie können einige umgehen dieser Einschränkungen durch Schreiben von Code manuell in Ihre Migrationen zu führen Sie eine Tabelle neu zu erstellen. Eine tabellenneuerstellung umfasst das Umbenennen der vorhandenen Tabelle, eine neue Tabelle erstellen, Kopieren von Daten in die neue Tabelle und löschen die alte Tabelle. Sie benötigen, verwenden Sie die `Sql(string)` Methode, um einige dieser Schritte durchzuführen.

Finden Sie unter [machen andere Arten von Tabelle Schemaänderungen](http://sqlite.org/lang_altertable.html#otheralter) in der SQLite-Dokumentation für weitere Details.

In Zukunft EF einige dieser Operationen unterstützen möglicherweise mit den Ansatz mit Tabelle Neuerstellung im Hintergrund. Sie können [verfolgen Sie dieses Feature auf unserem GitHub-Projekt](https://github.com/aspnet/EntityFrameworkCore/issues/329).
