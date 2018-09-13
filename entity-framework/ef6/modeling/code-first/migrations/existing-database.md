---
title: Code First-Migrationen mit einer vorhandenen Datenbank - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
ms.openlocfilehash: 77370ec7d922b8324b924a0b4aca3e58f5ec6066
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490570"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Code First-Migrationen mit einer vorhandenen Datenbank
> [!NOTE]
> **EF4.3 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 4.1 eingeführt wurden. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.

Dieser Artikel befasst sich mit Code First-Migrationen mit einer vorhandenen Datenbank, eine, die von Entity Framework erstellt wurde.

> [!NOTE]
> In diesem Artikel wird davon ausgegangen, dass Sie wissen, wie Code First-Migrationen in grundlegenden Szenarien verwenden. Wenn Sie dies nicht tun, müssen Sie lesen [Code First-Migrationen](~/ef6/modeling/code-first/migrations/index.md) vor dem fortfahren.

## <a name="screencasts"></a>Screencasts

Wenn Sie stattdessen sehen Sie sich einen Screencast als diesen Artikel lesen würde, behandelt die beiden folgenden Videos den gleichen Inhalt wie in diesem Artikel.

### <a name="video-one-migrations---under-the-hood"></a>Video 1: "Migrations - im Hintergrund"

[Dieser Screencast](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) befasst sich mit Migrationen wie nachverfolgt und Informationen über das Modell zum Erkennen von Änderungen des Datenmodells verwendet.

### <a name="video-two-migrations---existing-databases"></a>Video 2: "Migrations - vorhandene Datenbanken"

Erstellen auf den Konzepten von der früheren Video behandelt, [diesem Screencast](http://channel9.msdn.com/blogs/ef/migrations-existing-databases) wird beschrieben, wie aktivieren und Verwenden von Migrationen mit einer vorhandenen Datenbank.

## <a name="step-1-create-a-model"></a>Schritt 1: Erstellen eines Modells

Ein Code First-Modell zu erstellen, die die vorhandene Datenbank ausgerichtet ist, wird der erste Schritt sein. Die [Code First mit einer vorhandenen Datenbank](~/ef6/modeling/code-first/workflows/existing-database.md) Thema enthält ausführliche Anleitungen hierzu.

>[!NOTE]
> Es ist wichtig, befolgen Sie die restlichen Schritte in diesem Thema, bevor Änderungen an Ihrem Modell, das Änderungen am Datenbankschema erfordern würden. Die folgenden Schritte erfordern das Modell, das synchronisiert werden mit dem Datenbankschema.

## <a name="step-2-enable-migrations"></a>Schritt 2: Aktivieren von Migrationen

Der nächste Schritt ist, um Migrationen zu aktivieren. Hierzu können Sie mit der **Enable-Migrations** im Paket-Manager-Konsole den Befehl.

Dieser Befehl erstellen Sie einen Ordner, in der Projektmappe namens "Migrations", und fügen eine einzelne Klasse in dieser Konfiguration aufgerufen. Die Konfigurationsklasse ist, in dem Sie Migrationen für Ihre Anwendung konfigurieren, Sie finden weitere Informationen finden sie unter den [Code First-Migrationen](~/ef6/modeling/code-first/migrations/index.md) Thema.

## <a name="step-3-add-an-initial-migration"></a>Schritt 3: Fügen Sie eine anfängliche Migration hinzu.

Nach der Migration erstellt und angewendet wurden ändert sich die lokale Datenbank, die auch möglicherweise Sie diese anwenden möchten, auf andere Datenbanken. Z. B. die lokale Datenbank ist möglicherweise eine Testdatenbank und Sie möchten schließlich auch die Änderungen auf eine Produktionsdatenbank anwenden und/oder andere Entwickler Testen von Datenbanken. Es gibt zwei Optionen für diesen Schritt und diejenige aus, die Sie auswählen sollten abhängig ist, unabhängig davon, ob das Schema für alle anderen Datenbanken leer ist oder derzeit dem Schema der lokalen Datenbank entsprechen.

-   **Option 1: Verwenden Sie vorhandenes Schema als Ausgangspunkt.** Sie sollten diesen Ansatz verwenden, wenn andere Datenbanken, denen Migrationen, in der Zukunft angewendet werden wie Ihre lokale Datenbank verfügt derzeit über das gleiche Schema haben. Beispielsweise können Sie diese verwenden, wenn der lokale Testdatenbank derzeit v1 der Produktionsdatenbank entspricht, und Sie später diese Migrationen zum Aktualisieren der Produktionsdatenbank auf v2 übernehmen.
-   **Option 2: Verwenden Sie leere Datenbank als Ausgangspunkt.** Sie sollten diesen Ansatz verwenden, bei anderen Datenbanken, denen Migrationen in der Zukunft auf angewendet werden soll, leer sind (oder noch nicht vorhanden sind). Beispielsweise können Sie dies verwenden, wenn Sie Schritte bei der Entwicklung Ihrer Anwendung eine Testdatenbank verwenden, aber ohne Verwendung von Migrationen und Sie werden später eine Produktionsdatenbank von Grund auf neu erstellen möchten.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Option 1: Verwenden von vorhandenen Schema als Ausgangspunkt

Code First-Migrationen verwendet eine Momentaufnahme des Modells gespeichert wird, in der letzten Migration zum Erkennen von Änderungen am Modell (finden Sie ausführliche Informationen zu diesem [Code First-Migrationen in Teamumgebungen](~/ef6/modeling/code-first/migrations/teams.md)). Da wir davon ausgehen, dass die Datenbanken bereits das Schema des aktuellen Modells verfügen sollen, generieren wir eine leere (ohne-Op)-Migration, die das aktuelle Modell als eine Momentaufnahme verfügt.

1.  Führen Sie die **Add-Migration InitialCreate – IgnoreChanges** im Paket-Manager-Konsole den Befehl. Dadurch wird eine leere Migration mit dem aktuellen Modell als eine Momentaufnahme erstellt.
2.  Führen Sie die **Update-Database** im Paket-Manager-Konsole den Befehl. Dies gilt die InitialCreate-Migration für die Datenbank. Da die eigentliche Migration keine Änderungen enthält, wird es einfach hinzufügen, auf eine Zeile der \_ \_MigrationsHistory Tabelle gibt an, dass diese Migration bereits angewendet wurde.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>Option 2: Verwenden von leeren Datenbank als Ausgangspunkt

In diesem Fall benötigen wir Migrationen in der Lage, um die gesamte Datenbank zu erstellen, von Grund auf neu – einschließlich der Tabellen, die bereits in der lokalen Datenbank vorhanden sind. Wir werden eine InitialCreate-Migration zu erstellen, die enthält Logik zum vorhandene Schema zu erstellen. Wir stellen dann unsere vorhandenen Datenbank aussehen, als ob diese Migration bereits angewendet wurde.

1.  Führen Sie die **Add-Migration InitialCreate** im Paket-Manager-Konsole den Befehl. Dies erstellt eine Migration aus, um das vorhandene Schema zu erstellen.
2.  Kommentieren Sie den gesamten Code in der nach-oben-Methode, die neu erstellte Migration. Dadurch können wir "apply" die Migration der lokalen Datenbank ohne zu versuchen, alle Tabellen usw. neu erstellen, die bereits vorhanden.
3.  Führen Sie die **Update-Database** im Paket-Manager-Konsole den Befehl. Dies gilt die InitialCreate-Migration für die Datenbank. Da nicht die eigentliche Migration enthält Änderungen (da wir vorübergehend, kommentiert), es wird keine Zeile einfach hinzufügen der \_ \_MigrationsHistory Tabelle gibt an, dass diese Migration bereits angewendet wurde.
4.  Kommentar der Code in der nach-oben-Methode. Dies bedeutet, dass bei dieser Migration auf künftigen Datenbanken angewendet wird, wird das Schema, das in der lokalen Datenbank bereits vorhanden war durch Migrationen erstellt werden.

## <a name="things-to-be-aware-of"></a>Dinge zu beachten

Es gibt einige Dinge, denen Sie bei Verwendung von Migrationen anhand einer vorhandenen Datenbank berücksichtigen müssen.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>Standard/berechnet Namen möglicherweise nicht mit dem vorhandenen Schema übereinstimmen.

Migrationen gibt explizit an Namen für die Spalten und Tabellen bei der es erstellt das Gerüst für eine Migration. Es gibt jedoch auch andere Datenbankobjekte, die Migrationen berechnet einen Standardnamen für beim Anwenden von Migrationen. Dies schließt Indizes und foreign Key-Einschränkungen. Wenn Sie ein vorhandenes Schema zu verwenden, müssen diese berechneten Namen entsprechen möglicherweise nicht was tatsächlich in der Datenbank vorhanden ist.

Hier sind einige Beispiele für, wenn Sie dies berücksichtigen müssen:

**Wenn Sie verwendet "eine Option: vorhandenes Schema als Ausgangspunkt verwenden" aus Schritt 3:**

-   Wenn zukünftige Änderungen in Ihrem Modell erforderlich ist, ändern oder löschen keines der Datenbankobjekte, die unterschiedlich benannt ist, müssen Sie so ändern Sie die erstellte Migration durch, um den richtigen Namen zu geben. Die Migrations-APIs haben, einen optionalen Namensparameter, der Ihnen dies ermöglicht wird.
    Zum Beispiel möglicherweise das vorhandene Schema eine Post-Tabelle mit einer BlogId Fremdschlüsselspalte, die einen Index mit dem Namen IndexFk\_BlogId. Jedoch standardmäßig Migrationen würde erwarten, dass dieser Index den Namen IX\_BlogId. Wenn Sie eine Änderung an Ihrem Modell, die sich ergibt, die diesen Index löschen vornehmen, müssen Sie so ändern Sie den eingerüsteten DropIndex-Aufruf an die IndexFk\_BlogId-Namen.

**Wenn Sie verwendet ' Option zwei: leere Datenbank als Ausgangspunkt verwenden "aus Schritt 3:**

-   Die Down-Methode der anfänglichen Migration (die in eine leere Datenbank wiederhergestellt wird) für die lokale Datenbank ausführen möchten möglicherweise fehl, da Migrationen versucht werden, Indizes und foreign Key-Einschränkungen, die unter Verwendung der falschen Namen zu löschen. Dies betrifft nur die lokale Datenbank, da andere Datenbanken von Grund auf neu mithilfe der nach-oben-Methode, der die ursprüngliche Migration erstellt werden.
    Wenn Sie Ihre vorhandene lokale Datenbank auf einen leeren Zustand herabstufen möchten, ist es am einfachsten, manuell, entweder durch das Löschen der Datenbank, oder löschen alle Tabellen erfolgen. Nach dieser anfänglichen herabgestuft wird, die alle Datenbankobjekte neu ist, werden mit den Standardnamen erstellt werden, werden also dieses Problem nicht selbst erneut angezeigt.
-   Wenn zukünftige Änderungen in Ihrem Modell erforderlich ist, ändern oder löschen keines der Datenbankobjekte, die unterschiedlich benannt ist, funktioniert dies für Ihre vorhandene lokale Datenbank – nicht, da die Standardeinstellungen wird nicht mit den Namen übereinstimmen. Allerdings funktioniert es für Datenbanken, die "von Grund auf neu erstellen" erstellt wurden, da sie verwendet haben, die Standardnamen von Migrationen ausgewählt werden.
    Sie können diese Änderungen manuell vornehmen, für die vorhandene lokale Datenbank, oder erwägen Sie, dass Migrationen, erneuten Erstellen der Datenbank von Grund auf neu – wie auf anderen Computern wird.
-   Datenbanken erstellt wurden, mit der nach-oben-Methode, der Ihrem ersten Migrationsschritt möglicherweise unterscheiden sich etwas aus der lokalen Datenbank seit der berechneten Standardnamen für Indizes und foreign Key-Einschränkungen verwendet werden. Sie können auch mit zusätzlichen Indizes enden wie Migrationen Indizes für Fremdschlüsselspalten in der Standardeinstellung erstellt werden – dies möglicherweise nicht der Fall, in der ursprünglichen lokalen Datenbank waren.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>Nicht alle Datenbankobjekte werden im Modell dargestellt.

Datenbankobjekte, die nicht Teil des Modells sind, werden nicht durch Migrationen verarbeitet werden. Dies kann beinhalten, Sichten, gespeicherte Prozeduren, Berechtigungen, Tabellen, die nicht Teil des Modells, zusätzlichen Indizes usw. sind.

Hier sind einige Beispiele für, wenn Sie dies berücksichtigen müssen:

-   Unabhängig von der Option ausgewählt wird in "Schritt 3", wenn zukünftige Änderungen in Ihrem Modell erforderlich ist, ändern oder löschen diese zusätzlichen Objekte, die Migrationen nicht weiß, um diese Änderungen vorzunehmen. Z. B. Wenn Sie eine Spalte, die einen zusätzlichen Index verfügt löschen, erkennen Migrationen nicht kann der Index gelöscht werden. Sie müssen manuell auf den erstellten Migration hinzufügen.
-   Wenn Sie verwendet ' Option zwei: leere Datenbank als Ausgangspunkt verwenden ", diese zusätzlichen Objekte nicht von der Methode nach Ihrem ersten Migrationsschritt erstellt.
    Sie können ändern, die nach-oben und unten Methoden, um diese zusätzlichen Objekte Wenn erledigt werden soll. Für Objekte, die nicht nativ in der Migrations-API – z. B. Sichten unterstützt werden – können Sie die [Sql](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) Methode für die Ausführung von unformatierten SQL zum Erstellen/diese löschen.
