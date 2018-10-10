---
title: Entity Framework-Glossar – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
ms.openlocfilehash: 9ed7a2d841c625de35de57edb4e57e69b89a3db9
ms.sourcegitcommit: 5d74ac575f813110db6d870720f50dd7606446bc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48881266"
---
# <a name="entity-framework-glossary"></a>Entity Framework – Glossar
## <a name="code-first"></a>Code First
Erstellen eines Entity Framework-Datenmodells mithilfe von Code. Das Modell kann als Ziel und vorhandene oder eine neue Datenbank.

## <a name="context"></a>Kontext
Eine Klasse darstellt, die eine Sitzung mit der Datenbank, sodass Sie Abfragen und Speichern von Daten. Ein Kontext wird von der "DbContext" bzw. ObjectContext-Klasse abgeleitet.

## <a name="convention-code-first"></a>Konvention (Code First)
Eine Regel, die Entity Framework verwendet wird, um die Form des Modells aus Ihren Klassen abzuleiten.

## <a name="database-first"></a>Zunächst-Datenbank
Erstellen ein Entity Framework-Datenmodell, das mit dem EF-Designer, die eine vorhandene Datenbank ausgerichtet ist.

## <a name="eager-loading"></a>Eager Loading
Ein Muster laden zugehöriger Daten, in dem eine Abfrage für einen Entitätstyp auch verknüpfte Entitäten als Teil der Abfrage wird geladen.

## <a name="ef-designer"></a>EF-Designer
Ein visueller Designer in Visual Studio, die Sie erstellen ein Entity Framework-Datenmodell, das mithilfe der Felder und Linien können.

## <a name="entity"></a>Entität
Eine Klasse oder ein Objekt, die oder das Anwendungsdaten darstellt, z. B. Kunden, Bestellungen und Produkte.

## <a name="entity-data-model"></a>Entity Data Model
Ein Modell, das Entitäten und die Beziehungen zwischen ihnen beschrieben werden. EF verwendet EDM, um das konzeptionelle Modell für das Beschreiben der Developer-Programmen. EDM baut auf dem Entitätsbeziehungsmodell von Dr. eingeführt. Peter Chen. Das EDM wurde ursprünglich mit der primäre Zweck von zu common Data Model für eine Suite von Entwickler- und servertechnologien von Microsoft entwickelt. EDM wird auch als Teil des OData-Protokolls verwendet.

## <a name="explicit-loading"></a>Explizites Laden
Ein Muster laden zugehöriger Daten, in dem verbundene Objekte geladen werden, indem Sie eine API aufruft.

## <a name="fluent-api"></a>Fluent-API
Eine API, die zum Konfigurieren eines Code First-Modells verwendet werden kann.

## <a name="foreign-key-association"></a>Fremdschlüsselzuordnung
Eine Zuordnung zwischen Entitäten, in denen eine Eigenschaft, die den Fremdschlüssel darstellt, in der Klasse der abhängigen Entität enthalten ist. Produkt enthält z. B. eine CategoryId-Eigenschaft.

## <a name="identifying-relationship"></a>Identifizierende Beziehung
Eine Beziehung, in der der Primärschlüssel der Prinzipalentität Teil des Primärschlüssels der abhängigen Entität ist. Bei diesem Beziehungstyp kann die abhängige Entität nicht ohne die Prinzipalentität vorhanden sein.

## <a name="independent-association"></a>Unabhängige Zuordnung
Eine Zuordnung zwischen Entitäten, es keine Eigenschaft gibt, die den Fremdschlüssel in der Klasse der abhängigen Entität darstellt. Beispielsweise enthält eine Product-Klasse eine Beziehung zur aber keine CategoryId-Eigenschaft. Entitätsframework verfolgt den Status der Zuordnung unabhängig von den Zustand der Entitäten an die beiden Zuordnungsenden.

## <a name="lazy-loading"></a>Lazy Loading
Ein Muster zum Laden von verknüpften Daten, in denen verknüpfte Objekte automatisch geladen werden, wenn eine Navigationseigenschaft zugegriffen wird.

## <a name="model-first"></a>Model First
Erstellen ein Entity Framework-Datenmodell, das mit dem EF-Designer wird, klicken Sie dann zum Erstellen einer neuen Datenbank.

## <a name="navigation-property"></a>Navigationseigenschaft
Eine Eigenschaft einer Entität, die auf eine andere Entität verweist. Z. B. Produkt enthält eine Navigationseigenschaft für die Kategorie und Kategorie enthält eine Navigationseigenschaft für Produkte.

## <a name="poco"></a>POCO
Abkürzung für einfache alte CLR-Objekt. Eine einfache Klasse, die keine Abhängigkeiten mit jeden Framework enthält. Im Kontext von Entity Framework, einer Entitätsklasse, die nicht von "EntityObject" abgeleitet ist, alle Schnittstellen implementiert oder enthält alle Attribute in EF definiert. Solche Entitätsklassen, die von der persistenzframework entkoppelt sind werden auch als "Persistenzignoranz".  

## <a name="relationship-inverse"></a>Beziehung inverse
Die entgegengesetzten Ende einer Beziehung, z. B. Produkt. Kategorie "und" Kategorie ". Das Produkt.

## <a name="self-tracking-entity"></a>Entität mit selbstnachverfolgung
Eine Entität aus einer Vorlage für die codegenerierung, die mit N-Tier-Entwicklung unterstützen erstellt.

## <a name="table-per-concrete-type-tpc"></a>Tabelle pro konkretem Typ (TPC)
Eine Methode der Zuordnung der Vererbung, wobei jeder nicht abstrakten Typ in der Hierarchie zugeordnet ist, auf die um Tabelle in der Datenbank zu trennen.

## <a name="table-per-hierarchy-tph"></a>Tabelle pro Hierarchie (TPH)
Eine Methode der Zuordnung der Vererbung, in dem alle Typen in der Hierarchie auf dieselbe Tabelle in der Datenbank zugeordnet sind. Diskriminator Spalte(n) wird verwendet, um zu identifizieren, welche Art von jeder Zeile zugeordnet ist.

## <a name="table-per-type-tpt"></a>Tabelle pro Typ (TPT)
Eine Methode der Zuordnung der Vererbung, in dem die allgemeinen Eigenschaften aller Typen in der Hierarchie auf dieselbe Tabelle in der Datenbank zugeordnet sind, aber die eindeutige Eigenschaften für jeden Typ in eine separate Tabelle zugeordnet sind.

## <a name="type-discovery"></a>Typermittlung
Der Prozess zum Identifizieren der Typen, die ein Entity Framework-Modell enthalten sein soll.
