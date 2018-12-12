---
title: Tools und Erweiterungen – EF Core
author: ErikEJ
ms.date: 07/03/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 67eae6cb943b974cc9cd581b8054836d2e37b1e9
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53181993"
---
# <a name="ef-core-tools--extensions"></a>EF Core Tools und -Erweiterungen

Tools und Erweiterungen bieten zusätzliche Funktionen für Entity Framework Core.

> [!IMPORTANT]  
> Erweiterungen werden durch eine Vielzahl von Quellen erstellt und nicht als Teil des Entity Framework Core-Projekts verwaltet. Wenn Sie die Erweiterung eines Drittanbieters in Betracht ziehen, sollten Sie Qualität, Lizenzierung, Kompatibilität, Support usw. auswerten, um sicherzustellen, dass diese Ihren Anforderungen entspricht.

## <a name="tools"></a>Tools

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro ist eine Entitätsmodelllösung, die Entity Framework und Entity Framework Core unterstützt. Sie können problemlos ihr Entitätsmodell definieren und es Ihrer Datenbank zuordnen, indem Sie Database First oder Model First verwenden, sodass Sie sofort mit dem Schreiben von Abfragen beginnen können.

[Website](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer ist ein leistungsstarker ORM-Designer für ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access und LINQ to SQL. Sie können Model First- und Database First-Ansätze zum Entwerfen Ihres ORM-Modells verwenden und einen C#- oder Visual Basic .NET-Code für sie generieren. Es werden neue Ansätze für ORM-Modelle eingeführt, die Produktivität wird gesteigert, und die Entwicklung von Datenbankanwendungen wird erleichtert.

[Website](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

Erweiterung von Visual Studio 2017 und höher Sie können DbContext- und POCO-Klassen einer existierenden Datenbank oder eines SQL Server-Datenbankprojekts zurückentwickeln und DbContext auf verschiedene Art und Weise visualisieren und überprüfen.

[GitHub-Wiki](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

### <a name="entity-framework-visual-editor"></a>Visueller Editor für Entity Framework

Eine Erweiterung für Visual Studio 2017, die einen ORM-Designer für das visuelle Design von Entity Framework 6-, Core 2.0- und Core 2.1-Klassen bereit. Code wird mithilfe von T4-Vorlagen generiert und kann somit vollständig an die Anforderungen des Benutzers angepasst werden. Vererbung, uni- und bidirektionale Zuordnungen, Enumerationen, Farbcode für Klassen und das Hinzufügen von Textblöcken für Erklärungen zu möglicherweise schwer durchschaubaren Bereichen Ihres Designs werden unterstützt.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory ist eine Gerüstbau-Engine für .NET Core und Entity Framework Core. CatFactory exportiert vorhandene Datenbanken aus einer SQL Server-Instanz und erstellt mithilfe der Darstellung in Datenbankmodellen das Gerüst u.a. für Entitäten, Konfigurationen und Repositorys.

[GitHub-Repository](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Entity Framework Core-Generator von LoreSoft

Der Entity Framework Core-Generator ist ein .NET Core-CLI-Tool, das EF Core-Modelle aus vorhandenen Datenbanken generieren kann, ähnlich wie `dotnet ef dbcontext scaffold`. Im Gegensatz zu diesem Befehl unterstützt es jedoch die [erneute Generierung](https://efg.loresoft.com/en/latest/regeneration/) sicheren Codes. Die erneute Generierung kann durch das Ersetzen eines Bereichs oder durch das Analysieren von Zuordnungsdateien erfolgen. Das Tool unterstützt außerdem das Generieren von Ansichtsmodellen, die Validierung und Objektzuordnungscode. Weitere Informationen finden Sie unter den unten stehenden Links zu einem Tutorial und zur Dokumentation.

[Tutorial](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[Dokumentation](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Erweiterungen

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Ein Plug-In für Microsoft.EntityFrameworkCore zur Unterstützung der automatischen Aufzeichnung des Verlauf von Datenänderungen.

[GitHub-Repository](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Dynamic Linq-Erweiterungen für Microsoft.EntityFrameworkCore, das asynchrone Unterstützung hinzufügt.

 [GitHub-Repository](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Versuchen Sie gute oder bewährte Methoden in einer Tests unterstützenden API zu erfassen, einschließlich eines kleinen Frameworks für die Suche nach N+1-Abfragen.

[GitHub-Repository](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Zwischenspeicherungsbibliothek zweiter Ebene Das Zwischenspeichern zweiter Ebene ist ein Abfragecache. Die Ergebnisse von EF-Befehlen werden im Cache gespeichert, sodass die gleichen EF-Befehle ihre Daten eher aus dem Cache abrufen, als die Datenbank erneut zu durchsuchen.

[GitHub-Repository](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Lädt und speichert die gesamten getrennten Entitätsdiagramme (die Entität mit ihren untergeordneten Entitäten und Listen). Inspiriert von [GraphDiff](https://github.com/refactorthis/GraphDiff/). Es sollen einige Plug-Ins hinzugefügt werden, um sich wiederholende Aufgaben wie eine Überprüfung oder Paginierung zu vereinfachen.

[GitHub-Repository](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Den Primärschlüssel (inklusive zusammengesetzter Schlüssel) einer Entität als Wörterbuch abrufen.

[GitHub-Repository](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Reaktive Erweiterungswrapper für heiße Observables von Entity Framework-Entitäten.

[GitHub-Repository](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Fügen Sie Ihren Entitäten mit Einfügen-, Aktualisieren-, und Löschereignissen Trigger hinzu. Für jeden gibt es drei Ereignisse: vor, nach und bei Fehler.

[GitHub-Repository](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Erhalten Sie typisierten Zugriff auf den OriginalValue Ihrer Entitätseigenschaften. Im Gegensatz zu Navigation und Auflistungen werden einfache und komplexe Eigenschaften unterstützt.

[GitHub-Repository](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco stellt einen Reversemodellgenerator bereit, der sowohl die Pluralisierung und Singularisierung, als auch bearbeitbare Vorlagen unterstützt, die auf von C# 6.0 interpolierten Zeichenfolgen basieren und unter .Net Core ausgeführt werden. Es stellt auch einen Seed-Skript-Generator mit SQL-Mergeskripten und einem Skript Runner zur Verfügung.

[GitHub-Repository](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore sind Erweiterungen für Hauptbenutzer von LINQ to SQL und EntityFrameworkCore. Mit Include(...)- und IDbAsync-Support

[GitHub-Repository](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore stellt hilfreiche Erweiterungen für die Verwendung von LINQ-Anbietern, wie z.B. Entity Framework, bereit, die nur eine geringe Teilmenge von .NET-Funktionen unterstützen, Funktionen wiederverwenden, Abfragen neu schreiben, sie sogar NULL-sicher machen und mithilfe von übersetzbaren Prädikaten und Selektoren dynamische Abfragen erstellen.

[GitHub-Repository](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Ein Plug-In für Microsoft.EntityFrameworkCore zur Unterstützung von Repositorys, Arbeitseinheitenmustern und mehreren Datenbanken, die verteilte Transaktionen unterstützen.

[GitHub-Repository](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Lazy Loading für EF Core 1.1

[GitHub-Repository](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

EntityFrameworkCore-Erweiterungen für Massenvorgänge (Einfügen, Aktualisieren, Löschen)

[GitHub-Repository](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Fügt EF Core die Pluralisierung zur Entwurfszeit hinzu.

[GitHub-Repository](https://github.com/bricelam/EFCore.Pluralizer)
