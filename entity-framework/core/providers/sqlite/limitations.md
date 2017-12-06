---
title: "SQLite-Datenbank-Anbieter - Einschränkungen - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF Core Datenbank Anbietereinschränkungen

Der SQLite-Anbieter hat einige Migrationen Einschränkungen. Die meisten dieser Einschränkungen sind Beschränkungen in der zugrunde liegende Datenbankmodul SQLite zurückzuführen und sind nicht spezifisch für EF.

## <a name="modeling-limitations"></a>Modellieren von Einschränkungen

Die allgemeine relationale Bibliothek (shared von Entity Framework, relationale Datenbank-Anbieter) definiert APIs für die Modellierung Konzepte, die für die meisten relationale Datenbankmodule gelten. Eine Reihe von dieser Konzepte werden von der SQLite-Anbieter nicht unterstützt.

* Schemata
* Sequenzen

## <a name="migrations-limitations"></a>Migrationen von Einschränkungen

Die SQLite-Datenbank-Engine unterstützt nicht mehrere Schemavorgänge, die von der Mehrheit der andere relationalen Datenbanken unterstützt werden. Wenn Sie versuchen, einen nicht unterstützten Vorgänge anwenden auf eine SQLite-Datenbank wird eine `NotSupportedException` ausgelöst.

| Vorgang            | Unterstützt? |
| -------------------- | ---------- |
| AddColumn            | ✔          |
| AddForeignKey        | ✗          |
| AddPrimaryKey        | ✗          |
| AddUniqueConstraint  | ✗          |
| AlterColumn          | ✗          |
| CreateIndex          | ✔          |
| CreateTable          | ✔          |
| DropColumn           | ✗          |
| DropForeignKey       | ✗          |
| DropIndex            | ✔          |
| DropPrimaryKey       | ✗          |
| DropTable            | ✔          |
| DropUniqueConstraint | ✗          |
| "RenameColumn"         | ✗          |
| RenameIndex          | ✗          |
| RenameTable          | ✔          |

## <a name="migrations-limitations-workaround"></a>Migrationen Einschränkungen umgehen

Sie können einige umgehen dieser Einschränkungen durch Schreiben von Code manuell in Ihrer Migrationen, führen Sie eine Tabelle neu erstellen. Eine Neuerstellung der Tabelle umfasst das Umbenennen der vorhandenen Tabelle, eine neue Tabelle erstellen, Kopieren von Daten in die neue Tabelle und löschen die alte Tabelle. Sie benötigen, verwenden Sie die `Sql(string)` Methode, um einige der folgenden Schritte ausführen.

Finden Sie unter [machen andere Arten von Tabelle Schemaänderungen](http://sqlite.org/lang_altertable.html#otheralter) in der Dokumentation SQLite Weitere Details.

In der Zukunft EF einige dieser Vorgänge unterstützen möglicherweise mit der Tabelle Rebuild Ansatz im Hintergrund. Sie können [verfolgen Sie diese Funktion auf unserer GitHub-Projekt](https://github.com/aspnet/EntityFramework/issues/329).
