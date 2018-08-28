---
title: Ein Upgrade auf Entitätsframework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 8317fc0dcd5a7b176f3100e449bc6ff708bd310f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997370"
---
# <a name="upgrading-to-entity-framework-6"></a>Ein Upgrade auf Entitätsframework 6

In früheren Versionen von EF war der Code in Core-Bibliotheken (in erster Linie in "System.Data.Entity.dll"), geliefert als Bestandteil von .NET Framework und Out-of-Band (OOB) Bibliotheken (in erster Linie EntityFramework.dll) in ein NuGet-Paket ausgeliefert unterteilt. EF6 nimmt den Code aus den Core-Bibliotheken und integriert sie in den OOB-Bibliotheken. Dies war erforderlich, damit EF die open-Source vorgenommen werden kann und er sein eigenes Lerntempo von .NET Framework entwickeln können. Die Folge davon ist, dass Anwendungen für die verschobenen Typen neu erstellt werden müssen.

Dies sollte für Anwendungen einfach sein, die von "DbContext" verwenden als in EF 4.1 und höher geliefert. Ein wenig mehr Aufwand ist erforderlich, für Anwendungen, die von ObjectContext nutzen, jedoch ist es immer noch nicht schwierig.

Hier ist eine Prüfliste der Dinge, die Sie zum Aktualisieren einer vorhandenen Anwendung an ef6 darstellen müssen.

## <a name="1-install-the-ef6-nuget-package"></a>1. Installieren Sie das EF6-NuGet-Paket

Sie müssen auf der neuen Entity Framework 6-Laufzeit zu aktualisieren.

1. Mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **NuGet-Pakete verwalten...**  
2. Unter den **Online** wählen Sie Registerkarte **EntityFramework** , und klicken Sie auf **installieren**  
   > [!NOTE]
   > Wenn eine frühere Version des EntityFramework NuGet-Paket installiert wurde wird dies in EF6 aktualisiert.

Alternativ können Sie den folgenden Befehl aus Paket-Manager-Konsole ausführen:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Stellen Sie sicher, dass Assemblyverweise "System.Data.Entity.dll" entfernt werden

Das EF6-NuGet-Paket installieren, sollten alle Verweise auf System.Data.Entity aus Ihrem Projekt automatisch für Sie entfernen.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. Austauschen von EF-Designer (EDMX) Modelle zur Verwendung von EF 6.x-codegenerierung

Wenn Sie alle Modelle, die mit dem EF Designer erstellt haben, müssen Sie so aktualisieren Sie den Code Generation Vorlagen um kompatible EF6-Code zu generieren.

> [!NOTE]
> Es stehen derzeit nur Vorlagen mit EF 6.x DbContext Generator für Visual Studio 2012 und 2013.

1. Löschen Sie vorhandenen Code generieren-Vorlagen. Diese Dateien werden im Regelfall  **\<Edmx_file_name\>TT** und  **\<Edmx_file_name\>. Context.tt** und geschachtelt unter Ihrer Edmx-Datei im Projektmappen-Explorer. Sie können die Vorlagen auswählen, im Projektmappen-Explorer, und drücken Sie die **Del** Taste, um sie zu löschen.  
   > [!NOTE]
   > In Websiteprojekte werden die Vorlagen nicht geschachtelt unter der Edmx-Datei, sondern zusammen mit der sie im Projektmappen-Explorer aufgeführt.  

   > [!NOTE]
   > In VB.NET-Projekten müssen Sie zum Aktivieren von "Alle Dateien anzeigen", um die geschachtelte Vorlagendateien angezeigt werden.
2. Fügen Sie die entsprechende EF 6.x Code Generation Vorlage hinzu. Öffnen des Modells in der EF-Designer, mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Codegenerierungselement hinzufügen...**
    - Bei Verwendung der DbContext-API (empfohlen) klicken Sie dann **EF 6.x DbContext Generator** stehen unterhalb der **Daten** Registerkarte.  
      > [!NOTE]
      > Wenn Sie Visual Studio 2012 verwenden, müssen Sie die Entity Framework 6-Tools für diese Vorlage installieren. Finden Sie unter [Entity Framework erhalten](~/ef6/fundamentals/install.md) Details.  

    - Wenn Sie die ObjectContext-API verwenden Sie benötigen, wählen Sie die **Online** Registerkarte und suchen Sie nach **EF 6.x EntityObject Generator**.  
3. Wenn Sie alle Anpassungen angewendet haben den Code-Generierung-Vorlagen müssen Sie sie erneut auf den aktualisierten Vorlagen anzuwenden.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. Aktualisieren Sie die Namespaces für alle Core EF-Typen, die verwendet wird

Die Namespaces für "DbContext" und "Code First-Typen wurden nicht geändert werden. Dies bedeutet, dass für viele Anwendungen, mit denen EF 4.1 oder höher werden nicht müssen Sie nichts ändern.

Typen wie ObjectContext, die früher in "System.Data.Entity.dll" wurden in neue Namespaces verschoben. Dies bedeutet, Sie müssen möglicherweise Aktualisieren Ihrer *mit* oder *Import* Direktiven für EF6 den Build.

Die allgemeine Regel zum Namespace-Änderungen ist, dass für jeden Typ im System.Data.* in System.Data.Entity.Core.* verschoben wird. Das heißt, fügen Sie einfach **Entity.Core.** nach dem "System.Data". Zum Beispiel:

- System.Data.EntityException = > "System.Data". **Entity.Core.** EntityException  
- System.Data.Objects.ObjectContext = > "System.Data". **Entity.Core.** Objects.ObjectContext  
- System.Data.Objects.DataClasses.RelationshipManager = > "System.Data". **Entity.Core.** Objects.DataClasses.RelationshipManager  

Diese Typen sind der *Core* Namespaces, da sie nicht direkt für die meisten "DbContext"-basierten Anwendungen verwendet werden. Einige Typen, die Teil von "System.Data.Entity.dll" waren häufig und direkt noch für "DbContext"-basierten Anwendungen verwendet werden und daher nicht verschoben wurden in die *Core* Namespaces. Diese lauten wie folgt:

- System.Data.EntityState = > "System.Data". **Entität.** "EntityState"  
- System.Data.Objects.DataClasses.EdmFunctionAttribute = > "System.Data". **Entity.DbFunctionAttribute**  
  > [!NOTE]
  > Diese Klasse wurde umbenannt; eine Klasse mit dem alten Namen noch vorhanden ist und funktioniert, aber es nun als veraltet markiert.  
- System.Data.Objects.EntityFunctions = > "System.Data". **Entity.DbFunctions**  
  > [!NOTE]
  > Diese Klasse wurde umbenannt; eine Klasse mit dem alten Namen noch vorhanden ist und funktioniert, aber es nun als veraltet markiert.)  
- Räumliche Klassen (z. B. DbGeography, DbGeometry) wurden verschoben aus System.Data.Spatial = > "System.Data". **Entität.** Räumliche

> [!NOTE]
> Einige Typen in der System.Data-Namespace sind in "System.Data.dll", die keiner EF-Assembly ist. Diese Typen nicht verschoben haben und deren Namespaces bleiben daher unverändert.
