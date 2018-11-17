---
title: 'Räumliche Daten: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cf488c6b7d94ca19018efe1c23ff410fe7eb594b
ms.sourcegitcommit: 81c53ac43d8f15b900f117294ec71dc49fe028fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2018
ms.locfileid: "51817909"
---
# <a name="spatial-data"></a>Räumliche Daten

> [!NOTE]
> Dieses Feature ist neu in EF Core 2.2.

Räumliche Daten stellt den physischen Speicherort und die Form der Objekte dar. Viele Datenbanken bieten Unterstützung für diese Art von Daten, sodass er indiziert und zusammen mit anderen Daten abgefragt werden kann. Allgemeine Szenarien umfassen das Abfragen von Objekten innerhalb einer bestimmten Entfernung von einem Speicherort, oder wählen Sie das Objekt, dessen Rahmen mit eine angegebenen Position enthält. EF Core unterstützt die Zuordnung zu Typen von räumlichen Daten mit der [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) räumliche Bibliothek.

## <a name="installing"></a>Installation

Um räumliche Daten mit EF Core verwenden zu können, müssen Sie die entsprechende Unterstützung NuGet-Paket zu installieren. Welches Paket Sie installieren müssen, hängt von der Anbieter, den Sie verwenden.

EF Core-Datenbankanbieter                        | Räumliche NuGet-Paket
--------------------------------------- | ---------------------
"Microsoft.entityframeworkcore.SqlServer" | [Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
"Microsoft.entityframeworkcore.SQLite"    | [Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
"Microsoft.entityframeworkcore.InMemory"  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
"Npgsql.entityframeworkcore.PostgreSQL"   | [Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Reverse-engineering

Die räumliche NuGet-Pakete auch aktivieren [reverse-Engineering](../managing-schemas/scaffolding.md) Modelle mit räumlichen Eigenschaften, aber Sie müssen zum Installieren des Pakets ***vor*** ausführen `Scaffold-DbContext` oder `dotnet ef dbcontext scaffold`. Wenn Sie dies nicht tun, erhalten Sie Warnungen zum Suchen nach nicht-typzuordnungen für die Spalten und die Spalten übersprungen werden.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite ist eine räumliche Bibliothek für .NET. EF Core ermöglicht Zuordnungstypen auf räumliche Daten in der Datenbank mit NTS-Typen in Ihrem Modell.

Rufen Sie zum Aktivieren der Zuordnung für räumliche Typen über NTS die UseNetTopologySuite-Methode des Anbieters "DbContext"-Optionen-Generator. Beispielsweise würden mit SQL Server Sie ihn wie folgt aufrufen.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Es gibt mehrere Typen von räumlichen Daten. Der, den Sie verwenden, hängt von der Arten von Formen, die Sie zulassen möchten. Hier ist die Hierarchie der NTS-Typen, die Sie für Eigenschaften in Ihrem Modell verwenden können. Sie sind in der `NetTopologySuite.Geometries` Namespace. Entsprechende Schnittstellen im Paket GeoAPI (`GeoAPI.Geometries` Namespace) kann auch verwendet werden.

* Geometry
  * Punkt
  * LineString
  * Polygon
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve und CurePolygon werden von NTS nicht unterstützt.

Verwenden die Basis-Geometry-Datentyp kann jedem Typ der Form, die von der Eigenschaft angegeben werden.

Die folgenden Entitätsklassen verwendet werden, um Tabellen im Zuordnen der [Wide World Importers-Beispieldatenbank](http://go.microsoft.com/fwlink/?LinkID=800630).

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public IPoint Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public IGeometry Border { get; set; }
}
```

### <a name="creating-values"></a>Erstellen von Werten

Sie können Konstruktoren verwenden, um die Geometry-Objekten zu erstellen; jedoch empfiehlt NTS, verwenden Sie stattdessen eine Geometry-Factory. Dies ermöglicht die Angabe einer Standard-SRID (spatial Referenzsystem verwendet wird, durch die Koordinaten) und bietet Ihnen Kontrolle über noch auf Erweiterte Funktionen wie die Genauigkeit-Modell (während Berechnungen verwendet) und die Koordinate Sequenz (wird bestimmt, welche die Koordinaten – Dimensionen und Maßeinheiten an – Vorhandensein verfügbar sind).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 bezieht sich auf WGS-84, ein Standard, GPS und anderen geografischen Systemen verwendet.

### <a name="longitude-and-latitude"></a>Längen- und Breitengrad

Koordinaten in NTS sind hinsichtlich der X- und Y-Werte. Um Längen- und Breitengrad darzustellen, verwenden Sie X für Längen- und Y für die Breite an. Beachten Sie, dass dies **rückwärts** aus der `latitude, longitude` Format in der Sie diese Werte in der Regel angezeigt.

### <a name="srid-ignored-during-client-operations"></a>SRID ignoriert wird, während der Clientvorgänge

NTS ignoriert SRID-Werten während des Betriebs. Es wird angenommen, ein planare Koordinatensystem. Dies bedeutet, dass bei Angabe von Koordinaten in Bezug auf die Längen- und Breitengrad, einige Werte Client ausgewertet, z.B. Entfernung, Länge und Bereich in Grad, nicht die Zähler werden. Für sinnvolleren Werten, müssen Sie zuerst die Koordinaten für ein anderes Koordinatensystem, die mithilfe einer Bibliothek wie Projekt [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) vor der Berechnung dieser Werte.

Wenn ein Vorgang vom Entity Framework Core mithilfe von SQL Server-ausgewertet wird, wird das Ergebnis der Einheit von der Datenbank bestimmt.

Hier ist ein Beispiel der Verwendung von ProjNet4GeoAPI, der Berechnung der Entfernung zwischen zwei Orten.

``` csharp
static class GeometryExtensions
{
    static readonly IGeometryServices _geometryServices = NtsGeometryServices.Instance;
    static readonly ICoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                // (3857 and 4326 included automatically)

                // This coordinate system covers the area of our data.
                // Different data requires a different coordinate system.
                [2855] =
                @"
                    PROJCS[""NAD83(HARN) / Washington North"",
                        GEOGCS[""NAD83(HARN)"",
                            DATUM[""NAD83_High_Accuracy_Regional_Network"",
                                SPHEROID[""GRS 1980"",6378137,298.257222101,
                                    AUTHORITY[""EPSG"",""7019""]],
                                AUTHORITY[""EPSG"",""6152""]],
                            PRIMEM[""Greenwich"",0,
                                AUTHORITY[""EPSG"",""8901""]],
                            UNIT[""degree"",0.01745329251994328,
                                AUTHORITY[""EPSG"",""9122""]],
                            AUTHORITY[""EPSG"",""4152""]],
                        PROJECTION[""Lambert_Conformal_Conic_2SP""],
                        PARAMETER[""standard_parallel_1"",48.73333333333333],
                        PARAMETER[""standard_parallel_2"",47.5],
                        PARAMETER[""latitude_of_origin"",47],
                        PARAMETER[""central_meridian"",-120.8333333333333],
                        PARAMETER[""false_easting"",500000],
                        PARAMETER[""false_northing"",0],
                        UNIT[""metre"",1,
                            AUTHORITY[""EPSG"",""9001""]],
                        AUTHORITY[""EPSG"",""2855""]]
                "
            });

    public static IGeometry ProjectTo(this IGeometry geometry, int srid)
    {
        var geometryFactory = _geometryServices.CreateGeometryFactory(srid);
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        return GeometryTransform.TransformGeometry(
            geometryFactory,
            geometry,
            transformation.MathTransform);
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>Abfrage von Daten

In LINQ werden NTS-Methoden und Eigenschaften verfügbar, wie Datenbankfunktionen in SQL übersetzt werden. Beispielsweise werden die Entfernung und Contains-Methode in den folgenden Abfragen übersetzt. Die Tabelle am Ende dieses Artikels zeigt an, welche Elemente von verschiedenen Anbietern für EF Core unterstützt werden.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

Wenn Sie SQL Server verwenden, gibt es jedoch einige zusätzlichen Aufgaben, denen Sie kennen sollten.

### <a name="geography-or-geometry"></a>Geografie oder Geometrie

In der Standardeinstellung räumliche Eigenschaften zugeordnet werden `geography` Spalten in SQL Server. Verwendung von `geometry`, [konfigurieren Sie den Spaltentyp](xref:core/modeling/relational/data-types) in Ihrem Modell.

### <a name="geography-polygon-rings"></a>Geography-Polygon-Ringe

Bei Verwendung der `geography` Spaltentyp, SQL Server gelten zusätzliche Anforderungen für den äußeren Ring (oder die Shell) und inneren Ringe (oder Lücken). Der äußere Ring muss gegen den Uhrzeigersinn ausgerichtet sein, und das innere Ringe im Uhrzeigersinn. NTS überprüft diese vor dem Senden der Werte in der Datenbank.

### <a name="fullglobe"></a>FullGlobe

SQL Server hat einen nicht standardmäßigen Geometry-Datentyp, um die vollständige Kugel darstellen, bei Verwendung der `geography` Spaltentyp. Sie hat außerdem eine Methode zur Darstellung von Polygonen, die basierend auf die vollständige Kugel (ohne einen äußeren Ring). Beide Methoden werden von NTS unterstützt.

> [!WARNING]
> FullGlobe und Polygone, die darauf basierend werden von NTS nicht unterstützt.

## <a name="sqlite"></a>SQLite

Hier ist einige zusätzliche Informationen für Benutzer mithilfe von SQLite.

### <a name="installing-spatialite"></a>Installieren von SpatiaLite

Auf Windows wird die native Mod_spatialite-Bibliothek als eine NuGet-Paket-Abhängigkeit bereitgestellt. Andere Plattformen müssen separat installieren. Dies erfolgt normalerweise über ein Software-Paket-Manager. Beispielsweise können Sie APT unter Ubuntu und Homebrew unter MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>Konfigurieren von SRID

In SpatiaLite müssen Spalten, an dem SRID pro Spalte. Die Standard-SRID ist `0`. Geben Sie an eine andere SRID, die mit der ForSqliteHasSrid-Methode.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Dimension

SRID ähnlich, ist einer Spaltenwerts in der Dimension (oder die Koordinaten) auch als Teil der Spalte angegeben. Der Standardwert die Koordinaten sind X und y. Aktivieren Sie zusätzliche Koordinaten ("Z" und "M") mithilfe der ForSqliteHasDimension-Methode.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Übersetzte Vorgänge

Diese Tabelle zeigt, welche Member NTS von den einzelnen Anbietern für EF Core in SQL übersetzt werden.

NetTopologySuite | SQL Server (Geometrie) | SQL Server (Geography) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometry.Area | ✔ | ✔ | ✔ | ✔
Geometry.AsBinary() | ✔ | ✔ | ✔ | ✔
Geometry.AsText() | ✔ | ✔ | ✔ | ✔
Geometry.Boundary | ✔ | | ✔ | ✔
Geometry.Buffer(double) | ✔ | ✔ | ✔ | ✔
Geometry.Buffer (double, Int) | | | ✔
Geometry.Centroid | ✔ | | ✔ | ✔
Geometry.Contains(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ConvexHull() | ✔ | ✔ | ✔ | ✔
Geometry.CoveredBy(Geometry) | | | ✔ | ✔
Geometry.Covers(Geometry) | | | ✔ | ✔
Geometry.Crosses(Geometry) | ✔ | | ✔ | ✔
Geometry.Difference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Dimension | ✔ | ✔ | ✔ | ✔
Geometry.Disjoint(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Distance(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Envelope | ✔ | | ✔ | ✔
Geometry.EqualsExact(Geometry) | | | | ✔
Geometry.EqualsTopologically(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.GeometryType | ✔ | ✔ | ✔ | ✔
Geometry.GetGeometryN(int) | ✔ | | ✔ | ✔
Geometry.InteriorPoint | ✔ | | ✔
Geometry.Intersection(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Intersects(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry.IsSimple | ✔ | | ✔ | ✔
Geometry.IsValid | ✔ | ✔ | ✔ | ✔
Geometry.IsWithinDistance (Geometry, double) | ✔ | | ✔
Geometry.Length | ✔ | ✔ | ✔ | ✔
Geometry.NumGeometries | ✔ | ✔ | ✔ | ✔
Geometry.NumPoints | ✔ | ✔ | ✔ | ✔
Geometry.OgcGeometryType | ✔ | ✔ | ✔
Geometry.Overlaps(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.PointOnSurface | ✔ | | ✔ | ✔
Geometry.Relate (Geometry, Zeichenfolge) | ✔ | | ✔ | ✔
Geometry.Reverse() | | | ✔ | ✔
Geometry.SRID | ✔ | ✔ | ✔ | ✔
Geometry.SymmetricDifference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ToBinary() | ✔ | ✔ | ✔ | ✔
Geometry.ToText() | ✔ | ✔ | ✔ | ✔
Geometry.Touches(Geometry) | ✔ | | ✔ | ✔
Geometry.Union() | | | ✔
Geometry.Union(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Within(Geometry) | ✔ | ✔ | ✔ | ✔
GeometryCollection.Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [Int] | ✔ | ✔ | ✔ | ✔
LineString.Count | ✔ | ✔ | ✔ | ✔
LineString.EndPoint | ✔ | ✔ | ✔ | ✔
LineString.GetPointN(int) | ✔ | ✔ | ✔ | ✔
LineString.IsClosed | ✔ | ✔ | ✔ | ✔
LineString.IsRing | ✔ | | ✔ | ✔
LineString.StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString.IsClosed | ✔ | ✔ | ✔ | ✔
Point.M | ✔ | ✔ | ✔ | ✔
Point.X | ✔ | ✔ | ✔ | ✔
Point.Y | ✔ | ✔ | ✔ | ✔
Point.Z | ✔ | ✔ | ✔ | ✔
Polygon.ExteriorRing | ✔ | ✔ | ✔ | ✔
Polygon.GetInteriorRingN(int) | ✔ | ✔ | ✔ | ✔
Polygon.NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Räumliche Daten in SQLServer](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [SpatiaLite-Homepage](https://www.gaia-gis.it/fossil/libspatialite)
* [Räumliche Npgsql-Dokumentation](http://www.npgsql.org/efcore/mapping/nts.html)
* [PostGIS-Dokumentation](http://postgis.net/documentation/)
