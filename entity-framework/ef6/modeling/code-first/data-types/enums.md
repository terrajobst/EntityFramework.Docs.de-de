---
title: Unterstützung von Enumerationen – Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 986991c2dd9470867aaf5424ecb176d7db172426
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489504"
---
# <a name="enum-support---code-first"></a>Unterstützung von Enumerationen – Code First
> [!NOTE]
> **EF5 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 5 eingeführt wurden. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.

Dieses video und schrittweise exemplarische Vorgehensweise zeigt die Verwendung von Enumerationstypen mit Entity Framework Code First. Es wird veranschaulicht, wie Enumerationen in einer LINQ-Abfrage verwendet wird.

In dieser exemplarischen Vorgehensweise wird Code First verwenden, um eine neue Datenbank zu erstellen, aber Sie können auch [Code First mit einer vorhandenen Datenbank zuordnen](~/ef6/modeling/code-first/workflows/existing-database.md).

Enum-Unterstützung wurde in Entity Framework 5 eingeführt. Um die neuen Features wie Enumerationen, räumliche Datentypen und Tabellenwertfunktionen zu verwenden, müssen Sie .NET Framework 4.5 ausrichten. Visual Studio 2012 ist standardmäßig die Zielversion .NET 4.5.

Im Entity Framework kann eine Enumeration der folgenden zugrunde liegende Typen aufweisen: **Byte**, **Int16**, **Int32**, **Int64** , oder **SByte**.

## <a name="watch-the-video"></a>Video ansehen
Dieses Video zeigt, wie Sie Enum-Typen mit Entity Framework Code First zu verwenden. Es wird veranschaulicht, wie Enumerationen in einer LINQ-Abfrage verwendet wird.

**Präsentiert von**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Voraussetzungen

Sie müssen Visual Studio 2012, Ultimate, Premium, Professional oder Web Express Edition installiert, um diese exemplarische Vorgehensweise abgeschlossen haben.

 

## <a name="set-up-the-project"></a>Einrichten des Projekts

1.  Visual Studio 2012 öffnen
2.  Auf der **Datei** Startmenü **neu**, und klicken Sie dann auf **Projekt**
3.  Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage
4.  Geben Sie **EnumCodeFirst** als Namen für das Projekt und auf **OK**

## <a name="define-a-new-model-using-code-first"></a>Definieren Sie ein neues Modell mit Code First

Bei Verwendung von Code First-Entwicklung beginnen Sie in der Regel durch das Schreiben von .NET Framework-Klassen, die das konzeptionelle (Domäne)-Modell zu definieren. Der folgende Code definiert die Department-Klasse.

Der Code definiert außerdem die DepartmentNames-Enumeration. Die Enumeration wird standardmäßig der **Int** Typ. Die Name-Eigenschaft für die Abteilung-Klasse ist die DepartmentNames-Typs.

Öffnen Sie die Datei "Program.cs" ein, und fügen Sie die folgenden Klassendefinitionen.

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a>Definieren Sie die "DbContext" abgeleiteten Typ

Zusätzlich zum Definieren von Entitäten, müssen Sie eine Klasse definieren, die von "DbContext" abgeleitet und stellt "DbSet"&lt;TEntity&gt; Eigenschaften. "DbSet"&lt;TEntity&gt; Eigenschaften können Sie den Kontext, die wissen, welche Typen im Modell enthalten sein sollen.

Eine Instanz des Typs "DbContext" abgeleitet verwaltet die Entitätsobjekte während der Laufzeit, einschließlich ausfüllenden Objekten mit Daten aus einer Datenbank, verfolgen und Speichern von Daten in der Datenbank zu ändern.

Die "DbContext" und "DbSet"-Typen werden in der EntityFramework-Assembly definiert. Es wird einen Verweis auf diese DLL-Datei mit EntityFramework NuGet-Paket hinzufügen.

1.  Im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen.
2.  Wählen Sie **NuGet-Pakete verwalten...**
3.  Wählen Sie in das Dialogfeld "NuGet-Pakete verwalten" die **Online** Registerkarte, und wählen Sie die **EntityFramework** Paket.
4.  Klicken Sie auf **installieren**

Beachten Sie, dass zusätzlich zu den EntityFramework-Assembly, sowie Verweise auf Assemblys System.ComponentModel.DataAnnotations und System.Data.Entity hinzugefügt werden.

Fügen Sie am Anfang der Datei "Program.cs" die folgenden using-Anweisung:

``` csharp
using System.Data.Entity;
```

Fügen Sie die Kontextdefinition in "Program.cs" hinzu. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Speichern und Abrufen von Daten

Öffnen Sie die Datei "Program.cs", die, in die Main-Methode definiert ist. Fügen Sie der Main-Funktion mit den folgenden Code. Der Code Fügt ein neues Objekt für die Abteilung, in den Kontext. Klicken Sie dann die Daten gespeichert. Der Code führt auch eine LINQ-Abfrage, die eine Abteilung zurückgibt, wobei der Name DepartmentNames.English ist.

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Kompilieren Sie die Anwendung, und führen Sie sie aus. Das Programm erzeugt die folgende Ausgabe:

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Anzeigen der generierten Datenbank.

Wenn Sie die Anwendung zum ersten Mal ausführen, erstellt Entity Framework eine Datenbank für Sie. Da wir Visual Studio 2012 installiert haben, wird die Datenbank für die LocalDB-Instanz erstellt werden. In der Standardeinstellung benennt Entity Framework die Datenbank nach der vollqualifizierte Name des abgeleiteten Kontexts (in diesem Beispiel, das **EnumCodeFirst.EnumTestContext**). Die nachfolgende, wie oft die vorhandene Datenbank verwendet wird.  

Beachten Sie, dass wenn Sie Änderungen an Ihrem Modell vornehmen, nachdem die Datenbank erstellt wurde, sollten Sie Code First-Migrationen verwenden, um das Datenbankschema aktualisieren. Finden Sie unter [Code First in eine neue Datenbank](~/ef6/modeling/code-first/workflows/new-database.md) ein Beispiel für die Verwendung von Migrationen.

Um die Datenbank und die Daten anzuzeigen, führen Sie folgende Schritte aus:

1.  Wählen Sie im Hauptmenü von Visual Studio 2012 **Ansicht**  - &gt; **Objekt-Explorer von SQL Server**.
2.  Wenn LocalDB nicht in der Liste der Server ist, klicken Sie auf die rechten Maustaste **SQL Server** , und wählen Sie **SQL Server hinzufügen** verwenden Sie die Standardeinstellung **Windows-Authentifizierung** zum Herstellen einer Verbindung mit der LocalDB-Instanz
3.  Erweitern Sie den Knoten für LocalDB
4.  Erweitern der **Datenbanken** Ordner finden Sie unter der neuen Datenbank aus, und navigieren Sie zu der **Abteilung** Tabelle Beachten Sie, dass, die Code First keine Tabelle erstellt wird, der den Enumerationstyp zugeordnet
5.  Daten anzeigen, mit der rechten Maustaste auf die Tabelle aus, und wählen Sie **Anzeigedaten**

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise erläutert, wie Sie Enum-Typen mit Entity Framework Code First zu verwenden. 
