---
title: Überlegungen zur Leistung für EF4, EF5 und EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
caps.latest.revision: 4
ms.openlocfilehash: c01cf2b28e56fb73783bd9ed0d133bffa0a7dbe7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121524"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>Überlegungen zur Leistung für Entity Framework 4, 5 und 6
Von David Obando, Eric Dettinger usw.

Veröffentlichung: April 2012

Letzte Aktualisierung: Mai 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Einführung

Object-Relational Mapping-Frameworks sind eine einfache Möglichkeit, eine Abstraktion für den Datenzugriff in einer objektorientierten Anwendung bereitstellen. Für .NET-Anwendungen empfohlen Microsofts O/RM Entity Framework ist. Mit jeder Abstraktion kann die Leistung jedoch ist ein Problem auf.

In diesem Whitepaper wurde geschrieben, Überlegungen zur Leistung angezeigt werden, bei der Entwicklung von Anwendungen mithilfe von Entity Framework, um einen Überblick über die internen Algorithmen des Entity Framework Entwicklern, die Leistung beeinträchtigen können und Tipps zur Untersuchung angeben und Verbessern der Leistung ihrer Anwendungen, die Entity Framework verwenden. Es stehen eine Reihe von guten Themen auf die Leistung bereits im Web, und außerdem haben wir versucht, diese Ressourcen auf, wenn möglich.

Leistung ist ein kompliziertes Thema. Dieses Whitepaper dient als Ressource dazu, dass Sie die Leistung im Zusammenhang Entscheidungen für Ihre Anwendungen, die Entity Framework verwenden. Wir haben einige Testmetriken, um die Leistungsvorteile enthalten, aber diese Metriken sind nicht als absolute Indikatoren, die die Leistung, die Sie in Ihrer Anwendung angezeigt werden soll.

Aus praktischen Gründen wird in diesem Dokument davon ausgegangen, Entity Framework 4 ist unter .NET 4.0 und Entity Framework 5 und 6 unter .NET 4.5 ausgeführt werden. Viele der leistungsverbesserungen für Entity Framework 5 befinden sich innerhalb der Kernkomponenten, die mit .NET 4.5 enthalten sind.

Entitätsframework 6 ist ein Out-of-Band-Version und nicht von den Entity Framework-Komponenten, die mit .NET ausgeliefert abhängig. Entitätsframework 6 funktionieren auf .NET 4.0 und .NET 4.5, und eine großen Leistungssteigerung für diejenigen, die noch nicht von .NET 4.0 aktualisiert wurden, aber möchten die neuesten Entity Framework-Komponenten in ihrer Anwendung bieten. In diesem Dokument Entity Framework 6 erwähnt, verweist auf die neueste Version, die zum Zeitpunkt der Erstellung dieses Dokuments: Version 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. Kalte im Vergleich zu Warme Abfrageausführung

Beim allerersten vorgenommenen jede Abfrage für ein bestimmtes Modell, führt das Entity Framework viel Arbeit im Hintergrund geladen und überprüft das Modell. Wir bezeichnen häufig auf die erste Abfrage als Abfrage "kalt".  Weitere Abfragen für ein bereits geladenen Modell werden als "warmen" Abfragen bezeichnet, und Sie sind wesentlich schneller.

Lassen Sie uns einen allgemeinen Überblick darüber, wo Zeit verbracht wird, beim Ausführen einer Abfrage mithilfe von Entity Framework, und erfahren Sie, wo die Dinge in Entity Framework 6 verbessert wird.

**Erste Ausführung der Abfrage – kalte Abfrage**

| Die Benutzer schreibt Code                                                                                     | Aktion                    | EF4 Auswirkungen auf die Leistung                                                                                                                                                                                                                                                                                                                                                                                                        | EF5 Auswirkungen auf die Leistung                                                                                                                                                                                                                                                                                                                                                                                                                                                    | EF6 Auswirkungen auf die Leistung                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Beim Erstellen des Serverkontexts          | Mittel                                                                                                                                                                                                                                                                                                                                                                                                                        | Mittel                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Ausdruck abfragenerstellung | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                           | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | LINQ-Abfragen      | -Metadaten geladen: hohe aber zwischengespeicherten <br/> – Anzeigen von Generation: potenziell sehr hohe aber zwischengespeicherten <br/> -Parameter Evaluierung: Mittel <br/> -Abfragen Übersetzung: Mittel <br/> -Materializer Generierung: mittlere aber zwischengespeicherten <br/> -Database abfrageausführung: möglicherweise hohen <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> -Objekt Materialisierung: Mittel <br/> -Identity-Suche: Mittel | -Metadaten geladen: hohe aber zwischengespeicherten <br/> – Anzeigen von Generation: potenziell sehr hohe aber zwischengespeicherten <br/> -Parameter Evaluierung: Low <br/> -Abfragen Übersetzung: mittlere aber zwischengespeicherten <br/> -Materializer Generierung: mittlere aber zwischengespeicherten <br/> -Database abfrageausführung: möglicherweise hohen (Abfragen in einigen Situationen besser) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> -Objekt Materialisierung: Mittel <br/> -Identity-Suche: Mittel | -Metadaten geladen: hohe aber zwischengespeicherten <br/> – Anzeigen von Generation: mittlere aber zwischengespeicherten <br/> -Parameter Evaluierung: Low <br/> -Abfragen Übersetzung: mittlere aber zwischengespeicherten <br/> -Materializer Generierung: mittlere aber zwischengespeicherten <br/> -Database abfrageausführung: möglicherweise hohen (Abfragen in einigen Situationen besser) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> -Objekt Materialisierung: Mittel (schneller als EF5) <br/> -Identity-Suche: Mittel |
| `}`                                                                                                  | Connection.Close durchführen          | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                           | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**Zweiten Abfrageausführung – betriebsbereiten Abfrage**

| Die Benutzer schreibt Code                                                                                     | Aktion                    | EF4 Auswirkungen auf die Leistung                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | EF5 Auswirkungen auf die Leistung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | EF6 Auswirkungen auf die Leistung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Beim Erstellen des Serverkontexts          | Mittel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Mittel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Ausdruck abfragenerstellung | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | LINQ-Abfragen      | -Metadaten ~~laden~~ Suche: ~~aber zwischengespeicherten hohe~~ niedrig <br/> – Anzeigen von ~~Generation~~ Suche: ~~potenziell sehr hohe aber zwischengespeicherten~~ niedrig <br/> -Parameter Evaluierung: Mittel <br/> -Abfragen von ~~Übersetzung~~ Suche: Mittel <br/> -Materializer ~~Generation~~ Suche: ~~Mittel, aber zwischengespeicherten~~ niedrig <br/> -Database abfrageausführung: möglicherweise hohen <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> -Objekt Materialisierung: Mittel <br/> -Identity-Suche: Mittel | -Metadaten ~~laden~~ Suche: ~~aber zwischengespeicherten hohe~~ niedrig <br/> – Anzeigen von ~~Generation~~ Suche: ~~potenziell sehr hohe aber zwischengespeicherten~~ niedrig <br/> -Parameter Evaluierung: Low <br/> -Abfragen ~~Übersetzung~~ Suche: ~~Mittel, aber zwischengespeicherten~~ niedrig <br/> -Materializer ~~Generation~~ Suche: ~~Mittel, aber zwischengespeicherten~~ niedrig <br/> -Database abfrageausführung: möglicherweise hohen (Abfragen in einigen Situationen besser) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> -Objekt Materialisierung: Mittel <br/> -Identity-Suche: Mittel | -Metadaten ~~laden~~ Suche: ~~aber zwischengespeicherten hohe~~ niedrig <br/> – Anzeigen von ~~Generation~~ Suche: ~~Mittel, aber zwischengespeicherten~~ niedrig <br/> -Parameter Evaluierung: Low <br/> -Abfragen ~~Übersetzung~~ Suche: ~~Mittel, aber zwischengespeicherten~~ niedrig <br/> -Materializer ~~Generation~~ Suche: ~~Mittel, aber zwischengespeicherten~~ niedrig <br/> -Database abfrageausführung: möglicherweise hohen (Abfragen in einigen Situationen besser) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> -Objekt Materialisierung: Mittel (schneller als EF5) <br/> -Identity-Suche: Mittel |
| `}`                                                                                                  | Connection.Close durchführen          | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Niedrig                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Es gibt mehrere Möglichkeiten zur Reduzierung der Leistungskosten von kalte und warme Abfragen, und wir werden sehen Sie sich diese im folgenden Abschnitt. Genauer gesagt betrachten wir die kostenreduzierung von Modell laden in kalte Abfragen mit vorab generierten Sichten, mit dem Leistung-Probleme, die während des Generieren von Sichten zu verringern. Bei betriebsbereiten Abfragen wird die Zwischenspeichern von Abfrageplänen, keine Abfragen zur änderungsnachverfolgung und andere abfrageausführungsoptionen behandelt.

### <a name="21-what-is-view-generation"></a>2.1 Was Generieren von Sichten ist?

Um zu verstehen, welche Ansicht ist Generation, wir müssen zunächst verstehen, was "Zuordnen von Ansichten" sind. Zuordnung von Ansichten sind ausführbare Darstellungen der Transformationen in der Zuordnung für jede Entitätenmenge und der Zuordnung angegeben. Diese Zuordnung Ansichten intern die Form des CQTs (kanonische Abfrage Strukturen) in Anspruch nehmen. Es gibt zwei Arten von Ansichten der Zuordnung an:

-   Abfrageansichten: Diese stellen die Transformation, die notwendig, wechseln aus dem Datenbankschema dem konzeptionellen Modell dar.
-   Sichten zu aktualisieren: Diese repräsentieren die Transformation erforderlich, um das Datenbankschema aus dem konzeptionellen Modell zu wechseln.

Bedenken Sie, die das konzeptionelle Modell aus dem Datenbankschema auf verschiedene Weise unterscheiden. Beispielsweise kann eine einzelne Tabelle verwendet werden, zum Speichern der Daten für zwei verschiedene Entitätstypen verwendet. Vererbung und nicht trivialen Zuordnungen spielen eine Rolle, die Komplexität der Zuordnung Ansichten.

Der Prozess der Datenverarbeitung diese Sichten auf Grundlage der Spezifikation der Zuordnung ist, Generieren von Sichten nennen wir. Generieren von Sichten kann entweder dynamisch stattfinden, wenn ein Modell geladen wird, oder zur Buildzeit mithilfe von "vorab generierten Sichten"; Letztere werden serialisiert, in Form von Entity SQL-Anweisungen für eine C-\# oder VB-Datei.

Wenn Ansichten generiert werden, werden diese ebenfalls überprüft. Vom Standpunkt der Leistung ist die große Mehrheit der Kosten der Generieren von Sichten tatsächlich für die Überprüfung der Ansichten wird sichergestellt, dass die Verbindungen zwischen den Entitäten sinnvoll sind und die korrekte Kardinalität für alle unterstützten Vorgänge haben.

Wenn eine Abfrage über eine Entitätenmenge ausgeführt wird, wird die Abfrage mit entsprechenden Ansicht "Abfrage" kombiniert, und das Ergebnis dieser Zusammensetzung wird ausgeführt, durch den Plan-Compiler die Darstellung der Abfrage zu erstellen, die der Sicherungsspeicher verstehen kann. Für SQL Server werden das endgültige Ergebnis dieser Kompilierung einer T-SQL SELECT-Anweisung. Das erste Mal, das ein Update über eine Entitätenmenge ausgeführt wird, das wird der Updateansicht über einen ähnlichen Prozess die Transformation in DML-Anweisungen für die Zieldatenbank ausgeführt werden.

### <a name="22-factors-that-affect-view-generation-performance"></a>2.2-Faktoren, die Leistungseinbußen führen Generieren von Sichten

Die Leistung der Ansicht Generation Schritt hängt nicht nur auf die Größe des Modells, sondern auch dazu, wie verbundene des Modells ist. Wenn zwei Entitäten mit einer Vererbungskette oder eine Zuordnung verbunden sind, werden sie als verbunden. Auf ähnliche Weise werden zwei Tabellen über einen Fremdschlüssel verbunden sind, sie verbunden. Erhöhen Sie die Anzahl von verbundenen Entitäten und Tabellen in Ihren Schemas, Kosten für die Erstellung erhöht.

Der Algorithmus, den zum Generieren und überprüfen die Ansichten verwenden wir ist im ungünstigsten Fall exponentiellen, obwohl wir einige Optimierungen verwenden, um dies zu verbessern. Die wichtigsten Faktoren, die die Leistung negativ beeinflussen scheinen sind:

-   Modell Größe auf die Anzahl der Entitäten und die Menge der Zuordnungen zwischen diesen Entitäten verweisen.
-   Modell die Komplexität, insbesondere eine große Anzahl von Typen mit Vererbung.
-   Anstelle von unabhängigen Zuordnungen Foreign Key-Zuordnungen.

Für kleine, einfache Modelle möglicherweise die Kosten klein genug ist, zu nicht vorab generierte Sichten verwenden. Modellgröße und Komplexität zu erhöhen, gibt es mehrere Optionen zur Verfügung, die Kosten der Generieren von Sichten und Validierung.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>2.3 Pre-Generated Ansichten verwenden, Modell zu verringern, die Ladezeit

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools"></a>2.3.1 vorab generierten Sichten, die mit dem Entity Framework Power Tools

Sie können auch erwägen, mit dem Entity Framework Power Tools auf um Ansichten der EDMX-Datei und Code First-Modelle zu generieren, indem Sie mit der rechten Maustaste der Modell-Klassendatei, und mithilfe des Entity Framework-Menüs "Ansichten generieren" auswählen. Die Entity Framework Power Tools können nur für "DbContext" abgeleitet von Kontexten und finden Sie unter \< http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d>.

Weitere Informationen zur Verwendung von vorab generierten Sichten zu Entity Framework 6 finden Sie unter [Pre-Generated Zuordnen von Ansichten](~/ef6/fundamentals/performance/pre-generated-views.md).

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2, wie Sie mit vorab generierten Sichten mit einem Modell EDMGen erstellt

EDMGen ist ein Hilfsprogramm, das im Lieferumfang von .NET und funktioniert mit Entity Framework 4 und 5, jedoch nicht mit Entity Framework 6. EDMGen können Sie eine Modelldatei, der Objektebene und die Ansichten über die Befehlszeile zu generieren. Eine der Ausgaben werden eine Ansichten-Datei in der Sprache Ihrer Wahl, VB oder C#\#. Dies ist eine Codedatei mit Entity SQL-Codeausschnitte für jede Entitätenmenge. Um Sichten zu aktivieren, fügen Sie einfach die Datei in Ihrem Projekt.

Wenn Sie die Schemadateien für das Modell manuell Änderungen vornehmen, müssen Sie die Ansichten-Datei erneut zu generieren. Hierzu können Sie EDMGen mit Ausführen der **/mode:ViewGeneration** Flag.

Weitere finden Sie unter [wie: Verbessern der Abfrageleistung Pre-Generate Ansichten](https://msdn.microsoft.com/library/bb896240.aspx).

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 Verwendung Pre-Generated Ansichten mit einer EDMX-Datei

Sie können auch EDMGen verwenden, um Ansichten für eine EDMX-Datei zu generieren: im zuvor erwähnten MSDN-Thema wird beschrieben, wie eine Präbuildereignis - dazu hinzufügen, aber dies ist kompliziert und es gibt einige Fälle, in denen es nicht möglich ist. Es ist im Allgemeinen leichter zu eine T4-Vorlage zu verwenden, um die Ansichten zu generieren, wenn Ihr Modell in einer Edmx-Datei ist.

ADO.NET-Teamblog hat einen Beitrag, der beschreibt, wie Sie mit einer T4-Vorlage für das Generieren von Sichten ( \< http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Dieser Beitrag enthält eine Vorlage, die heruntergeladen und dem Projekt hinzugefügt werden kann. Die Vorlage wurde für die erste Version von Entity Framework geschrieben, sodass sie garantiert sind nicht mit den neuesten Versionen von Entity Framework arbeiten. Allerdings können Sie einem aktuelleren Satz von Generation Ansichtsvorlagen für Entity Framework 4 und 5from der Visual Studio Gallery herunterladen:

-   VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Bei Verwendung von Entity Framework 6 erhalten Sie die Ansicht Generation T4-Vorlagen aus Visual Studio Gallery unter \< http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

#### <a name="234-how-to-use-pre-generated-views-with-a-code-first-model"></a>2.3.4 wie Pre-Generated Ansichten mit einem Code First-Modell verwenden

Es ist auch möglich, die vorgenerierte Sichten mit einem Code First-Projekt verwenden. Die Entity Framework Power Tools verfügt über die Möglichkeit, eine Datei Ansichten für das Code First-Projekt zu generieren. Sie finden die Entity Framework Power Tools in Visual Studio Gallery unter \< http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d/>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2.4 Senkung der Kosten der Generieren von Sichten

Verschiebt vorab generierte Sichten verwenden die Kosten für das Generieren von Sichten Modell geladen (Laufzeit) in der Kompilierung. Während dies verbessert die Leistung beim Start zur Laufzeit, dennoch treten die Probleme beim Generieren von Sichten während der Entwicklung. Es gibt einige zusätzliche Tricks, die die Kosten für das Generieren von Sichten, sowohl zur Kompilierzeit und Laufzeit verringern können.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 mithilfe anzeigen Generation von Kosten reduzieren von Foreign Key-Zuordnungen

Wir haben gesehen, eine Reihe von Fällen, in denen wechseln von Zuordnungen in das Modell von unabhängigen Zuordnungen auf Zuordnungen von Foreign Key drastisch die Zeit im Generieren von Sichten verbessert.

Um diese Verbesserung zu demonstrieren, haben wir zwei Versionen des Modells Navision mithilfe von EDMGen generiert. *Hinweis: Seeappendix Cfor eine Beschreibung des Modells Navision.* Das Modell Navision ist für diese Übung, aufgrund dessen große Menge von Entitäten und Beziehungen zwischen ihnen interessant.

Eine Version dieses Modells sehr große mit Zuordnungen von Foreign-Schlüssel generiert wurde, und der andere mit unabhängigen Zuordnungen generiert wurde. Klicken Sie dann Timeout wir bei, wie lang es gedauert, um die Ansichten für jedes Modell zu generieren. Entität Framework5-Test verwendet die GenerateViews()-Methode aus der Klasse EntityViewGenerator, um den Ansichten zu generieren, während der Entity Framework 6-Test die GenerateViews()-Methode aus der Klasse StorageMappingItemCollection verwendet. Dies ist aufgrund von Code umstrukturieren, die in der Entity Framework 6-Codebasis aufgetreten sind.

Mithilfe von Entity Framework 5, dauerte Generieren von Sichten für das Modell mit Fremdschlüsseln 65 Minuten, in einer Lab-Computer. Es unbekannten wie lange erforderlich gewesen wäre, um die Ansichten für das Modell zu generieren, die unabhängige Zuordnungen verwendet. Wir bleiben den Test ausführen, die für mehr als einem Monat, bevor der Computer neu gestartet wurde, in unserer testumgebung monatlichen Updates zu installieren.

Verwendung von Entity Framework 6, dauerte Generieren von Sichten für das Modell mit Fremdschlüsseln, 28 Sekunden, in dem gleichen Lab-Computer. Generieren von Sichten für das Modell, das unabhängigen Zuordnungen verwendet hat 58 Sekunden gedauert. Die Verbesserungen, die auf Entity Framework 6 auf seine Ansicht generierungscodes bedeuten, dass es sich bei vielen Projekten nicht vorab generierte Sichten, um schnellere Startzeiten zu erhalten benötigen.

Es ist wichtig, die Anmerkung, die mit EDMGen oder die Entity Framework Power Tools vorgenerieren von Ansichten im Entity Framework 4 und 5 ausgeführt werden können. Für Entity Framework 6-Ansicht Generation kann erfolgen über die Entity Framework Power Tools oder programmgesteuert, siehe [Pre-Generated Zuordnen von Ansichten](~/ef6/fundamentals/performance/pre-generated-views.md).

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1, wie Sie mit Fremdschlüsseln anstelle von unabhängigen Zuordnungen

Wenn EDMGen oder der Entity Designer in Visual Studio verwenden, erhalten Sie ein Fremdschlüssel in der Standardeinstellung, und es dauert nur einen einzigen Kontrollkästchen oder über die Befehlszeile Flag zum Wechseln zwischen Fremdschlüssel und IAs.

Wenn Sie ein großes Code First-Modell verfügen, müssen die unabhängigen Zuordnungen mit, dass die gleiche Wirkung auf das Generieren von Sichten. Sie diese Auswirkungen zu vermeiden, einschließlich der Foreign Key-Eigenschaften für die Klassen für die abhängige Objekte, obwohl manche Entwickler diese Option, um ihr Objektmodell zu stören, werden berücksichtigt werden. Sie finden weitere Informationen zu diesem Thema im \< http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| Bei Verwendung      | Vorgehensweise                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Entity Designer | Nach dem Hinzufügen einer Zuordnung zwischen zwei Entitäten, sicher, dass eine referenzielle Einschränkung besitzt. Referenzielle Einschränkungen Teilen Entity Framework zum Fremdschlüssel anstatt unabhängige Zuordnungen verwenden. Weitere Informationen finden Sie unter \< http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | Wenn EDMGen verwenden, um Ihre Dateien aus der Datenbank zu generieren, wird Ihre Fremdschlüssel berücksichtigt und das Modell daher hinzugefügt werden. Weitere Informationen zu den verschiedenen Optionen, die verfügbar gemacht werden, indem EDMGen finden Sie [ http://msdn.microsoft.com/library/bb387165.aspx ](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| Code First      | Finden Sie im Abschnitt "Beziehung Konvention" die [Code First-Konventionen](~/ef6/modeling/code-first/conventions/built-in.md) Thema enthält Informationen zum Fremdschlüsseleigenschaften für abhängige Objekte einschließen, wenn Sie Code First verwenden.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 verschieben Ihr Modell in eine separate assembly

Wenn Ihr Modell direkt in Ihrer Anwendung Projekt enthalten ist, und Sie die Sichten, die über ein Ereignis vor dem Erstellen oder eine T4-Vorlage generieren, Generieren von Sichten und Überprüfung erfolgt wird, wenn das Projekt neu erstellt wird, selbst wenn das Modell geändert wurde. Wenn Sie das Modell in eine separate Assembly verschieben und verweisen sie aus dem Projekt Ihrer Anwendung hinzu, können Sie andere Änderungen für Ihre Anwendung vornehmen, ohne das Projekt mit dem Modell neu erstellen.

*Hinweis:* beim Verschieben von Modell aus, um separate Assemblys Denken Sie daran, die Verbindungszeichenfolgen für das Modell in der Anwendungskonfigurationsdatei des Clientprojekts zu kopieren.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 Deaktivieren der Validierung des eine Edmx-basierten Modells

EDMX-Modelle werden zum Zeitpunkt der Kompilierung überprüft, auch wenn das Modell unverändert ist. Wenn Ihr Modell bereits überprüft wurde, können Sie die Überprüfung zur Kompilierzeit unterdrücken, durch die Eigenschaft "Auf Build überprüfen" auf "false" festlegen, im Eigenschaftenfenster. Wenn Sie Ihre Zuordnung oder das Modell ändern, können Sie vorübergehend Überprüfung, um zu überprüfen, ob die Änderungen erneut aktivieren.

Beachten Sie, dass die wurden leistungsverbesserungen vorgenommen, die Entity Framework Designer für Entity Framework 6, und die Kosten für die "Überprüfen für Build" ist wesentlich geringer als in früheren Versionen des Designers.

## <a name="3-caching-in-the-entity-framework"></a>3-Zwischenspeicherung in Entitätsframework

Entitätsframework gibt es die folgenden Formen Zwischenspeichern integrierte:

1.  Objekt Zwischenspeichern – verfolgt das ObjectStateManager integriert eine ObjectContext-Instanz im Arbeitsspeicher, der die Objekte, die mit dieser Instanz abgerufen wurden. Dies ist auch bekannt als E1-Cache.
2.  Abfrage-Plan Caching - generierten Speicherbefehl wiederverwenden, wenn eine Abfrage mehr als einmal ausgeführt wird.
3.  Metadaten, die caching - die Metadaten für ein Modell für andere Verbindungen mit dem gleichen Modell freigegeben.

Neben der Caches, die EF standardmäßig eine besondere Art von ADO.NET-Datenanbieter, nämlich ein Wrapping-Anbieter auch verwendet werden kann, zum Erweitern von Entity Framework mit einem Cache für die aus der Datenbank abgerufenen Ergebnisse enthält auch bekannt als Zwischenspeicherung auf zweiter Ebene.

### <a name="31-object-caching"></a>3.1 Objekt Zwischenspeichern

Standardmäßig beim Zurückgeben einer Entität in den Ergebnissen einer Abfrage, überprüft kurz bevor EF, materialisiert Sie ObjectContext, ob eine Entität mit dem gleichen Schlüssel bereits in der ObjectStateManager geladen wurde. Wenn eine Entität mit dem gleichen Schlüssel bereits vorhanden ist wird EF diese in den Ergebnissen der Abfrage enthalten. Obwohl EF die Abfrage für die Datenbank weiterhin ausstellt, kann ein Großteil der Kosten für die Materialisierung der Entitäts mehrmals dieses Verhalten umgangen werden.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 Abrufen von Entitäten aus der Objektcache mit "DbContext" Suchen

Im Gegensatz zu einer regelmäßigen Abfrage wird die Find-Methode in "DbSet" (APIs, die zum ersten Mal in EF 4.1 enthalten) eine Suche im Arbeitsspeicher auszuführen, bevor Sie auch die Ausgabe der Abfrage für die Datenbank. Es ist wichtig zu beachten, dass zwei verschiedene ObjectContext-Instanzen zwei verschiedene ObjectStateManager-Instanzen hat, was bedeutet, dass sie separate Objektcaches aufweisen.

Suchen wird mit dem Wert des Primärschlüssels finden Sie eine Entität, die vom Kontext nachverfolgt versucht. Ist die Entität nicht im Kontext eine Abfrage wird ausgeführt und werden in der Datenbank ausgewertet, und Null wird zurückgegeben, wenn die Entität nicht im Kontext oder in der Datenbank gefunden wird. Beachten Sie, dass suchen auch Entitäten zurückgibt, die dem Kontext hinzugefügt wurden, aber noch nicht mit der Datenbank gespeichert wurde.

Es gibt eine leistungsoptimierung mit der Suche verwendet werden. Aufrufe dieser Methode werden standardmäßig werden eine Überprüfung des Objektcaches ausgelöst, um die Änderungen zu erkennen, die Commit an die Datenbank immer noch aussteht. Dieser Prozess kann sehr teuer sein, wenn es gibt eine sehr große Anzahl von Objekten im Objektcache oder in einem LOB-Diagramm hinzugefügte Objektcache sein, aber sie kann auch deaktiviert werden. In bestimmten Fällen können Sie wahrnehmen über eine Größenordnung des Unterschieds bei Aufrufen der Methode, wenn das Deaktivieren der automatischen Erkennung finden Änderungen. Ein zweites bedeutend wird noch angesehen, wenn das Objekt tatsächlich in den Cache ist, wenn das Objekt wurde aus der Datenbank abgerufen werden sollen. Hier ist ein Beispieldiagramm mit Messungen, die mit einigen unserer Microbenchmarks, ausgedrückt in Millisekunden, die bei einer Last von 5000 Entitäten:

![Net45LogScale](~/ef6/media/net45logscale.png ".NET 4.5 - logarithmische Skalierung")

Beispiel für Suchen zur automatischen Erkennung um Änderungen deaktiviert:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

Was man berücksichtigen die Find-Methode zu verwenden ist:

1.  Wenn das Objekt nicht im Cache ist die Vorteile der Suchen negiert werden, aber die Syntax ist noch einfacher, als eine Abfrage nach Schlüssel.
2.  Wenn die automatische Änderungen Erkennung ist aktiviert. die Kosten für die Find-Methode kann durch eine Zehnerpotenz ausgestochen oder sogar noch abhängig von der Komplexität des Modells und die Menge von Entitäten im Objektcache zu erhöhen.

Darüber hinaus denken Sie daran, die nur finden, gibt die Entität, die Sie suchen und zwar nicht automatisch laden die zugeordneten Entitäten, wenn sie nicht bereits im Objektcache vorhanden sind. Wenn Sie zugeordnete Entitäten abrufen möchten, können Sie eine Abfrage nach Schlüssel mit unverzüglichem Laden. Weitere Informationen finden Sie unter **8.1 Lazy Loading Visual Studio. Eager Loading**.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>3.1.2 Leistungsprobleme, wenn der Objektcache über viele Entitäten verfügt

Objektcache hilft, die um die allgemeine Reaktionsfähigkeit der Entity Framework zu erhöhen. Allerdings wird Wenn Objektcache hat eine große Menge von Entitäten geladen, es bestimmte Vorgänge wie hinzufügen auswirkt, entfernen Sie, suchen Sie, Eintrag, "SaveChanges" und vieles mehr. Insbesondere werden aufgelistet, die einen Aufruf von DetectChanges Auslösen von sehr großes Objektcaches beeinträchtigt werden. DetectChanges synchronisiert das Objektdiagramm mit dem Objekt-Status-Manager und seine Leistung wird direkt von der Größe des Objektdiagramms bestimmt. Weitere Informationen zu DetectChanges, finden Sie unter [Nachverfolgen von Änderungen in POCO-Entitäten](https://msdn.microsoft.com/library/dd456848.aspx).

Wenn Sie Entity Framework 6 verwenden zu können, sind Entwickler "AddRange" und "RemoveRange" direkt auf ein "DbSet" anstatt auf eine Auflistung durchlaufen und Aufrufs von Add einmal pro Instanz aufrufen können. Der Vorteil der Verwendung der "Range"-Methoden ist, dass die Kosten für die DetectChanges nur einmal für den gesamten Satz von Entitäten im Gegensatz zu einmal pro jedes hinzugefügte Entität gezahlt wird.

### <a name="32-query-plan-caching"></a>3.2 Fragen Sie Zwischenspeichern von Abfrageplänen ab

Beim ersten, die eine Abfrage ausgeführt wird, durchläuft er der internen Plan Compiler übersetzt die grundlegende Abfrage in den Store-Befehl (z. B. die T-SQL die ausgeführt wird, wenn Sie für SQL Server ausgeführt).  Wenn Zwischenspeichern von Abfrageplänen aktiviert ist, wird der nächsten Ausführung die Abfrage wird ausgeführt im Store Befehl direkt aus dem Abfrageplancache zur Ausführung unter Umgehung des Plan Compilers abgerufen wird.

Abfrageplancache wird ObjectContext-Instanzen innerhalb derselben Anwendungsdomäne gemeinsam genutzt. Sie müssen nicht auf eine ObjectContext-Instanz zum Zwischenspeichern von Abfrageplänen profitieren enthalten.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 einige Hinweise zum Planen der Abfrage Zwischenspeichern.

-   Abfrageplancache wird freigegeben, für alle Typen abgefragt: Entity SQL, LINQ to Entities und CompiledQuery-Objekte.
-   Zwischenspeichern von Abfrageplänen ist für Entity SQL-Abfragen, in der Standardeinstellung aktiviert, ob durch eine von "EntityCommand" oder durch eine ObjectQuery ausgeführt. Es ist auch standardmäßig für LINQ to Entities-Abfragen in Entity Framework auf .NET 4.5 und Entity Framework 6 aktiviert
    -   Zwischenspeichern von Abfrageplänen kann deaktiviert werden, durch die EnablePlanCaching-Eigenschaft (auf von "EntityCommand" oder ObjectQuery) auf "false" festlegen. Zum Beispiel:
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   Für parametrisierte Abfragen erreichen ändern den Wert des Parameters dennoch die zwischengespeicherte Abfrage. Aber eine parameterfacets (z. B. Größe, Genauigkeit oder Skala) ändern, einen anderen Eintrag im Cache ist erreicht.
-   Verwendung von Entity SQL ist die Abfragezeichenfolge Teil des Schlüssels. Ändern die Abfrage überhaupt führt zu mehrere Cacheeinträge, auch wenn die Abfragen funktionell gleichwertig sind. Dies schließt Änderungen, die Groß-/Kleinschreibung oder ein Leerzeichen.
-   Wenn Sie LINQ verwenden, wird die Abfrage verarbeitet, um einen Teil des Schlüssels zu generieren. Ändern des LINQ-Ausdrucks wird daher einen anderen Schlüssel generieren.
-   Weitere technische Einschränkungen können anfallen. Weitere Informationen finden Sie in der Autocompiled Abfragen.

#### <a name="322------cache-eviction-algorithm"></a>3.2.2 Cache Entfernung-Algorithmus

Verstehen, wie die internen Algorithmus funktioniert Sie zum Aktivieren oder deaktivieren Zwischenspeichern von Abfrageplänen herausfinden können. Der Cleanup-Algorithmus sieht folgendermaßen aus:

1.  Sobald der Cache über eine festgelegte Anzahl von Einträgen (800) enthält, beginnen wir mit einen Timer an, der in regelmäßigen Abständen (einmal pro Minute) des Caches führt ein Sweep.
2.  Während der Cache-Sweep-Einträge werden entfernt, aus dem Cache eine LFRU (Least häufig – zuletzt verwendete) Basis. Dieser Algorithmus berücksichtigt Trefferanzahl und der ALTER bei der Entscheidung, welche Einträge ausgeworfen werden.
3.  Am Ende der einzelnen Cache löschen enthält der Cache erneut 800 Einträge.

Alle Cacheeinträge werden gleich behandelt werden, beim Ermitteln der Einträge zu entfernen. Dies bedeutet, dass der Speicherbefehl für eine CompiledQuery gleichen Wahrscheinlichkeit Entfernung wie der Speicherbefehl für eine Entity SQL-Abfrage.

Beachten Sie, dass der Cache Entfernung Timer gestartet ist, 800 Entitäten vorhanden sind, im Cache, aber der Cache ist nur eine Komprimierung 60 Sekunden, nachdem dieser Timer gestartet wurde. Das bedeutet, dass für bis zu 60 Sekunden Ihres Caches wachsen kann, um sehr groß sein.

#### <a name="323-------test-metrics-demonstrating-query-plan-caching-performance"></a>3.2.3 Testmetriken Sie Demonstration Leistung Zwischenspeichern von Abfrageplänen

Um die Auswirkungen auf die Leistung Ihrer Anwendung Zwischenspeichern von Abfrageplänen zu demonstrieren, ausgeführt, wir einen Test, in dem wir eine Anzahl von Entity SQL-Abfragen für das Microsoft Navision-Modell ausgeführt haben. Finden Sie im Anhang eine Beschreibung des Modells Navision und die Typen von Abfragen, die ausgeführt wurden. In diesem Test wir zunächst die Liste der Abfragen durchlaufen und jeweils einmal ausgeführt, um sie mit dem Cache hinzuzufügen (sofern es sich um eine Zwischenspeicherung aktiviert ist). Dieser Schritt ist untimed. Als Nächstes Standbymodus wir den Hauptthread für mehr als 60 Sekunden zum Cache sweeping zum durchgeführt werden; zum Schluss durchlaufen wir die Liste eine 2. Ausführungsdauer die zwischengespeicherten Abfragen. Darüber hinaus ist er SQL Server-Plancache geleert, bevor jede Gruppe von Abfragen ausgeführt wird, so, dass die Häufigkeit, mit die wir genau erhalten den Vorteil, dass vom Abfrageplancache angezeigt.

##### <a name="3231-------test-results"></a>3.2.3.1 "Testergebnisse"

| Test                                                                   | EF5 kein Cache | EF5 zwischengespeichert | EF6 kein Cache | EF6 zwischengespeichert |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| Auflisten aller 18723 Abfragen                                          | 124          | 125.4      | 124,3        | 125.3      |
| Vermeiden Sweep (nur die ersten 800 Abfragen, unabhängig von der Komplexität)  | 41.7         | 5.5        | 40,5         | 5.4        |
| Nur die AggregatingSubtotals Abfragen (178 insgesamt – wodurch Sweep vermieden werden) | 39,5         | 4.5        | 38.1         | 4.6        |

*Alle Uhrzeiten in Sekunden.*

Moralische - beim Ausführen von losen der unterschiedlichen Abfragen (z. B. Abfragen dynamisch erstellt), caching nicht unterstützen, und der resultierende Leerung des Cache können Sie die Abfragen, die profitieren, dass die meisten Zwischenspeichern von Abfrageplänen aus tatsächlich beibehalten.

Die AggregatingSubtotals-Abfragen sind sehr komplexe Abfragen, die, denen wir mit getestet. Wie erwartet desto komplexer ist die Abfrage, den weitere Vorteil von Zwischenspeichern von Abfrageplänen wird angezeigt.

Da eine CompiledQuery wirklich eine LINQ-Abfrage mit einem Plan zwischengespeichert ist, sollte der Vergleich eine CompiledQuery im Vergleich zu den entsprechenden Entity SQL-Abfrage ähnliche Ergebnisse haben. In der Tat verfügt eine app viele dynamische Entity SQL-Abfragen, bewirkt füllen den Cache mit Abfragen auch CompiledQueries "dekompiliert", wenn sie aus dem Cache geleert werden. In diesem Szenario kann die Leistung verbessert werden durch das Deaktivieren der Zwischenspeicherung für die dynamische Abfragen, die CompiledQueries zu priorisieren. Besser noch, natürlich wäre, Schreiben Sie die app, um parametrisierte Abfragen anstelle von dynamischen Abfragen verwenden.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3.3 Verwendung von CompiledQuery zur Verbesserung der Leistung mit LINQ-Abfragen

Unsere Tests anzugeben, dass mit der CompiledQuery ein Vorteil von 7 % über Autocompiled LINQ-Abfragen bieten kann; Das heißt, Sie 7 % weniger Zeit für die Ausführung von Code aus dem Entity Framework-Stapel beschäftigen. Es bedeutet nicht, dass Ihre Anwendung 7 % schneller ist. Im Allgemeinen kann die Kosten für das Schreiben und pflegen von CompiledQuery-Objekte in EF 5.0 sollte das Problem im Vergleich zu den Vorteilen nicht. Ihr Bedarf kann variieren, Übung also diese Option aus, wenn das Projekt, den zusätzliche Push erfordert. Beachten Sie, dass CompiledQueries nur kompatibel mit ObjectContext abgeleiteten-Modellen und nicht kompatibel mit Modellen "DbContext" abgeleitet sind.

Weitere Informationen zum Erstellen und Aufrufen einer CompiledQuery finden Sie unter [kompilierte Abfragen (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

Es gibt zwei Überlegungen haben Sie an, die beim Verwenden einer CompiledQuery, nämlich die Anforderung, verwenden Sie statische Instanzen und die Probleme, dass sie zusammensetzbarkeit haben. Hier folgt eine ausführliche Erläuterung dieser beiden Aspekte.

#### <a name="331-------use-static-compiledquery-instances"></a>3.3.1 verwenden Sie statische CompiledQuery-Instanzen

Kompilieren eine LINQ-Abfrage ein sehr zeitaufwendiger Prozess daher möchten wir nicht dafür, jedem müssen wir zum Abrufen von Daten aus der Datenbank. CompiledQuery-Instanzen ermöglichen Ihnen einmal kompilieren und führen mehrere Male, aber Sie vorsichtig sein müssen, und beschaffen, um dieselbe Instanz CompiledQuery jedes Mal statt kompilieren immer wieder erneut zu verwenden. Die Verwendung der statischen Member zum Speichern der CompiledQuery-Instanzen ist erforderlich; Andernfalls wird keine Vorteile nicht angezeigt.

Nehmen wir beispielsweise an, dass die Seite den folgenden Methodentext, behandeln die Anzeige von Produkten für die ausgewählte Kategorie verfügt:

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

In diesem Fall erstellen Sie eine neue Instanz der CompiledQuery im laufenden Betrieb jedes Mal, wenn die Methode aufgerufen wird. Statt die Leistungsvorteile durch Aufrufen der entsprechenden Speicherbefehl Abfrageplancache befinden, wird der Plan-Compiler der CompiledQuery durchlaufen, jedes Mal eine neue Instanz erstellt wird. In der Tat werden Sie Entwurfsausdruck werden Ihrem Abfrageplancache mit einem neuen CompiledQuery-Eintrag jedes Mal, wenn die Methode aufgerufen wird.

Sie möchten stattdessen erstellen eine statische Instanz der kompilierten Abfrage aus, damit Sie die gleiche kompilierte Abfrage aufrufen, jedes Mal, wenn die Methode aufgerufen wird. Eine Möglichkeit, durch das Hinzufügen der CompiledQuery-Instanz als ein Mitglied der Objektkontext ist.  Sie können Aufgaben klicken Sie dann eine wenig klarer vornehmen, durch den Zugriff auf die CompiledQuery über eine Hilfsmethode:

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

Diese Hilfsmethode würde wie folgt aufgerufen werden:

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-------composing-over-a-compiledquery"></a>3.3.2 zusammenstellen über eine CompiledQuery

Die Möglichkeit, die für alle LINQ-Abfrage verfasst ist äußerst nützlich. zu diesem Zweck einfach Methode aufgerufen, um eine nach der das IQueryable-Objekt wie z. B. *Skip()"* oder *Count()*. Jedoch im Grunde genommen also tun, wird ein neues "IQueryable"-Objekt zurückgegeben. Zwar gibt es nichts zu technisch zusammenstellen, die über eine CompiledQuery verhindern, erfordert der Generierung eines neuen "IQueryable"-Objekts, das bewirkt dies übergeben, durch den Compiler Plan erneut an.

Einige Komponenten werden Nutzen aus "IQueryable"-Objekten, die erweiterte Funktionalität zu aktivieren. Beispiel: ASP. NET GridView kann Daten an ein IQueryable-Objekt über die SelectMethod-Eigenschaft gebunden werden. Das GridView wird dann für diese "IQueryable"-Objekt zu sortieren und paging für das Datenmodell verfasst. Wie Sie sehen können, verwenden eine CompiledQuery für GridView würde nicht erreicht, der die kompilierte Abfrage jedoch erzeugt eine neue Autocompiled-Abfrage.

Die Customer Advisory Team erläutert dies in ihren Blogbeitrag "Mögliche Leistung Probleme mit kompilierte LINQ-Abfrage erneut kompiliert wird": <http://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/potential-performance-issues-with-compiled-linq-query-re-compiles.aspx>.

Einem zentralen Ort, in denen dies unter Umständen auftreten, ist beim progressiven Filter auf eine Abfrage hinzufügen. Nehmen wir beispielsweise an, dass Sie eine Kundenseite mit verschiedene Dropdownlisten für die optionalen Filtern (z. B. "Land" und "OrdersCount") konnten. Sie können diese Filter für die "IQueryable" Ergebnisse von einer CompiledQuery verfassen, aber dies wird in der neuen Abfrage durchlaufen des Plan-Compilers, jedes Mal, wenn Sie es ausführen, führen.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Um diese erneute Kompilierung zu vermeiden, können Sie umschreiben, dass der CompiledQuery, um die mögliche Filter zu berücksichtigen:

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Die in der Benutzeroberfläche wie aufgerufen werden sollen:

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Ein Nachteil hierbei ist generierten Speicherbefehl weisen stets die Filter mit der null-Überprüfungen, sollten Sie werden jedoch recht einfach, für den Datenbankserver zur Optimierung:

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>3.4-Metadaten-Zwischenspeicherung

Das Entity Framework unterstützt auch das Zwischenspeichern von Metadaten. Dies ist im Wesentlichen der Typinformationen und Zuordnungsinformationen und Typ der Datenbank über verschiedene Verbindungen demselben Modell zwischengespeichert. Metadatencache ist pro Anwendungsdomäne eindeutig.

#### <a name="341-metadata-caching-algorithm"></a>3.4.1 Metadaten-Caching-Algorithmus

1.  Informationen zu Metadaten für ein Modell wird in eine ItemCollection für die einzelnen EntityConnection-Objekt gespeichert.
    -   Nebenbei bemerkt gibt es verschiedene ItemCollection-Objekte für die verschiedenen Teile des Modells. StoreItemCollections enthält beispielsweise die Informationen über das Datenbankmodell; ObjectItemCollection zurück enthält Informationen über das Datenmodell. EdmItemCollection enthält Informationen über das konzeptionelle Modell.

2.  Wenn zwei Verbindungen die gleiche Verbindungszeichenfolge verwenden, werden sie die gleiche ItemCollection Instanz freigeben.
3.  Funktionell gleichwertig, aber textlich unterschiedliche Verbindungszeichenfolgen können dazu führen, dass andere Metadaten-Caches. Wir Verbindungszeichenfolgen, mit einem Token versehen einfach ändern der Reihenfolge der Token in den freigegebenen Metadaten führen soll. Jedoch zwei Verbindungszeichenfolgen, die funktionell identisch erscheinen möglicherweise nicht ausgewertet werden als identisch nach der Zerlegung in Token.
4.  Die ItemCollection wird in regelmäßigen Abständen für die Verwendung überprüft. Wenn festgestellt wird, dass ein Arbeitsbereich nicht zuletzt zugegriffen wurde, wird es für die Bereinigung auf der nächsten Cache Sweep gekennzeichnet.
5.  Allein durch das Erstellen einer EntityConnection-Objekt wird dazu führen, dass einem veralteten Metadatencache erstellt werden (obwohl die elementauflistungen darin nicht initialisiert werden, bis die Verbindung geöffnet wird). Dieser Arbeitsbereich bleibt im Arbeitsspeicher, bis die caching-Algorithmus feststellt, dass sie nicht "in Verwendung" ist.

Die Customer Advisory Team verfügt über einen Blogbeitrag, der beschreibt, enthält einen Verweis auf eine ItemCollection um "als veraltet" zu vermeiden, wenn Sie große Modelle verwenden geschrieben: \< http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 die Beziehung zwischen Metadaten und Abfragen Planen der Zwischenspeicherung

Die Abfrage-Plan-Cache-Instanz befindet sich in MetadataWorkspaces ItemCollection von Store-Typen. Dies bedeutet, dass zwischengespeicherte Speicherbefehle für Abfragen in einem Kontext mit einem bestimmten MetadataWorkspace instanziiert verwendet werden. Dies bedeutet auch, wenn Sie zwei Verbindungszeichenfolgen verwenden, die unterscheiden sich leicht aus, und nach der Tokenisierung stimmen nicht überein, Sie verschiedene Abfragen, die Cache-Instanzen zu planen müssen.

### <a name="35-results-caching"></a>3.5 Ergebnisse zwischenspeichern

Mit Ergebnissen Zwischenspeichern (auch bekannt als "Second-Level-caching") halten Sie die Ergebnisse von Abfragen in einem lokalen Cache. Wenn eine Abfrage ausgegeben wird, sehen Sie sich zunächst, wenn die Ergebnisse vor dem Sie eine Abfrage für den Speicher lokal verfügbar sind. Während der Ergebnisse, die Zwischenspeicherung direkt von Entity Framework unterstützt werden, ist es möglich, einen Cache zweiten Ebenen hinzufügen, indem Sie mithilfe eines Anbieters umschließen. Ein Beispiel-Wrapping-Anbieter mit einem Cache zweiter Ebene ist die Alachisoft [Cache zweiter Ebene in Entity Framework auf der Grundlage von NCache](http://www.alachisoft.com/ncache/entity-framework.html).

Diese Implementierung der Zwischenspeicherung auf zweiter Ebene ist eine eingefügte verfügbar, die direkt nach der LINQ-Ausdruck ausgewertet wurde (und Funcletized) und der Abfrageausführungsplan berechnet oder abgerufen, die aus dem Cache auf oberster Ebene. Cache zweiter Ebene werden dann nur die Ergebnisse des raw-Datenbank gespeichert, damit die Materialization-Pipeline danach immer noch ausgeführt.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 Weitere Verweise für Ergebnisse zwischenspeichern, wenn die Wrapping-Anbieter

-   Julie Lerman hat einen "Second-Level-Zwischenspeicherung in Entity Framework und Windows Azure" MSDN-Artikel geschrieben, der Vorgehensweise beim Aktualisieren des Wrapping-Beispielanbieters zum Verwenden von Windows Server AppFabric caching enthält: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Wenn Sie mit Entity Framework 5 arbeiten, hat das Teamblog einen Beitrag, der beschreibt, wie Sie die Dinge, die mit der caching-Anbieter für Entity Framework 5 ausgeführt werden: \< http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. Darüber hinaus eine T4-Vorlage zum Hinzufügen von der Ebene 2. caching zu Ihrem Projekt automatisieren.

## <a name="4-autocompiled-queries"></a>4 Autocompiled Abfragen

Wenn eine Abfrage für eine Datenbank mit Entity Framework ausgegeben wird, muss es eine Reihe von Schritten durchlaufen, bevor tatsächlich materialisieren die Ergebnisse; Ein solcher Schritt ist die Abfragekompilierung. Entity SQL-Abfragen wurden auf gute Leistung gezeigt hat, wie sie automatisch zwischengespeichert werden, damit die zweites oder drittes Mal ausführen derselben Abfrage können sie überspringen den Plan-Compiler und verwenden Sie stattdessen den zwischengespeicherten Plan bezeichnet.

Entitätsframework 5 wurde die automatische Zwischenspeicherung für LINQ to Entities-Abfragen sowie eingeführt. In früheren Editionen von Entity Framework, erstellen eine CompiledQuery um beschleunigen war der Leistung Ihrer allgemeinen Brauch werden, wie diese LINQ to Entities-Abfrage zwischengespeichert werden sollen. Da Zwischenspeichern jetzt automatisch ohne eine CompiledQuery durchgeführt wird, rufen wir dieses Feature "Autocompiled Abfragen". Zwischenspeichern von Abfrageplänen finden Sie weitere Informationen zu den Abfrageplancache und seine Funktionsweise.

Entitätsframework erkennt, wenn eine Abfrage erforderlich ist, neu kompiliert werden, und führt Sie dies, wenn die Abfrage aufgerufen wird, auch wenn sie vor dem kompiliert wurde, hatte. Sind allgemeine Bedingungen, unter denen die Abfrage neu kompiliert werden:

-   Ändern die MergeOption für die Abfrage verknüpft ist. Die Abfrage im Cache nicht verwendet werden, stattdessen die Plan-Compiler werden erneut ausgeführt, und der neu erstellte Plan zwischengespeichert.
-   Ändern den Wert der ContextOptions.UseCSharpNullComparisonBehavior. Sie erhalten dieselbe Wirkung wie das Ändern der MergeOption.

Andere Bedingungen können verhindern, dass Ihre Abfrage mit dem Cache. Häufige Beispiele sind:

-   Mithilfe von "IEnumerable"&lt;T&gt;. Enthält&lt;&gt;(T-Wert).
-   Verwenden von Funktionen, die Abfragen mit Konstanten zu erzeugen.
-   Verwenden die Eigenschaften eines Objekts nicht zugeordnet.
-   Verknüpfen Ihre Abfrage an eine andere Abfrage, die erforderlich sind, neu kompiliert werden.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4.1 mithilfe von "IEnumerable"&lt;T&gt;. Enthält&lt;T&gt;(T-Wert)

Entitätsframework zwischenspeichert keine Abfragen, die "IEnumerable" aufrufen&lt;T&gt;. Enthält&lt;T&gt;(T-Wert) für eine in-Memory-Sammlung, da die Werte der Auflistung als flüchtige betrachtet werden. Die folgende Beispielabfrage werden nicht zwischengespeichert werden, damit er immer durch den Plan-Compiler verarbeitet werden:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

Beachten Sie, dass, die ausgeführt wird, dass Sie die Größe der IEnumerable für enthält die bestimmt, wie schnell oder langsam wie die Abfrage kompiliert wird. Bei Verwendung von großen Auflistungen, wie im obigen Beispiel gezeigt kann die Leistung erheblich beeinträchtigt werden.

Entitätsframework 6 enthält Optimierungen, mit der Art "IEnumerable"&lt;T&gt;. Enthält&lt;T&gt;(T-Wert) funktioniert, wenn Abfragen ausgeführt werden. Ist des SQL-Codes, der generiert wird, erzeugt viel schneller und besser lesbar und in den meisten Fällen auch führt schneller auf dem Server.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4.2 mithilfe von Funktionen, die Abfragen mit Konstanten erzeugen.

Die Skip()"", "Take()" "," Contains() "und" DefautIfEmpty() LINQ-Operatoren erzeugen nicht SQL-Abfragen mit Parametern aber stattdessen legen Sie die Werte, die als Konstanten an sie übergeben. Aus diesem Grund Abfragen, die andernfalls über identische am Ende die Abfrage Entwurfsausdruck möglicherweise Plancache, sowohl auf dem EF-Stapel als auch auf dem Datenbankserver, und keine reutilized, es sei denn, die gleichen Konstanten in einer nachfolgenden abfrageausführung verwendet werden. Zum Beispiel:

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

In diesem Beispiel wird jedes Mal, wenn diese Abfrage mit einem anderen Wert für die Id der Abfrage ausgeführt wird, in einen neuen Plan kompiliert werden.

In bestimmten Achten Sie darauf, die Verwendung von Skip und Take beim Paging. In EF6 haben diese Methoden eine Lambda-Überladung, die dadurch des zwischengespeicherten Abfrageplans wiederverwendbare wird, da EF an diese Methoden übergebene Variablen erfassen und auf von ' SqlParameters ' übersetzen kann. Dadurch wird auch den Cache übersichtlicher zu halten, da jede Abfrage mit einer anderen Konstanten für überspringen, und ergreifen Sie eine eigene Cacheeintrag des Abfrage-Plan erhalten würden.

Betrachten Sie den folgenden Code, die nicht optimal, aber dient nur beispielhaft diese Klasse von Abfragen aus:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Eine schnellere Version der gleichen Code enthält z. B. Aufrufen von Skip mit einem Lambda-Ausdruck:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i \< count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Der zweite Codeausschnitt kann bis zu 11 % schneller ausgeführt werden, da der gleiche Abfrageplan verwendet wird, jedes Mal, wenn die Abfrage ausgeführt wird, speichert der CPU-Zeit und vermeidet, den Abfragecache beschädigen. Darüber hinaus, da der Parameter zu überspringende in einem Abschluss ist möglicherweise der Code auch jetzt aussehen:

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4.3 mithilfe der Eigenschaften eines Objekts nicht zugeordnet

Wenn eine Abfrage der Eigenschaften eines Objekttyps nicht zugeordnete verwendet, wie Parameter und klicken Sie dann die Abfrage nicht zwischengespeichert zu erhalten. Zum Beispiel:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

In diesem Beispiel wird davon ausgegangen Sie, dass die Klasse NonMappedType nicht Teil des Modells für die Entität ist. Diese Abfrage kann leicht geändert werden, um nicht verwenden Sie einen Typ nicht zugeordnet, und verwenden stattdessen eine lokale Variable als Parameter für die Abfrage:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

In diesem Fall wird die Abfrage wird in der Lage, zwischengespeichert, abrufen und aus dem Plancache für die Abfrage profitiert.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4.4 Verknüpfen mit Abfragen, für die erneute Kompilierung erforderlich

Entsprechend dem gleichen Beispiel wie oben beschrieben Wenn Sie eine zweite Abfrage, die auf einer Abfrage, die neu kompiliert werden verfügen basiert, wird Ihre gesamte zweite Abfrage auch neu kompiliert werden. Hier ist ein Beispiel zur Veranschaulichung dieses Szenario:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

Im Beispiel ist generisch, aber es wird veranschaulicht, wie Verknüpfen mit FirstQuery SecondQuery nicht zwischengespeichert, abrufen können verursacht werden. Wenn FirstQuery nicht mit einer Abfrage, die erforderlich sind hätte, neu kompilieren zu müssen, würde klicken Sie dann SecondQuery zwischengespeichert wurden.

## <a name="5-notracking-queries"></a>5 NoTracking-Abfragen

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5.1 Deaktivieren der änderungsnachverfolgung um Status-Verwaltungsaufwand zu reduzieren.

Wenn Sie in einem Szenario mit nur-Lese und den Aufwand für das Laden die Objekte in ObjectStateManager vermeiden möchten, können Sie Abfragen "ohne nachverfolgung" ausgeben.  Die änderungsnachverfolgung kann auch auf Abfrageebene deaktiviert werden.

Beachten Sie jedoch, dass durch das Deaktivieren der änderungsnachverfolgung Sie effektiv aus dem Objektcache eingeschaltet sind. Beim Abfragen einer Entität können wir Materialisierung nicht überspringen, indem zuvor materialisierte Abfrageergebnisse aus dem ObjectStateManager abgerufen. Wenn Sie wiederholt die gleichen Entitäten im gleichen Kontext Abfragen sind, möglicherweise tatsächlich eine Leistung profitieren, Aktivieren der änderungsnachverfolgung angezeigt.

Bei der Verwendung von ObjectContext Abfrage speichert ObjectQuery- und "ObjectSet"-Instanzen eine MergeOption, nachdem sie festgelegt ist, und Abfragen, die darauf bestehen, die effektive MergeOption von der übergeordneten Abfrage übernimmt. Wenn "DbContext" verwenden, kann die Überwachung durch Aufrufen des AsNoTracking()-Modifizierers auf "DbSet" deaktiviert werden.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 Deaktivieren der änderungsnachverfolgung für eine Abfrage, bei Verwendung von "DbContext"

Sie können den Modus einer Abfrage zu NoTracking wechseln, durch einen Aufruf an die AsNoTracking()-Methode in der Abfrage zu verketten. Im Gegensatz zu ObjectQuery verfügen nicht die "DbSet" und DbQuery Klassen in der DbContext-API eine änderbare Eigenschaft für die MergeOption.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 Deaktivieren der änderungsnachverfolgung auf Abfrageebene mithilfe von ObjectContext

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 Deaktivieren der änderungsnachverfolgung für eine gesamte Entität, die mit ObjectContext festgelegt

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52-test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5.2 Testmetriken Sie veranschaulicht die Leistungsvorteile bei NoTracking-Abfragen

In diesem Test suchen wir auf Kosten der ObjectStateManager durch Vergleichen der NoTracking-Abfragen für das Modell Navision Überwachung ausfüllen. Finden Sie im Anhang eine Beschreibung des Modells Navision und die Typen von Abfragen, die ausgeführt wurden. In diesem Test haben wir die Liste der Abfragen durchlaufen und jeweils einmal ausgeführt. Wir haben zwei Varianten der Testassembly, einmal mit der NoTracking-Abfragen und einmal mit der standardmäßigen Zusammenführungsoption von "AppendOnly" ausgeführt. Wir haben jede Variante 3 Mal und nutzen den Mittelwert des ausgeführt wird. Zwischen den Tests haben wir den Abfragecache auf dem SQL Server löschen, und Verkleinern die Tempdb, indem Sie die folgenden Befehle ausführen:

1.  DBCC DROPCLEANBUFFERS
2.  DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (Tempdb, 0)

Testergebnisse, Median mehr als 3 ausgeführt wird:

|                        | KEINE NACHVERFOLGUNG – ARBEITSSATZ | KEINE NACHVERFOLGUNG – ZEIT | NUR-ANHÄNGEN SIE-SATZ ARBEITEN | FÜGEN SIE NUR – ZEIT |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entitätsframework 5** | 460361728                 | 1163536 ms         | 596545536                 | 1273042 ms         |
| **Entity Framework 6** | 647127040                 | 190228 ms          | 832798720                 | 195521 ms          |

Entitätsframework 5 müssen einen geringeren Speicherbedarf am Ende der Ausführung als Entity Framework 6 ist. Zusätzliche durch Entity Framework 6 belegte Arbeitsspeicher ist das Ergebnis zusätzlichen Arbeitsspeicher-Strukturen und Code, der neuen Features und eine bessere Leistung zu ermöglichen.

Es gibt auch ein klaren Unterschied im Speicherbedarf bei Verwendung von ObjectStateManager. Entitätsframework 5 erhöht, was den Speicherbedarf von 30 %, wenn nachverfolgt alle Entitäten, die wir aus der Datenbank materialisiert. Entitätsframework 6 erhöht, was den Speicherbedarf von 28 %, dabei.

Im Hinblick auf die Zeit übertrifft Entity Framework 6 Entity Framework 5 in diesem Test durch eine große Rand an. Entitätsframework 6 abgeschlossen, den Test in ungefähr 16 % der Zeit von Entity Framework 5 genutzt wird. Darüber hinaus wird Entity Framework 5 9 % mehr Zeit, wenn der ObjectStateManager verwendet wird. Im Gegensatz dazu wird Entity Framework 6 3 % mehr Zeit, wenn mithilfe von ObjectStateManager verwendet.

## <a name="6-query-execution-options"></a>6 Abfrageausführungsoptionen

Entitätsframework bietet verschiedene Möglichkeiten, die Abfrage. Wir sehen Sie sich die folgenden Optionen, vergleichen Sie die vor- und Nachteile der einzelnen und untersuchen Sie ihre Leistungsmerkmale:

-   LINQ to Entities.
-   Keine nachverfolgung LINQ to Entities.
-   Entity SQL über eine ObjectQuery.
-   Entity SQL über eine von "EntityCommand".
-   "ExecuteStoreQuery".
-   SqlQuery.
-   CompiledQuery.

### <a name="61-------linq-to-entities-queries"></a>6.1-LINQ to Entities-Abfragen

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**Experten**

-   Geeignet für CRUD-Vorgänge.
-   Vollständig materialisierte Objekte.
-   Am einfachsten, mit der Syntax schreiben, die in der Programmiersprache integriert werden.
-   Gute Leistung.

**Nachteile**

-   Bestimmte technische Einschränkungen, z. B.:
    -   Muster DefaultIfEmpty für OUTER JOIN-Abfragen mit komplexer Abfragen als einfache OUTER JOIN-Anweisungen in Entity SQL führen.
    -   Sie können nicht weiterhin verwenden wie die entsprechende allgemeine Muster.

### <a name="62-------no-tracking-linq-to-entities-queries"></a>6.2 keine nachverfolgung LINQ to Entities-Abfragen

Wenn der Kontext ObjectContext abgeleitet:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Wenn der Kontext für "DbContext" abgeleitet:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**Experten**

-   Verbesserte Leistung gegenüber regulären LINQ-Abfragen.
-   Vollständig materialisierte Objekte.
-   Am einfachsten, mit der Syntax schreiben, die in der Programmiersprache integriert werden.

**Nachteile**

-   Nicht geeignet für CRUD-Vorgänge.
-   Bestimmte technische Einschränkungen, z. B.:
    -   Muster DefaultIfEmpty für OUTER JOIN-Abfragen mit komplexer Abfragen als einfache OUTER JOIN-Anweisungen in Entity SQL führen.
    -   Sie können nicht weiterhin verwenden wie die entsprechende allgemeine Muster.

Beachten Sie, dass Abfragen, die skalare Eigenschaften zu projizieren nicht nachverfolgt werden, auch wenn der NoTracking nicht angegeben wird. Zum Beispiel:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

Diese Abfrage nicht explizit angeben, wird der NoTracking, aber da es nicht materialisieren ist ein Typ, der dem Objekt-Status-Manager klicken Sie dann das materialisierte Ergebnis bekannt sind nicht nachverfolgt.

### <a name="63-------entity-sql-over-an-objectquery"></a>6.3 Entity SQL über eine ObjectQuery

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**Experten**

-   Geeignet für CRUD-Vorgänge.
-   Vollständig materialisierte Objekte.
-   Unterstützt das Abfragen Zwischenspeichern von Abfrageplänen.

**Nachteile**

-   Umfasst Text Abfragezeichenfolgen sind anfälliger für Fehler als Abfragekonstrukte integriert die Sprache an.

### <a name="64-------entity-sql-over-an-entity-command"></a>6.4 Entity SQL über eine Entität-Befehl

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

**Experten**

-   Unterstützt das Abfragen Zwischenspeichern von Abfrageplänen in .NET 4.0 (Zwischenspeichern von Abfrageplänen wird von allen anderen Abfragetypen in .NET 4.5 unterstützt).

**Nachteile**

-   Umfasst Text Abfragezeichenfolgen sind anfälliger für Fehler als Abfragekonstrukte integriert die Sprache an.
-   Nicht geeignet für CRUD-Vorgänge.
-   Ergebnisse werden nicht automatisch materialisiert, und müssen aus der Datenleser gelesen werden.

### <a name="65-------sqlquery-and-executestorequery"></a>6.5 SqlQuery und "ExecuteStoreQuery"

SqlQuery für Datenbank:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

SqlQuery auf "DbSet":

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

ExecyteStoreQuery:

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

**Experten**

-   Im Allgemeinen schnellste Leistung, da Plan Compiler umgangen wird.
-   Vollständig materialisierte Objekte.
-   Geeignet für CRUD-Vorgänge bei der Verwendung von "DbSet".

**Nachteile**

-   Abfrage ist und fehleranfällig.
-   Abfrage wird an einen bestimmten Back-End mit Speicher-Semantik statt konzeptionelle Semantik gebunden.
-   Bei der Vererbung vorhanden ist, muss Handgefertigte Abfrage zuordnungsbedingungen für den angeforderten Typ zu berücksichtigen.

### <a name="66-------compiledquery"></a>6.6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**Experten**

-   Stellt ein 7 % leistungsverbesserung von bis zu über reguläre LINQ-Abfragen bereit.
-   Vollständig materialisierte Objekte.
-   Geeignet für CRUD-Vorgänge.

**Nachteile**

-   Erhöhte Komplexität und Aufwand zu programmieren.
-   Verbesserung der Leistung geht verloren, wenn zusätzlich eine kompilierte Abfrage zu erstellen.
-   Einige LINQ-Abfragen können nicht als eine CompiledQuery - z. B. Projektionen von anonymen Typen geschrieben werden.

### <a name="67-------performance-comparison-of-different-query-options"></a>6.7 Leistungsvergleich der anderen Abfrageoptionen

Einfache, in denen die kontexterstellung kein Timeout aufgetreten war, Microbenchmarks wurden in der Praxis testen. Wir Maßen 5000 Mal für einen Satz von Entitäten nicht zwischengespeicherten, in einer kontrollierten Umgebung Abfragen. Diese Zahlen sind, die mit einer Warnung ausgeführt werden: sie spiegeln nicht die tatsächlichen Werte, die von einer Anwendung erstellt, aber stattdessen sind sie ein sehr genau Maß wie viel Leistung Unterschied beim Vergleich von verschiedenen Abfragen-Optionen besteht Äpfel mit Äpfeln, ausgenommen die Kosten für die ein neuen Kontext erstellt werden.

| EF  | Test                                 | Zeit (ms) | Arbeitsspeicher   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | ObjectContext Linq-Abfrage             | 2692      | 38277120 |
| EF5 | "DbContext" Linq-Abfrage keine nachverfolgung     | 2818      | 41840640 |
| EF5 | Linq-Abfrage für "DbContext"                 | 2930      | 41771008 |
| EF5 | Keine nachverfolgung der ObjectContext-Linq-Abfrage | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | ObjectContext Linq-Abfrage             | 3074      | 45248512 |
| EF6 | "DbContext" Linq-Abfrage keine nachverfolgung     | 3125      | 47575040 |
| EF6 | Linq-Abfrage für "DbContext"                 | 3420      | 47652864 |
| EF6 | Keine nachverfolgung der ObjectContext-Linq-Abfrage | 3593      | 45260800 |

![EF5Micro5000Warm](~/ef6/media/ef5micro5000warm.png)

![EF6Micro5000Warm](~/ef6/media/ef6micro5000warm.png)

Microbenchmarks sind sehr empfindlich gegenüber kleinen Änderungen im Code. In diesem Fall unterscheiden sich die Kosten für die Entity Framework 5 und Entity Framework 6 sind, auf das Hinzufügen von [Abfangfunktion](~/ef6/fundamentals/logging-and-interception.md) und [transaktionale Verbesserungen](~/ef6/saving/transactions.md). Diese Zahlen Microbenchmarks sind jedoch einen verstärkten Visionen in einem sehr kleinen informationsfragment der Funktionsweise von Entity Framework. Reale Szenarien betriebsbereiten Abfragen sollte einem Leistungsverlust nicht angezeigt werden, beim Aktualisieren von Entity Framework 5 auf Entity Framework 6.

Um die reale Leistung der anderen Abfrageoptionen vergleichen zu können, haben wir 5 separaten Variationen, in dem wir eine andere Abfrage-Option verwenden, um alle Produkte auswählen, deren Kategoriename "Getränke" wird, erstellt. Jede Iteration umfasst die Kosten für die Erstellung von Kontext und die Kosten für die Umsetzung von Entitäten in alle zurückgegebenen. Bevor Sie die Summe der Timeout bei 1000 Iterationen zu können, werden 10 Iterationen untimed ausgeführt. Die angezeigten Ergebnisse sind die durchschnittliche Ausführung 5 ausgeführt wird, der einzelnen Tests entnommen. Weitere Informationen finden Sie in Anhang B, die den Code für den Test enthält.

| EF  | Test                                        | Zeit (ms) | Arbeitsspeicher   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | ObjectContext-Entity-Befehl                | 621       | 39350272 |
| EF5 | Sql-Abfrage für "DbContext" für Datenbank             | 825       | 37519360 |
| EF5 | ObjectContext-Store-Abfrage                   | 878       | 39460864 |
| EF5 | Keine nachverfolgung der ObjectContext-Linq-Abfrage        | 969       | 38293504 |
| EF5 | ObjectContext Entity Sql mit Objektabfrage | 1089      | 38981632 |
| EF5 | Kompilierte Abfrage ObjectContext                | 1099      | 38682624 |
| EF5 | ObjectContext Linq-Abfrage                    | 1152      | 38178816 |
| EF5 | "DbContext" Linq-Abfrage keine nachverfolgung            | 1208      | 41803776 |
| EF5 | Sql-Abfrage für "DbContext" auf "DbSet"                | 1414      | 37982208 |
| EF5 | Linq-Abfrage für "DbContext"                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | ObjectContext-Entity-Befehl                | 480       | 47247360 |
| EF6 | ObjectContext-Store-Abfrage                   | 493       | 46739456 |
| EF6 | Sql-Abfrage für "DbContext" für Datenbank             | 614       | 41607168 |
| EF6 | Keine nachverfolgung der ObjectContext-Linq-Abfrage        | 684       | 46333952 |
| EF6 | ObjectContext Entity Sql mit Objektabfrage | 767       | 48865280 |
| EF6 | Kompilierte Abfrage ObjectContext                | 788       | 48467968 |
| EF6 | "DbContext" Linq-Abfrage keine nachverfolgung            | 878       | 47554560 |
| EF6 | ObjectContext Linq-Abfrage                    | 953       | 47632384 |
| EF6 | Sql-Abfrage für "DbContext" auf "DbSet"                | 1023      | 41992192 |
| EF6 | Linq-Abfrage für "DbContext"                        | 1290      | 47529984 |


![EF5WarmQuery1000](~/ef6/media/ef5warmquery1000.png)

![EF6WarmQuery1000](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> Der Vollständigkeit halber enthalten wir eine Variante, die eine Entity SQL-Abfrage für ein EntityCommand ausführen. Allerdings nicht, da die Ergebnisse für solche Abfragen nicht materialisiert werden, der Vergleich unbedingt Äpfel mit Äpfeln. Der Test umfasst eine weitgehende Annäherung darstellen, wenn Sie versuchen, den Vergleich eine gerechtere materialisieren.

In diesem Fall End-to-End-Entity Framework 6, Entity Framework 5 aufgrund von leistungsverbesserungen auf mehreren Teilen des Stapels, einschließlich einer viel übersichtlicher "DbContext"-Initialisierung und schnellere MetadataCollection übertrifft&lt;T&gt; Suchvorgänge.

## <a name="7-design-time-performance-considerations"></a>7 Design-Time-Leistungsaspekte

### <a name="71-------inheritance-strategies"></a>7.1 Vererbung Strategien

Eine andere Überlegungen zur Leistung bei Verwendung von Entity Framework ist die Vererbungsstrategie, die Sie verwenden. Entitätsframework unterstützt 3 grundlegende Arten der Vererbung sowie deren Kombinationen:

-   Tabelle pro Hierarchie (TPH) –, in dem jede Vererbung Maps festgelegt, in eine Tabelle mit der eine Unterscheidungsspalte, um anzugeben, welche speziellen Typ in der Hierarchie in der Zeile dargestellt wird.
-   Tabelle pro Typ (TPT) –, in denen jeder eine eigene Tabelle in der Datenbank verfügt über. die untergeordneten Tabellen definieren Sie nur die Spalten, die nicht in der übergeordneten Tabelle enthält.
-   Tabelle pro Klasse (TPC) –, in denen jeder eine eigene vollständige Tabelle in der Datenbank verfügt über. die untergeordneten Tabellen definieren, allen ihren Feldern, einschließlich der im übergeordneten Typen definiert.

Wenn Ihr Modell TPT-Vererbung verwendet wird, werden die Abfragen, die generiert werden komplexer als die sein, die mit den anderen Vererbungsstrategien in längeren Ausführungszeiten im Store zu entstehen generiert werden.  Im Allgemeinen dauert Abfragen über ein TPT-Modell zu generieren und die resultierenden Objekte materialisiert länger.

Finden Sie unter den "Überlegungen zur Leistung bei Verwendung von (Tabelle pro Typ)-TPT-Vererbung im Entitätsframework" MSDN-Blogbeitrag: \< http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.

#### <a name="711-------avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 Vermeiden von TPT in Model First oder Code First-Anwendungen

Wenn Sie ein Modell über eine vorhandene Datenbank, die eine TPT-Schema aufweist erstellen, müssen Sie nicht viele Optionen. Aber wenn Sie eine Anwendung mit Model First oder Code First zu erstellen, sollten Sie TPT-Vererbung für Bedenken hinsichtlich der Leistung.

Wenn Sie Model First im Entity Designer-Assistenten verwenden, erhalten Sie TPT für die Vererbung in Ihrem Modell. Wenn Sie in eine TPH-Vererbung-Strategie mit Model First wechseln möchten, können Sie die "Entity Designer Datenbank Generation Power Pack" zur Verfügung verwenden, aus der Visual Studio Gallery ( \< http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Bei Code First verwenden, um die Zuordnung eines Modells mit '-Vererbung konfigurieren, EF verwendet standardmäßig TPH, daher werden alle Entitäten in der Vererbungshierarchie zu derselben Tabelle zugeordnet. Finden Sie im Abschnitt "Zuordnung mit der Fluent-API" des Artikels "Code zuerst in Entity Framework4.1" im MSDN Magazine ( [ http://msdn.microsoft.com/magazine/hh126815.aspx ](https://msdn.microsoft.com/magazine/hh126815.aspx)) Weitere Informationen.

### <a name="72-------upgrading-from-ef4-to-improve-model-generation-time"></a>7.2 Aktualisieren von EF4 zur Verbesserung der modellgenerierung Zeit

Eine SQL Server-spezifische Verbesserung für den Algorithmus, der generiert die Store-Ebene (SSDL) des Modells ist in Entity Framework 5 und 6, und als Update für Entity Framework 4 verfügbar, wenn Visual Studio 2010 SP1 installiert ist. Die folgenden Testergebnisse zeigen die Verbesserung beim Generieren eines großen Modells in diesem Fall das Navision-Modell. Weitere Details finden Sie unter Anhang C.

Das Modell enthält 1005 Entitätenmengen und Zuordnungssätze 4227.

| Konfiguration                              | Aufschlüsselung der verbrauchte Zeit                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010 Entitätsframework 4     | SSDL-Generierung: 2 Std. 27 Min. <br/> Zuordnung generieren: 1 Sekunde <br/> CSDL-Generierung: 1 Sekunde <br/> ObjectLayer generieren: 1 Sekunde <br/> Generieren von Sichten: 2 h 14 Min. |
| Visual Studio 2010 SP1, Entitätsframework 4 | SSDL-Generierung: 1 Sekunde <br/> Zuordnung generieren: 1 Sekunde <br/> CSDL-Generierung: 1 Sekunde <br/> ObjectLayer generieren: 1 Sekunde <br/> Generieren von Sichten: 1 Std. 53 Min.   |
| Visual Studio 2013, Entitätsframework 5     | SSDL-Generierung: 1 Sekunde <br/> Zuordnung generieren: 1 Sekunde <br/> CSDL-Generierung: 1 Sekunde <br/> ObjectLayer generieren: 1 Sekunde <br/> Generieren von Sichten: 65 Minuten    |
| Visual Studio 2013, Entitätsframework 6     | SSDL-Generierung: 1 Sekunde <br/> Zuordnung generieren: 1 Sekunde <br/> CSDL-Generierung: 1 Sekunde <br/> ObjectLayer generieren: 1 Sekunde <br/> Generieren von Sichten: 28 Sekunden.   |


Es ist erwähnenswert, dass beim SSDL zu generieren, die Last fast ausschließlich auf dem SQL-Server aufgewendet wird während der Client-Entwicklungscomputer wartet im Leerlauf, für die Ergebnisse vom Server zurückgegeben werden. Datenbankadministratoren sollten besonders, dass diese Verbesserung zu schätzen wissen. Es ist auch erwähnenswert, dass im Wesentlichen die Gesamtkosten der Modellerstellung im Generieren von Sichten jetzt stattfindet.

### <a name="73-------splitting-large-models-with-database-first-and-model-first"></a>7.3 Teilen Sie große Modelle mit Datenbank zuerst und Model First

Mit zunehmender Größe des Datenbankmodells wird die Oberfläche des Designers überladen und schwierig zu verwenden. In der Regel betrachten wir ein Modell mit mehr als 300 Entitäten, zu groß, um effektiv der Designer verwendet werden soll. Im folgenden Blogbeitrag beschreibt mehrere Optionen für das Aufteilen von großer Models: \< http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

Der Beitrag wurde für die erste Version von Entity Framework geschrieben, aber die Schritte gelten weiterhin.

### <a name="74-------performance-considerations-with-the-entity-data-source-control"></a>7.4-leistungsüberlegungen zu Entity Datenquellen-Steuerelements

Wir haben gesehen Fälle im Multithread-Leistungs- und Belastungstests, wirkt sich negativ auf die Leistung einer Webanwendung, die über das EntityDataSource-Steuerelement deutlich. Die zugrunde liegende Ursache ist, dass das EntityDataSource wiederholt MetadataWorkspace.LoadFromAssembly auf den verwiesen wird, von der Webanwendung zum Ermitteln der Typen, die als Entitäten verwendet werden Assemblys aufruft.

Die Lösung besteht darin, dem "ContextTypeName" von EntityDataSource auf den Namen der abgeleiteten Klasse ObjectContext festgelegt. Dadurch wird der Mechanismus, der alle referenzierten Assemblys für Entitätstypen scannt deaktiviert.

Festlegen des Felds "ContextTypeName" wird verhindert, dass auch ein funktionales Problem, in dem das EntityDataSource in .NET 4.0 ReflectionTypeLoadException löst aus, wenn einen Typ nicht aus einer Assembly über Reflektion geladen werden können. Dieses Problem wurde in .NET 4.5 behoben.

### <a name="75-------poco-entities-and-change-tracking-proxies"></a>7.5 POCO-Entitäten und Proxys mit änderungsnachverfolgung

Entitätsframework ermöglicht Ihnen, benutzerdefinierte Datenklassen zusammen mit dem Datenmodell zu verwenden, ohne Änderungen an den Datenklassen selbst vornehmen. Dies bedeutet, dass Sie POCO-Objekte (Plain-old CLR objects), z. B. vorhandene Domänenobjekte, mit dem Datenmodell verwenden können. Diese POCO-Datenklassen (auch bekannt als Dauerhaftigkeit ignorierende Objekte), die auf Entitäten zugeordnet sind, die in einem Datenmodell definiert sind, unterstützen die meisten derselben Abfrage, einfügen, aktualisieren und Löschverhalten wie Entitätstypen, die von den Entity Data Model-Tools generiert werden.

Entitätsframework kann auch erstellen die Webdienstproxy-Klassen abgeleitet aus den POCO-Typen, die verwendet werden, wenn Sie Funktionen wie lazy Loading und automatischen Nachverfolgen von Änderungen in POCO-Entitäten aktivieren möchten. Die POCO-Klassen müssen bestimmte Anforderungen Entity Framework, Proxys verwenden können, wie hier beschrieben: [ http://msdn.microsoft.com/library/dd468057.aspx ](https://msdn.microsoft.com/library/dd468057.aspx).

Wahrscheinlichkeit Änderungsnachverfolgungsproxys benachrichtigt den Objekt-Status-Manager jedes Mal, die die Eigenschaften der Entität verfügt über einen Wert, der geändert, damit Entity Framework den tatsächlichen Zustand der Entitäten immer weiß, dass. Dies ist der Text, der den Setter-Methoden, Eigenschaften des Benachrichtigungsereignisse hinzugefügt wird und des Objekt-Status-Managers, die derartige Ereignisse verarbeiten. Hinweis: Erstellen eines Proxys in der Regel wird der Entität werden teurer als eine nicht-Proxy-POCO-Entität aufgrund von den hinzugefügten Satz von Ereignissen, die von Entity Framework erstellte erstellen.

Wenn eine POCO-Entität ein Proxy für die änderungsnachverfolgung nicht besitzt, werden Änderungen durch vergleichen den Inhalt Ihrer Entitäten mit der Kopie einen zuvor gespeicherten Zustand gefunden. Diese umfassende Vergleich werden ein langwieriger Prozess Wenn viele Entitäten in den Kontext verfügbar ist oder wenn Ihre Entitäten über eine große Menge von Eigenschaften haben, auch wenn keine von ihnen geändert, seit der letzten Vergleich.

Zusammenfassung: Sie Zahlen eine Leistungseinbuße, wenn es sich bei die änderungsnachverfolgung mit Proxys erstellen, aber die änderungsnachverfolgung können Sie den Erkennungsprozess Änderung beschleunigen, wenn die Entitäten haben viele Eigenschaften oder wenn Sie viele Entitäten in Ihrem Modell. Für Entitäten mit einer kleinen Anzahl von Eigenschaften, in denen die Menge von Entitäten zu viel wachsen nicht, mit Change Tracking-Proxys viel Vorteil möglicherweise nicht.

## <a name="8-loading-related-entities"></a>8 Laden von verknüpften Entitäten

### <a name="81-lazy-loading-vs-eager-loading"></a>8.1 Lazy Loading im Vergleich zu Eager Loading

Entitätsframework bietet verschiedene Möglichkeiten, die Entitäten zu laden, die im Zusammenhang mit der Zielentität. Z. B. Wenn Sie Produkte abzufragen, es gibt verschiedene Möglichkeiten, die verknüpften Bestellungen geladen werden in den Zustands-Manager-Objekt. Vom Standpunkt der Leistung wird die größte Frage beim Laden von verknüpften Entitäten zu berücksichtigen, ob Lazy Loading oder Eager Loading verwendet werden.

Wenn Sie Eager Loading verwenden, werden die verknüpften Entitäten zusammen mit Ihrer Zielentitätenmenge geladen. Sie verwenden eine Include-Anweisung in der Abfrage an, um anzugeben, die verknüpften Entitäten, die Sie importieren möchten.

Wenn Sie Lazy Loading verwenden, übernimmt die ursprüngliche Abfrage nur die zielentitätssammlung. Jedoch wenn Sie Zugriff auf eine Navigationseigenschaft, für den Speicher laden Sie die verknüpfte Entität einer anderen Abfrage ausgegeben.

Sobald eine Entität geladen wurde, lädt alle weiteren Abfragen für die Entität es direkt aus dem Objekt-Status-Manager, ob Sie lazy Loading oder eager Loading verwenden.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8.2 wie zwischen Lazy Loading und Eager Loading

Wichtig ist, dass Sie den Unterschied zwischen Lazy Loading und Eager Loading verstehen, sodass Sie die richtige Wahl für Ihre Anwendung vornehmen können. Dies können Sie den Kompromiss zwischen mehrere Anforderungen für die Datenbank im Vergleich zu einer einzelnen Anforderung zu bewerten, die eine große Nutzlast enthalten können. Es kann mit eager Loading in einigen Teilen der Anwendung und lazy Loading in anderen Teilen geeignet sein.

Nehmen wir an, dass Sie für die Kunden Abfragen, die im Vereinigten Königreich und die Anzahl der Bestellungen Leben möchten, beispielsweise davon, was hinter den Kulissen passiert.

**Verwenden Eager Loading**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**Verwendung von Lazy Loading**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

Wenn Sie eager Loading verwenden zu können, müssen Sie eine einzelne Abfrage, die alle Kunden zurückgibt ausgeben und alle Aufträge. Der Speicherbefehl sieht folgendermaßen aus:

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

Wenn lazy Loading verwenden zu können, müssen Sie zunächst die folgende Abfrage ausgeben:

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

Und jedes Mal, die Sie Zugriff auf die Navigationseigenschaft von Bestellungen eines Kunden für den Speicher wird eine andere Abfrage wie folgt ausgegeben:

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

Weitere Informationen finden Sie unter den [laden verbundener Objekte](https://msdn.microsoft.com/library/bb896272.aspx).

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 Lazy Loading im Vergleich zu Eager Loading Cheat Sheet für Algorithmen

Es gibt keine als eine allgemeingültige Eager Load und Lazy Load auswählen. Zuerst versucht, die zu den Unterschieden zwischen beide Strategien, damit Sie gut informierte Entscheidung tun können; Beachten Sie auch, ob Ihr Code auf eine der folgenden Szenarien geeignet ist:

| Szenario                                                                    | Unsere Empfehlung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Möchten Sie die abgerufenen Entitäten viele Navigationseigenschaften zugreifen? | **Keine** – beide Optionen werden wahrscheinlich der Fall. Allerdings ist die Nutzlast, die die Abfrage eingebunden wurde nicht zu groß ist, auftreten können, die Leistungsvorteile mit unverzüglichem Laden, da es weniger erfordern werden Netzwerkroundtrips um die Objekte zu materialisieren. <br/> <br/> **Ja** – Wenn Sie viele Navigationseigenschaften aus den Entitäten zugreifen müssen, würden Sie tun, indem Sie mehrere Anweisungen, in der Abfrage mit unverzüglichem Laden enthalten. Weitere Entitäten eingeschlossen werden, desto größer wird die Nutzlast die Abfrage zurückgibt. Nachdem Sie drei oder mehr Entitäten in die Abfrage einschließen, wechseln Sie ggf. zu verzögerten Laden. |
| Wissen Sie, welche Daten genau zur Laufzeit benötigt werden?                   | **Keine** -Lazy Loading werden besser für Sie. Sie können, andernfalls beenden Sie, dass Sie nicht benötigen Daten Abfragen. <br/> <br/> **Ja** – Eager Loading ist wahrscheinlich die beste Lösung, es hilft beim gesamten legt schneller geladen. Wenn Ihre Abfrage erfordert eine sehr große Datenmenge abrufen, und dies zu langsam wird, wiederholen Sie dann stattdessen Laden verzögert.                                                                                                                                                                                                                                                       |
| Werden der Code ist weit entfernt von der Datenbank ausgeführt? (erhöhte Netzwerklatenz)  | **Keine** : Wenn die Netzwerklatenz kein Problem dar, ist möglicherweise bei Verwendung von Lazy Loading Ihren Code vereinfachen. Denken Sie daran, dass die Topologie Ihrer Anwendung also Datenbank NEAR als selbstverständlich nicht ändern kann. <br/> <br/> **Ja** : Wenn das Netzwerk ein Problem ist, nur Sie entscheiden können, was für Ihr Szenario besser geeignet ist. Eager Loading wird in der Regel besser sein, da weniger Roundtrips erforderlich ist.                                                                                                                                                                                                      |


#### <a name="822-------performance-concerns-with-multiple-includes"></a>8.2.2 Leistungsbedenken mit mehreren enthält

Wenn wir Fragen zur Performance, die Zeit von Serverproblemen-Antwort enthalten hören, ist die Quelle des Problems häufig Abfragen mit mehreren Include-Anweisungen. Während einschließlich verknüpfte Entitäten in einer Abfrage leistungsfähig ist, ist es wichtig zu verstehen, was hinter den Kulissen passiert.

Es dauert relativ lange dauert, bis eine Abfrage mit mehreren Include-Anweisungen, um unsere internen Plan Compiler erstellt den Speicherbefehl zu durchlaufen. Der Großteil dieser Zeit ist hat versucht, die sich ergebende Abfrage zu optimieren. Der generierte Speicherbefehl enthält einer Outer Join oder Union für jede einschließen, je nach Ihrer Zuordnung. Derartige Abfragen werden in großen verbundene Diagramme aus der Datenbank in einer einzelnen Nutzlast, anzuzeigen, in dem alle Bandbreitenprobleme acerbate wird, insbesondere, wenn Sie viele der Redundanz in der Nutzlast (z. B., wenn mehrere Ebenen von Include verwendet werden, durchlaufen Zuordnungen in der 1: n Richtung).

Sehen Sie sich für Fälle, in denen Ihre Abfragen übermäßig große Nutzlasten zurückgegeben werden, durch den Zugriff auf das zugrunde liegende TSQL für die Abfrage von ToTraceString verwenden und in SQL Server Management Studio, um die Größe der Nutzlast finden Sie unter der Store-Befehl ausführen. Führen Sie in solchen Fällen können Sie versuchen, reduzieren die Anzahl der Include-Anweisungen in der Abfrage nur die benötigten Daten. Oder Sie Ihre Abfrage in eine kleinere Unterabfragen, z. B. aufteilen:

**Vor dem Unterbrechen der Abfrage ein:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

**Nach der aktuellen Abfrage an:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

Dies funktioniert nur für nachverfolgte Abfragen, wie wir machen verwenden die Möglichkeit, die der Kontext zum automatischen Ausführen von Identity-Auflösung und Zuordnung Fixup wurde.

Wie bei lazy Loading werden der Nachteil hierbei mehr Abfragen für geringere Nutzlasten. Sie können auch Projektionen der einzelnen Eigenschaften explizit nur die benötigten Daten aus jeder Entität auswählen, aber nicht laden Sie Entitäten in diesem Fall und Updates werden nicht unterstützt.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3-problemumgehung für das verzögerte Laden von Eigenschaften zu erhalten.

Entitätsframework unterstützt derzeit keine lazy Loading von skalaren oder komplexen Eigenschaften. Allerdings in Fällen, in dem Sie eine Tabelle verfügen, die ein großes Objekt wie ein BLOB enthält, können Sie die tabellenaufteilung große Eigenschaften in eine getrennte Entität getrennt. Nehmen wir beispielsweise an, dass Sie über eine Product-Tabelle verfügen, die eine Varbinary-Spalte Photo enthält. Wenn Sie häufig Zugriff auf diese Eigenschaft in Ihren Abfragen nicht möchten, können Sie die Tabelle aufteilen, um nur die Teile der Entität zu importieren, die Sie normalerweise benötigen. Die Entität, die das Produktfoto darstellt wird nur bei Bedarf explizit geladen.

Eine gute Ressource, die zeigt, wie die tabellenaufteilung aktiviert ist, die Gil Fink "Tabelle Aufteilen in Entity Framework"-Blogbeitrag: \< http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 Weitere Überlegungen

### <a name="91------server-garbage-collection"></a>9.1 Garbagecollection-server

Einige Benutzer können Ressourcenkonflikte auftreten, die die Parallelität einschränkt, die sie erwarten, wenn der Garbage Collector nicht ordnungsgemäß konfiguriert ist. Wenn EF in einem Multithread-Szenario verwendet wird, oder in einer beliebigen Anwendung, die eine serverseitige System ähnelt, stellen Sie sicher, dass Garbage Collection auf dem Server zu aktivieren. Dies erfolgt über eine einfache Einstellung in der Konfigurationsdatei der Anwendung:

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

Dies sollte Ihre Threadkonflikte verringern und den Durchsatz erhöhen, indem Sie bis zu 30 % in CPU-Kapazität erschöpft-Szenarien. Im Allgemeinen sollten Sie immer testen, das Anwendungsverhalten mit der klassischen Garbage Collection (die besser für die Benutzeroberfläche und Client-Seite-Szenarien optimiert ist) sowie die Garbage Collection auf dem Server.

### <a name="92------autodetectchanges"></a>9.2 AutoDetectChanges

Wie bereits erwähnt, kann Entity Framework Leistungsprobleme angezeigt, wenn es sich bei der Objektcache viele Entitäten verfügt. Bestimmte Vorgänge, z. B. hinzufügen "," entfernen "," Suchen "," Eintrag "und" SaveChanges ", lösen Aufrufe von DetectChanges die verwenden möglicherweise eine große Menge an CPU, die basierend auf der Objektcache wie groß geworden ist. Der Grund dafür ist, dass Objektcache und das Objekt Status-Manager, versuchen Sie, als bleiben synchronisiert werden, wie möglich auf jeden Vorgang, der an einen Kontext durchgeführt werden, sodass die erzeugten Daten garantiert ist, unter einer Vielzahl von Szenarien korrekt zu sein.

Es ist im Allgemeinen empfohlen, automatische änderungserkennung für Entity Framework für die gesamte Lebensdauer der Anwendung aktiviert zu lassen. Wenn Ihr Szenario durch hohe CPU-Auslastung negativ beeinflusst wird wird, und Ihre Profile anzugeben, dass die Ursache der Aufruf von DetectChanges, beachten Sie, vorübergehend AutoDetectChanges im sensiblen Bereich Ihres Codes deaktivieren:

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

Vor einem ausschalten AutoDetectChanges durch, ist es gut zu wissen, dass dies möglicherweise dazu, dass Entity Framework, verlieren die Möglichkeit, bestimmte Informationen zu den Änderungen zu verfolgen, die für die Entitäten durchgeführt werden. Wenn nicht ordnungsgemäß verarbeitet, kann dies Dateninkonsistenz in Ihrer Anwendung führen. Finden Sie weitere Informationen zum Deaktivieren der AutoDetectChanges, \< http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93------context-per-request"></a>9.3 Kontext pro Anforderung

Entitätsframework-Kontexte sollen verwendet werden, da es sich bei Auftreten von kurzlebige-Instanzen verwendet, um die optimale Leistung zu erzielen. Kontexte sind erwartungsgemäß von kurzer kurzlebig und verworfen, und daher sehr einfache und Metadaten, die möglichst reutilize implementiert wurden. -Szenarien ist es wichtig zu beachten Sie, dass keinen Kontext für mehr als die Dauer einer einzelnen Anforderung. Auf ähnliche Weise sollte in nicht-Szenarien Kontext basierend auf Ihre Kenntnisse zu den verschiedenen Ebenen des Zwischenspeicherns im Entity Framework verworfen werden. Im Allgemeinen sollte eine vermeiden, dass eine Kontextinstanz während der gesamten Lebensdauer der Anwendung als auch Kontexte pro Thread und statischen Kontext werden können.

### <a name="94------database-null-semantics"></a>9.4-Datenbank-null-Semantik

Entitätsframework standardmäßig generiert die SQL-Code mit C\# null-Vergleichssemantik. Beachten Sie die folgende Beispielabfrage:

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

In diesem Beispiel haben wir eine Anzahl von nullable-Variablen auf NULL festlegbaren Eigenschaften für die Entität, z. B. SupplierID "und" UnitPrice vergleichen. Das generierte SQL für diese Abfrage fordert, wenn der Wert des Parameters den Wert der Spalte identisch ist oder wenn die Parameter und die Werte der Spalte null sind. Dies wird ausgeblendet, wie die Datenbank-Server NULL-Werte behandelt, und stellt eine konsistente C\# Erfahrung über verschiedene Datenbankanbieter null. Auf der anderen Seite der generierte Code ist ein wenig kompliziert, und kann nicht ausgeführt werden, auch bei den Betrag der Vergleiche in der Where-Anweisung der Abfrage eine große Anzahl erreicht.

Eine Möglichkeit, dieser Situation ist die Verwendung von Datenbank-null-Semantik. Beachten Sie, die dies für den C-möglicherweise unterschiedlich Verhalten möglicherweise\# null-Semantik, da jetzt Entity Framework einfacheres SQL, die die Methode verfügbar macht generiert, die Datenbank-Engine null-Werte behandelt. Datenbank-null-Semantik kann pro kontextbezogen mit einer einzelnen Konfigurationszeile mit der Kontext aktiviert sein:

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

Kleine bis mittlere Größe Abfragen zeigt keine leistungsverbesserung der wahrnehmbaren bei Verwendung von Datenbank-null-Semantik, aber der Unterschied wird umso deutlicher, für Abfragen mit einer großen Anzahl von möglichen null-Vergleiche.

In der Beispielabfrage, die oben genannten war der Leistungsunterschied weniger als 2 % in einer Microbenchmark in einer kontrollierten Umgebung ausgeführt wird.

### <a name="95------async"></a>9.5 Async

Entity Framework 6 eingeführte Unterstützung von asynchronen Vorgängen bei der Ausführung unter .NET 4.5 oder höher. Zum größten Teil, der Anwendungen, die e/a Konflikte verknüpft werden, profitieren am meisten von der asynchronen Abfrage mithilfe und Speichervorgängen. Wenn Ihre Anwendung von e/a-Konflikte nicht der Fall ist, wird die Verwendung von Async im besten Fall wird synchron ausgeführt und geben das Ergebnis zurück, in die gleiche Menge an Zeit wie ein synchroner Aufruf oder im schlimmsten Fall einfach verzögern der Ausführung einer asynchronen Aufgabe und Hinzufügen von zusätzlichen tim e für die Ausführung Ihres Szenarios.

Informationen zu asynchroner Programmierung Arbeit, die helfen, die Sie die Entscheidung, ob Async die Leistung Ihrer Anwendung verbessert besuchen [ http://msdn.microsoft.com/library/hh191443.aspx ](https://msdn.microsoft.com/library/hh191443.aspx). Weitere Informationen zur Verwendung von asynchronen Vorgängen auf Entity Framework finden Sie unter [asynchronen Abfrage- und speichern](~/ef6/fundamentals/async.md
).

### <a name="96------ngen"></a>9.6 NGEN

Entitätsframework 6 stammt nicht in der Standardinstallation von .NET Framework. Daher sind die Entity Framework-Assemblys nicht, dass NGEN standardmäßig würde, was bedeutet, dass alle der Entity Framework-Code gelten die gleichen JIT'ing Kosten als jede andere MSIL-Assembly ist. Dies kann die F5-Erfahrung beim Entwickeln und auch auf den Kaltstart der Anwendung in die produktionsumgebungen beeinträchtigt werden. Um die Senkung der Kosten für CPU und Arbeitsspeicher des JIT'ing ist es ratsam, NGEN-images von Entity Framework nach Bedarf. Weitere Informationen zum Verbessern der startleistung von Entity Framework 6 mit NGEN finden Sie unter [Verbessern der Startleistung mit NGen](~/ef6/fundamentals/performance/ngen.md).

### <a name="97------code-first-versus-edmx"></a>9.7 code First und EDMX-Datei

Entity Framework Gründen über die Impedance-Mismatch-Problems zwischen objektorientierter Programmierung und relationalen Datenbanken, indem Sie eine in-Memory-Darstellung des konzeptionellen Modell (die Objekte), das Speicherschema (Datenbank) und eine Zuordnung zwischen den zwei. Diese Metadaten ist ein Entity Data Model oder EDM kurz bezeichnet. Aus diesem EDM Entity Framework die Objekte im Arbeitsspeicher der Datenbank die Ansichten auf Roundtrip Daten abgeleitet und sichern.

Das konzeptionelle Modell, das Speicherschema und die Zuordnung gibt Wenn Entity Framework, früher mit einer EDMX-Datei verwendet wird, dann die Modell laden Phase nur verfügt über zu überprüfen, ob das EDM richtig (z. B. Stellen Sie sicher, dass keine Zuordnungen nicht vorhanden sind), klicken Sie dann Generieren Sie die Ansichten zu, überprüfen Sie die Ansichten und haben Sie diese Metadaten, die zur Verwendung bereit. Nur dann kann eine Abfrage ausgeführt werden, oder neue Daten im Datenspeicher gespeichert werden.

Der Code First-Ansatz ist im Grunde eine anspruchsvolle Entity Data Model-Generator. Das Entity Framework verfügt über ein EDM aus den bereitgestellten Code zu erstellen; Dies erfolgt durch die Klassen, die das Modell, Anwenden von Konventionen, und konfigurieren das Modell über die Fluent-API zum Analysieren. Nachdem das EDM erstellt wurde, verhält sich das Entity Framework im Wesentlichen Weise, wie würden sie hatte, eine EDMX-Datei wurde im Projekt vorhanden. Auf diese Weise bietet beim Erstellen des Modells aus Code First zusätzlichen Komplexität, der in eine langsamere Startzeit für das Entity Framework, dass eine EDMX-Datei im Vergleich zu übersetzt. Die Kosten sind völlig abhängig von der Größe und Komplexität des Modells, das erstellt wird.

Bei der Auswahl EDMX-Datei im Vergleich zu Code First verwenden, ist es wichtig zu wissen, dass die Flexibilität, die durch Code First eingeführt, die Kosten für das Erstellen des Modells zum ersten Mal erhöht. Wenn Ihre Anwendung gegen die Kosten für diese ersten Last widerstandsfähig in der Regel werden Code First die bevorzugte Methode zu.

## <a name="10-investigating-performance"></a>10 Untersuchen der Leistung

### <a name="101-using-the-visual-studio-profiler"></a>10.1 mithilfe der Visual Studio-Profiler

Wenn Sie Leistungsprobleme mit Entity Framework haben, können Sie einen Profiler, wie in Visual Studio integriert verwenden, um anzuzeigen, in denen Ihre Anwendung Zeit verloren geht. Dies ist das Tool, das wir verwendet, um die Kreisdiagramme im Blogbeitrag "Exploring the Performance of ADO.NET Entity Framework - Teil 1" zu generieren ( \< http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) , die anzeigen, in denen Entity Framework bei Abfragen für kalte und warme befindet.

"Profilerstellung Entitätsframework, die mithilfe der Visual Studio 2010-Profiler" im Blogbeitrag von Daten und Modellieren von Customer Advisory Team geschrieben wurde, zeigt ein praktisches Beispiel, wie sie den Profiler verwendet, um ein Leistungsproblem zu untersuchen.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Dieser Beitrag wurde für eine Windows-Anwendung geschrieben. Wenn Sie ein Profil eine Webanwendung müssen funktionieren die Tools Windows Performance Recorder (WPR) und Windows Performance Analyzer (WPA) eine bessere Leistung als die Verwendung von Visual Studio. WPR "und" WPA sind Teil der Windows Performance Toolkit, das in das Windows Assessment and Deployment Kit enthalten ist ( [ http://www.microsoft.com/en-US/download/details.aspx?id=39982 ](https://www.microsoft.com/en-US/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>10.2 Anwendung/Datenbank-profilerstellung

Tools wie der Profiler in Visual Studio integriert erfahren Sie, wo Ihre Anwendung Zeit verbringt, ist.  Eine andere Art von Profiler ist verfügbar, die dynamische Analyse der ausgeführten Anwendung, entweder in der Produktion oder vor der Produktion je nach Anforderungen und sucht nach häufige Fehlerquellen und Anti-Muster der Zugriff auf die Datenbank.

Zwei kommerziell erhältliche Tools für die profilerstellung werden die Entity Framework Profiler ( \< http://efprof.com>) und ORMProfiler ( \< http://ormprofiler.com>).

Wenn Ihre Anwendung eine MVC-Anwendung mithilfe von Code First ist, können Sie StackExchanges-MiniProfiler verwenden. Scott Hanselman beschreibt dieses Tool, das in seinem Blog unter: \< http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Weitere Informationen zur profilerstellung Ihrer Anwendung Datenbankaktivität, Julie lermans MSDN Magazine-Artikel, finden Sie unter [Datenbankaktivitäten im Entity Framework-Profilerstellung](https://msdn.microsoft.com/magazine/gg490349.aspx).

### <a name="103-database-logger"></a>10.3-Datenbank-Protokollierung

Bei Verwendung von Entity Framework 6 auch sollten Sie mithilfe der integrierten Protokollierung-Funktion. Die Datenbank-Eigenschaft des Kontexts kann angewiesen werden, um die Aktivität über eine einfache einzeilige-Konfiguration zu protokollieren:

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

In diesem Beispiel wird die Aktivität "Datenbank" in der Konsole protokolliert werden, aber die Log-Eigenschaft kann konfiguriert werden, um eine Aktion aufzurufen&lt;Zeichenfolge&gt; delegieren.

Wenn Sie aktivieren möchten datenbankprotokollierung ohne neu kompilieren zu müssen, und Sie werden mithilfe von Entity Framework 6.1 oder höher, Sie können dies vornehmen, sofern einen Interceptor in der Datei "Web.config" oder "App.config" Ihrer Anwendung.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

Weitere Informationen dazu, wie Protokollierung hinzufügen, ohne erneute Kompilierung wechseln Sie zu \< http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.

## <a name="11-appendix"></a>11 Anhang

### <a name="111-a-test-environment"></a>11.1 a Test-Umgebung

Diese Umgebung verwendet einen 2-Machine-Setup mit der Datenbank auf einem separaten Computer von der Clientanwendung. Computer sind im selben Rack, also die Netzwerklatenz relativ gering, aber realistischer als eine Umgebung für die einzelnen Computer.

#### <a name="1111-------app-server"></a>11.1.1-app-Server

##### <a name="11111------software-environment"></a>11.1.1.1-Software-Umgebung

-   Entity Framework 4-Software-Umgebung
    -   Name des Betriebssystems: Windows Server 2008 R2 Enterprise SP1.
    -   Visual Studio 2010 – Ultimate.
    -   Visual Studio 2010 SP1 (nur für einige Vergleiche).
-   Entity Framework 5 und 6-Software-Umgebung
    -   Name des Betriebssystems: Windows 8.1 Enterprise
    -   Visual Studio 2013 – Ultimate.

##### <a name="11112------hardware-environment"></a>11.1.1.2 Hardwareumgebung

-   Dual-Prozessor: Intel(R) Xeon(R) CPU L5520 W3530 @ mit 2,27 GHz, 2261 Mhz8 GHz, 4 Kerne, 84 logische Prozessoren.
-   2412 GB RamRAM.
-   136 GB SCSI250GB SATA 7200 u/Min / 3GB/s-Laufwerk in 4 Partitionen aufgeteilt.

#### <a name="1112-------db-server"></a>11.1.2-DB-server

##### <a name="11121------software-environment"></a>11.1.2.1-Software-Umgebung

-   Name des Betriebssystems: Windows Server 2008 R28.1 Enterprise SP1.
-   SQL Server 2008 R22012.

##### <a name="11122------hardware-environment"></a>11.1.2.2 Hardwareumgebung

-   Einzelner Prozessor: Intel(R) Xeon(R) CPU L5520 mit 2,27 GHz, 2261 MhzES-1620 0 @ 3.60 GHz, 4 Kerne, 8 logische Prozessoren.
-   824 GB RamRAM.
-   465 GB ATA500GB SATA 7200 Rpm 6 GBIT/s-Laufwerk in 4 Partitionen aufgeteilt.

### <a name="112------b-query-performance-comparison-tests"></a>11.2 b Abfrage Leistung im Vergleich tests

Das Northwind-Modell wurde verwendet, um diese Tests auszuführen. Es wurde aus der Datenbank, die mit dem Entity Framework Designer generiert. Klicken Sie dann wurde der folgende Code verwendet, um die Leistung von Optionen für die Abfrage vergleichen:

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a>11.3 C. Navision Modell

Die Navision-Datenbank ist eine große Datenbank verwendet, um die Demo Microsoft Dynamics-NAV. Die generierte konzeptionelle Modell enthält 1005 Entitätenmengen und Zuordnungssätze 4227. Das Modell im Test verwendeten "flach" – ist verfügt über keine Vererbung hinzugefügt wurde.

#### <a name="1131-queries-used-for-navision-tests"></a>11.3.1 Abfragen für die Navision tests

Die Liste der Abfragen verwendet, mit dem Modell Navision enthält 3 Kategorien von Entity SQL-Abfragen:

##### <a name="11311-lookup"></a>11.3.1.1-Suche

Eine einfache Suchabfrage ohne Aggregationen

-   Anzahl: 16232
-   Beispiel:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312-singleaggregating"></a>11.3.1.2 SingleAggregating

Eine normale BI-Abfrage mit mehrere Aggregationen, jedoch keine Teilergebnisse (Einzelabfrage)

-   Anzahl: 2313
-   Beispiel:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

In denen MDF\_SessionLogin\_Zeit\_Max() in das Modell definiert ist:

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313-aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

Eine BI-Abfrage mit Aggregationen und Teilergebnisse (über die Union aller)

-   Anzahl: 178
-   Beispiel:

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
