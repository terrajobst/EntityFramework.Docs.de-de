---
title: Räumliche - Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: b8f2485a7002dcb4079f4045432f3224123fdb2f
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283601"
---
# <a name="spatial---code-first"></a>Räumliche - Code First
> [!NOTE]
> **EF5 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 5 eingeführt wurden. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.

Die Video- und schrittweise exemplarische Vorgehensweise veranschaulicht das Zuordnen von räumlichen Typen mit Entity Framework Code First. Darüber hinaus veranschaulicht eine LINQ-Abfrage verwenden, um einen Abstand zwischen zwei Orten zu finden.

In dieser exemplarischen Vorgehensweise wird Code First verwenden, um eine neue Datenbank zu erstellen, aber Sie können auch [Code First mit einer vorhandenen Datenbank](~/ef6/modeling/code-first/workflows/existing-database.md).

Unterstützung von räumlichen Typen wurde in Entity Framework 5 eingeführt. Beachten Sie, um die neuen Features wie räumlichen Typen, Enumerationen und Tabellenwertfunktionen zu verwenden, Sie .NET Framework 4.5 als Ziel haben müssen. Visual Studio 2012 ist standardmäßig die Zielversion .NET 4.5.

Verwendung der Typen von räumlichen Daten müssen Sie auch einen Entity Framework-Anbieter verwenden, der Unterstützung von räumlichen Daten verfügt. Finden Sie unter [anbieterunterstützung für räumliche Typen](~/ef6/fundamentals/providers/spatial-support.md) für Weitere Informationen.

Es gibt zwei Arten von main räumliche Daten: Geography und Geometry. Der Geography-Datentyp speichert ellipsenförmige Daten (z. B. GPS-Breiten- und Längengrad koordiniert). Der Geometry-Datentyp stellt euklidischen (flachen) Koordinatensystem dar.

## <a name="watch-the-video"></a>Video ansehen
Dieses Video zeigt, wie räumliche Typen mit Entity Framework Code First zugeordnet wird. Darüber hinaus veranschaulicht eine LINQ-Abfrage verwenden, um einen Abstand zwischen zwei Orten zu finden.

**Präsentiert von**: Julia Kornich

**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Voraussetzungen

Sie müssen Visual Studio 2012, Ultimate, Premium, Professional oder Web Express Edition installiert, um diese exemplarische Vorgehensweise abgeschlossen haben.

## <a name="set-up-the-project"></a>Einrichten des Projekts

1.  Visual Studio 2012 öffnen
2.  Auf der **Datei** Startmenü **neu**, und klicken Sie dann auf **Projekt**
3.  Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage
4.  Geben Sie **SpatialCodeFirst** als Namen für das Projekt und auf **OK**

## <a name="define-a-new-model-using-code-first"></a>Definieren Sie ein neues Modell mit Code First

Bei Verwendung von Code First-Entwicklung beginnen Sie in der Regel durch das Schreiben von .NET Framework-Klassen, die das konzeptionelle (Domäne)-Modell zu definieren. Der folgende Code definiert die University-Klasse.

Die Universität hat die Location-Eigenschaft des Typs DbGeography. Um den Typ der DbGeography verwenden zu können, müssen Sie fügen einen Verweis auf die Assembly System.Data.Entity und auch die System.Data.Spatial using-Anweisung.

Öffnen Sie die Datei "Program.cs", und fügen Sie die folgenden using-Anweisungen am Anfang der Datei:

``` csharp
using System.Data.Spatial;
```

Fügen Sie die folgende Klassendefinition in University in die Datei "Program.cs" hinzu.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
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

Beachten Sie, dass zusätzlich zu den EntityFramework-Assembly auch ein Verweis auf die System.ComponentModel.DataAnnotations-Assembly hinzugefügt wird.

Fügen Sie am Anfang der Datei "Program.cs" die folgenden using-Anweisung:

``` csharp
using System.Data.Entity;
```

Fügen Sie die Kontextdefinition in "Program.cs" hinzu. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Speichern und Abrufen von Daten

Öffnen Sie die Datei "Program.cs", die, in die Main-Methode definiert ist. Fügen Sie der Main-Funktion mit den folgenden Code.

Der Code fügt zwei neue University-Objekte in den Kontext an. Räumliche Eigenschaften werden mithilfe der Methode DbGeography.FromText initialisiert. Der Geography-Punkt dargestellt, wie WellKnownText an die Methode übergeben wird. Der Code speichert dann die Daten. Klicken Sie dann die LINQ-Abfrage, die ein University-Objekt zurückgegeben, bei denen am Speicherort am nächsten am angegebenen Speicherort erstellt und ausgeführt wird.

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Kompilieren Sie die Anwendung, und führen Sie sie aus. Das Programm erzeugt die folgende Ausgabe:

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Anzeigen der generierten Datenbank.

Wenn Sie die Anwendung zum ersten Mal ausführen, erstellt Entity Framework eine Datenbank für Sie. Da wir Visual Studio 2012 installiert haben, wird die Datenbank für die LocalDB-Instanz erstellt werden. In der Standardeinstellung benennt Entity Framework die Datenbank nach der vollqualifizierte Name des abgeleiteten Kontexts (in diesem Beispiel **SpatialCodeFirst.UniversityContext**). Die nachfolgende, wie oft die vorhandene Datenbank verwendet wird.  

Beachten Sie, dass wenn Sie Änderungen an Ihrem Modell vornehmen, nachdem die Datenbank erstellt wurde, sollten Sie Code First-Migrationen verwenden, um das Datenbankschema aktualisieren. Finden Sie unter [Code First in eine neue Datenbank](~/ef6/modeling/code-first/workflows/new-database.md) ein Beispiel für die Verwendung von Migrationen.

Um die Datenbank und die Daten anzuzeigen, führen Sie folgende Schritte aus:

1.  Wählen Sie im Hauptmenü von Visual Studio 2012 **Ansicht**  - &gt; **Objekt-Explorer von SQL Server**.
2.  Wenn LocalDB nicht in der Liste der Server ist, klicken Sie auf die rechten Maustaste **SQL Server** , und wählen Sie **SQL Server hinzufügen** verwenden Sie die Standardeinstellung **Windows-Authentifizierung** zum Herstellen einer Verbindung mit der die LocalDB-Instanz
3.  Erweitern Sie den Knoten für LocalDB
4.  Erweitern der **Datenbanken** Ordner finden Sie unter der neuen Datenbank aus, und navigieren Sie zu der **Universitäten** Tabelle
5.  Daten anzeigen, mit der rechten Maustaste auf die Tabelle aus, und wählen Sie **Anzeigedaten**

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise erläutert, wie Sie räumliche Typen mit Entity Framework Code First zu verwenden. 
