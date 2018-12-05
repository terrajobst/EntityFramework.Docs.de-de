---
title: Seeding von Daten – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 8f28dfea12461572ade8fbf3910ebd216dafb389
ms.sourcegitcommit: fa863883f1193d2118c2f9cee90808baa5e3e73e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52857428"
---
# <a name="data-seeding"></a>Data Seeding

Seeding von Daten ist der Prozess für das Auffüllen einer Datenbank mit einem anfänglichen Satz von Daten.

Es gibt verschiedene Möglichkeiten, die dies in EF Core erreicht werden kann:
* Seed-Daten des Modells
* Manuelle Migration Anpassung
* Benutzerdefinierte Initialisierungslogik

## <a name="model-seed-data"></a>Seed-Daten des Modells

> [!NOTE]
> Dieses Feature ist neu in EF Core 2.1.

Im Gegensatz zu kann in EF6 in EF Core seeding von Daten mit einem Entitätstyp als Teil der Modellkonfiguration verknüpft sein. EF Core [Migrationen](xref:core/managing-schemas/migrations/index) bestimmen dann automatisch, welche INSERT-, UPDATE- oder DELETE-Operationen beim Upgrade der Datenbank auf eine neue Version des Datenmodells angewendet werden müssen.

> [!NOTE]
> Migrationen berücksichtigt nur modelländerungen beim bestimmen, welcher Vorgang ausgeführt werden soll, um die Seed-Daten in den gewünschten Zustand zu erhalten. Daher können Änderungen an den Daten, die außerhalb von Migrationen durchgeführt getrennt werden, oder führen zu einem Fehler.

Dadurch wird beispielsweise startwertdaten für konfiguriert eine `Blog` in `OnModelCreating`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Um Entitäten hinzuzufügen, die eine Beziehung die Fremdschlüsselwerte müssen angegeben werden:

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Wenn der Entitätstyp alle Eigenschaften, die Shadow-Status, die eine anonyme Klasse verwendet werden kann aufweist, um die Werte bereitzustellen:

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Der eigenen Entität, die Typen, die auf ähnliche Weise ausgeführt werden können:

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Finden Sie unter den [vollständigen Beispielprojekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) für weiteren Kontext.

Nachdem die Daten, dem Modell hinzugefügt wurde [Migrationen](xref:core/managing-schemas/migrations/index) sollte verwendet werden, um die Änderungen zu übernehmen.

> [!TIP]
> Wenn Sie zum Anwenden von Migrationen als Teil einer automatisierten Bereitstellung benötigen können Sie [erstellen Sie ein SQL­Skript](xref:core/managing-schemas/migrations/index#generate-sql-scripts) können, die vor der Ausführung eine Vorschau angezeigt werden.

Alternativ können Sie `context.Database.EnsureCreated()` zum Erstellen einer neuen Datenbank, die die Seed-Daten, z. B. für eine Testdatenbank oder bei Verwendung von in-Memory-Anbieter oder eine nicht-Relation-Datenbank enthält. Beachten Sie, dass bei die Datenbank bereits vorhanden ist, `EnsureCreated()` wird weder die Schema noch Seed-Daten in der Datenbank aktualisiert. Sie sollten nicht für relationale Datenbanken aufrufen `EnsureCreated()` , wenn Sie Migrationen verwenden möchten.

Diese Art von Seed-Daten wird durch Migrationen verwaltet, und das Skript zum Aktualisieren der Daten, die bereits in der Datenbank ist ohne Verbindung zur Datenbank generiert werden muss. Diese erzwingt einige Einschränkungen:
* Wert des Primärschlüssels muss angegeben werden, auch wenn es in der Regel von der Datenbank generiert wird. Er wird zum Erkennen von datenänderungen zwischen Migrationen verwendet werden.
* Zuvor werden per Seeding hinzugefügte Daten entfernt werden, wenn der Primärschlüssel in keiner Weise geändert wird.

Aus diesem Grund eignet sich diese Funktion am besten für statische Daten, die ist nicht dazu gedacht, die außerhalb von Migrationen zu ändern, und hängt nicht alles in der Datenbank, z. B. Postleitzahlen.

Wenn Ihr Szenario eine der folgenden enthält wird empfohlen, die angepasste Initialisierung von Logik, die im vorherigen Abschnitt beschrieben zu verwenden:
* Temporäre Daten für Tests
* Daten, die abhängig von Datenbankstatus
* Daten, die Schlüsselwerte generiert werden, von der Datenbank, einschließlich der Entitäten, die alternativen Schlüssel als Identität verwendet.
* Daten, die eine benutzerdefinierte Transformation erfordert, (, erfolgt nicht durch [Wert Konvertierungen](xref:core/modeling/value-conversions)), z. B. eine Kennwort-hashing
* Daten, die Aufrufe an externe API, z.B. ASP.NET Core Identity-Rollen und Benutzer erstellen erfordern

## <a name="manual-migration-customization"></a>Manuelle Migration Anpassung

Wenn eine Migration die Änderungen an den Daten, die mit angegebenen hinzugefügt wird `HasData` werden in Aufrufe an transformiert `InsertData()`, `UpdateData()`, und `DeleteData()`. Eine Möglichkeit, arbeiten umgehungslösungen zu einigen der Einschränkungen der `HasData` besteht darin, diese Aufrufe manuell hinzufügen oder [benutzerdefinierte Vorgänge](xref:core/managing-schemas/migrations/operations) zur Migration stattdessen.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Benutzerdefinierte Initialisierungslogik

Eine einfache und leistungsstarke Möglichkeit zum seeding von Daten ausführen ist die Verwendung [ `DbContext.SaveChanges()` ](xref:core/saving/index) vor der hauptanwendung Logik ihrer Ausführung beginnt.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> Der seedingcode muss nicht Teil der normal-app-Ausführung, wie dies von Parallelitätsproblemen führen kann, wenn mehrere Instanzen ausgeführt werden und müsste auch die app keine Berechtigung zum Ändern des Datenbankschemas.

Je nach den Einschränkungen der Bereitstellung kann der Initialisierungscode auf unterschiedliche Weise ausgeführt werden:
* Lokales Ausführen der app für die Initialisierung
* Die Initialisierung-app mit der Haupt-app, die Initialisierungsroutine aufrufen, und deaktivieren, oder entfernen die Initialisierung-app bereitstellen.

Dies kann in der Regel mit automatisiert werden [Veröffentlichungsprofile](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).
