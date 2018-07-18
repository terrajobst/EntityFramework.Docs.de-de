---
title: Arbeiten mit "DbContext" - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
caps.latest.revision: 3
ms.openlocfilehash: 865c9883ce25f405a173791df4e46b98550cd41f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120802"
---
# <a name="working-with-dbcontext"></a>Arbeiten mit "DbContext"

Um Entity Framework zum Abfragen, einfügen, aktualisieren und Löschen von Daten mithilfe von .NET-Objekten verwenden, müssen Sie zuerst [erstellen Sie ein Modell](~/ef6/modeling/index.md) der zugeordnet werden, die Entitäten und Beziehungen, die in Ihrem Modell, Tabellen in einer Datenbank definiert sind.

Sobald Sie ein Modell verfügen, ist die primäre Klasse, die Ihre Anwendung interagiert mit `System.Data.Entity.DbContext` (häufig als der Context-Klasse bezeichnet). Sie können einen "DbContext", die ein Modell zugeordnet sind:
- Schreiben und Ausführen von Abfragen   
- Abfrageergebnisse als Entitätsobjekte zu materialisieren.
- Nachverfolgen von Änderungen, die mit diesen Objekten vorgenommen werden
- Speichern von objektänderungen an die Datenbank
- Binden von Objekten im Arbeitsspeicher, an der UI-Steuerelemente

Diese Seite bietet hilfreiche Informationen zur Verwaltung der Context-Klasse.  

## <a name="defining-a-dbcontext-derived-class"></a>Definieren einer Klasse "DbContext" abgeleitet  

Die empfohlene Methode zum Arbeiten mit Kontext ist eine Klasse definiert, die von "DbContext" abgeleitet und stellt "DbSet"-Eigenschaften, die Auflistungen der angegebenen Entitäten im Kontext darstellen. Wenn Sie mit dem EF Designer arbeiten, wird der Kontext für Sie generiert. Wenn Sie mit Code First arbeiten, werden Sie in der Regel den Kontext selbst schreiben.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Nachdem Sie einen Kontext haben, würden Sie für Abfragen, hinzufügen (mithilfe von `Add` oder `Attach` Methoden) oder zu entfernen (mit `Remove`) Entitäten im Zusammenhang mit diesen Eigenschaften. Zugreifen auf eine `DbSet` Eigenschaft auf ein Context-Objekt darstellt, eine Startabfrage, die alle Entitäten des angegebenen Typs zurückgibt. Beachten Sie, dass nur Zugriff auf eine Eigenschaft der Abfrage nicht ausgeführt werden soll. Abfrage wird ausgeführt, wenn:  

- Sie wird durch eine `foreach` (C#)-Anweisung oder eine `For Each` (Visual Basic)-Anweisung aufgezählt.  
- Es aufgezählt wird durch einen Auflistungsvorgang z. B. `ToArray`, `ToDictionary`, oder `ToList`.  
- LINQ-Operatoren, z. B. `First` oder `Any` im äußersten Teil der Abfrage angegeben sind.  
- Eine der folgenden Methoden aufgerufen werden: die `Load` Erweiterungsmethode `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, und `DbSet<T>.Find`, wenn eine Entität mit dem angegebenen Schlüssel nicht bereits geladen, im Kontext gefunden wird.  

## <a name="lifetime"></a>Lebensdauer  

Die Lebensdauer des Kontexts beginnt, wenn die Instanz erstellt wird, und endet, wenn die Instanz freigegeben oder Garbage Collection durchgeführt wird. Verwendung **mit** sollten Sie alle Ressourcen, die vom Kontext gesteuerten am Ende des Blocks freigegeben werden. Bei Verwendung von **mit**, erstellt der Compiler automatisch einen Try/finally-Block und ruft dispose im der **schließlich** Block.  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

Hier sind einige allgemeinen Richtlinien, bei der Entscheidung über die Lebensdauer des Kontexts:  

- Verwenden Sie bei der Arbeit mit Webanwendungen eine Context-Instanz pro Anforderung.  
- Verwenden Sie bei der Arbeit mit Windows Presentation Foundation (WPF) oder Windows Forms eine Kontextinstanz pro Formular. Dadurch können Sie die änderungsnachverfolgung zurückgreifen, Kontext bereitstellt.  
- Wenn die Context-Instanz, die von einem Dependency Injection-Container erstellt wird, ist es in der Regel die Verantwortung für den Container, den Kontext zu verwerfen.
- Wenn der Kontext im Anwendungscode erstellt wird, müssen Sie Kontext freigeben, wenn es nicht mehr benötigt wird.  
- Beim Arbeiten mit lang andauernden Kontext Folgendes:  
    - Wenn Sie weitere Objekte und ihre Verweise in den Arbeitsspeicher laden, kann die arbeitsspeichernutzung der den Kontext schnell erhöhen. Dies kann zu Leistungseinbußen führen.  
    - Der Kontext ist nicht threadsicher, sollte daher nicht über mehrere Threads gleichzeitig Arbeit für sie freigegeben werden.
    - Wenn eine Ausnahme bewirkt, den Kontext dass, der nicht wiederhergestellt werden, kann die gesamte Anwendung beendet.  
    - Je größer der zeitliche Abstand zwischen der Abfrage der Daten und ihrer Aktualisierung, desto höher ist die Wahrscheinlichkeit, dass Parallelitätsprobleme auftreten.  

## <a name="connections"></a>Verbindungen  

Standardmäßig wird der Kontext Verbindungen mit der Datenbank verwaltet. Der Kontext wird geöffnet und schließt Verbindungen nach Bedarf. Beispielsweise wird der Kontext öffnet eine Verbindung zum Ausführen einer Abfrage und schließt dann die Verbindung aus, wenn alle Resultsets verarbeitet wurden.  

In bestimmten Fällen ist es erforderlich, den Zeitpunkt für das Öffnen und Schließen der Verbindung genauer zu steuern. Beispielsweise wird bei der Arbeit mit SQL Server Compact oft empfohlen, eine separate offene Verbindung zur Datenbank für die Lebensdauer der Anwendung zur Verbesserung der Leistung zu verwalten. Mit der `Connection`-Eigenschaft können Sie diesen Vorgang manuell steuern.  
