---
title: 'Räumliche Daten: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 49c18758af2f2383ea082ead2f6df4c022152b36
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688786"
---
# <a name="spatial-data"></a><span data-ttu-id="cbc8f-102">Räumliche Daten</span><span class="sxs-lookup"><span data-stu-id="cbc8f-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="cbc8f-103">Dieses Feature ist neu in EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="cbc8f-104">Räumliche Daten stellt den physischen Speicherort und die Form der Objekte dar.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="cbc8f-105">Viele Datenbanken bieten Unterstützung für diese Art von Daten, sodass er indiziert und zusammen mit anderen Daten abgefragt werden kann.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="cbc8f-106">Allgemeine Szenarien umfassen das Abfragen von Objekten innerhalb einer bestimmten Entfernung von einem Speicherort, oder wählen Sie das Objekt, dessen Rahmen mit eine angegebenen Position enthält.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="cbc8f-107">EF Core unterstützt die Zuordnung zu Typen von räumlichen Daten mit der [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) räumliche Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="cbc8f-108">Installation</span><span class="sxs-lookup"><span data-stu-id="cbc8f-108">Installing</span></span>

<span data-ttu-id="cbc8f-109">Um räumliche Daten mit EF Core verwenden zu können, müssen Sie die entsprechende Unterstützung NuGet-Paket zu installieren.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="cbc8f-110">Welches Paket Sie installieren müssen, hängt von der Anbieter, den Sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="cbc8f-111">EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="cbc8f-111">EF Core Provider</span></span>                        | <span data-ttu-id="cbc8f-112">Räumliche NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="cbc8f-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="cbc8f-113">"Microsoft.entityframeworkcore.SqlServer"</span><span class="sxs-lookup"><span data-stu-id="cbc8f-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="cbc8f-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="cbc8f-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="cbc8f-115">"Microsoft.entityframeworkcore.SQLite"</span><span class="sxs-lookup"><span data-stu-id="cbc8f-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="cbc8f-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="cbc8f-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="cbc8f-117">"Microsoft.entityframeworkcore.InMemory"</span><span class="sxs-lookup"><span data-stu-id="cbc8f-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="cbc8f-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="cbc8f-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="cbc8f-119">"Npgsql.entityframeworkcore.PostgreSQL"</span><span class="sxs-lookup"><span data-stu-id="cbc8f-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="cbc8f-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="cbc8f-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="cbc8f-121">Reverse-engineering</span><span class="sxs-lookup"><span data-stu-id="cbc8f-121">Reverse engineering</span></span>

<span data-ttu-id="cbc8f-122">Die räumliche NuGet-Pakete auch aktivieren [reverse-Engineering](../managing-schemas/scaffolding.md) Modelle mit räumlichen Eigenschaften, aber Sie müssen zum Installieren des Pakets ***vor*** ausführen `Scaffold-DbContext` oder `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="cbc8f-123">Wenn Sie dies nicht tun, erhalten Sie Warnungen zum Suchen nach nicht-typzuordnungen für die Spalten und die Spalten übersprungen werden.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="cbc8f-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="cbc8f-125">NetTopologySuite ist eine räumliche Bibliothek für .NET.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="cbc8f-126">EF Core ermöglicht Zuordnungstypen auf räumliche Daten in der Datenbank mit NTS-Typen in Ihrem Modell.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="cbc8f-127">Rufen Sie zum Aktivieren der Zuordnung für räumliche Typen über NTS die UseNetTopologySuite-Methode des Anbieters "DbContext"-Optionen-Generator.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="cbc8f-128">Beispielsweise würden mit SQL Server Sie ihn wie folgt aufrufen.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="cbc8f-129">Es gibt mehrere Typen von räumlichen Daten.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-129">There are several spatial data types.</span></span> <span data-ttu-id="cbc8f-130">Der, den Sie verwenden, hängt von der Arten von Formen, die Sie zulassen möchten.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="cbc8f-131">Hier ist die Hierarchie der NTS-Typen, die Sie für Eigenschaften in Ihrem Modell verwenden können.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="cbc8f-132">Sie sind in der `NetTopologySuite.Geometries` Namespace.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span> <span data-ttu-id="cbc8f-133">Entsprechende Schnittstellen im Paket GeoAPI (`GeoAPI.Geometries` Namespace) kann auch verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-133">Corresponding interfaces in the GeoAPI package (`GeoAPI.Geometries` namespace) can also be used.</span></span>

* <span data-ttu-id="cbc8f-134">Geometry</span><span class="sxs-lookup"><span data-stu-id="cbc8f-134">Geometry</span></span>
  * <span data-ttu-id="cbc8f-135">Punkt</span><span class="sxs-lookup"><span data-stu-id="cbc8f-135">Point</span></span>
  * <span data-ttu-id="cbc8f-136">LineString</span><span class="sxs-lookup"><span data-stu-id="cbc8f-136">LineString</span></span>
  * <span data-ttu-id="cbc8f-137">Polygon</span><span class="sxs-lookup"><span data-stu-id="cbc8f-137">Polygon</span></span>
  * <span data-ttu-id="cbc8f-138">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="cbc8f-138">GeometryCollection</span></span>
    * <span data-ttu-id="cbc8f-139">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="cbc8f-139">MultiPoint</span></span>
    * <span data-ttu-id="cbc8f-140">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="cbc8f-140">MultiLineString</span></span>
    * <span data-ttu-id="cbc8f-141">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="cbc8f-141">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="cbc8f-142">CircularString, CompoundCurve und CurePolygon werden von NTS nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-142">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="cbc8f-143">Verwenden die Basis-Geometry-Datentyp kann jedem Typ der Form, die von der Eigenschaft angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-143">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="cbc8f-144">Die folgenden Entitätsklassen verwendet werden, um Tabellen im Zuordnen der [Wide World Importers-Beispieldatenbank](http://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="cbc8f-144">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="cbc8f-145">Erstellen von Werten</span><span class="sxs-lookup"><span data-stu-id="cbc8f-145">Creating values</span></span>

<span data-ttu-id="cbc8f-146">Sie können Konstruktoren verwenden, um die Geometry-Objekten zu erstellen; jedoch empfiehlt NTS, verwenden Sie stattdessen eine Geometry-Factory.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-146">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="cbc8f-147">Dies ermöglicht die Angabe einer Standard-SRID (spatial Referenzsystem verwendet wird, durch die Koordinaten) und bietet Ihnen Kontrolle über noch auf Erweiterte Funktionen wie die Genauigkeit-Modell (während Berechnungen verwendet) und die Koordinate Sequenz (wird bestimmt, welche die Koordinaten – Dimensionen und Maßeinheiten an – Vorhandensein verfügbar sind).</span><span class="sxs-lookup"><span data-stu-id="cbc8f-147">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="cbc8f-148">4326 bezieht sich auf WGS-84, ein Standard, GPS und anderen geografischen Systemen verwendet.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-148">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="cbc8f-149">Längen- und Breitengrad</span><span class="sxs-lookup"><span data-stu-id="cbc8f-149">Longitude and Latitude</span></span>

<span data-ttu-id="cbc8f-150">Koordinaten in NTS sind hinsichtlich der X- und Y-Werte.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-150">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="cbc8f-151">Um Längen- und Breitengrad darzustellen, verwenden Sie X für Längen- und Y für die Breite an.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-151">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="cbc8f-152">Beachten Sie, dass dies **rückwärts** aus der `latitude, longitude` Format in der Sie diese Werte in der Regel angezeigt.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-152">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="cbc8f-153">SRID ignoriert wird, während der Clientvorgänge</span><span class="sxs-lookup"><span data-stu-id="cbc8f-153">SRID Ignored during client operations</span></span>

<span data-ttu-id="cbc8f-154">NTS ignoriert SRID-Werten während des Betriebs.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-154">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="cbc8f-155">Es wird angenommen, ein planare Koordinatensystem.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-155">It assumes a planar coordinate system.</span></span> <span data-ttu-id="cbc8f-156">Dies bedeutet, dass bei Angabe von Koordinaten in Bezug auf die Längen- und Breitengrad, einige Werte Client ausgewertet, z.B. Entfernung, Länge und Bereich in Grad, nicht die Zähler werden.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-156">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="cbc8f-157">Für sinnvolleren Werten, müssen Sie zuerst die Koordinaten für ein anderes Koordinatensystem, die mithilfe einer Bibliothek wie Projekt [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) vor der Berechnung dieser Werte.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-157">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="cbc8f-158">Wenn ein Vorgang vom Entity Framework Core mithilfe von SQL Server-ausgewertet wird, wird das Ergebnis der Einheit von der Datenbank bestimmt.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-158">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="cbc8f-159">Hier ist ein Beispiel der Verwendung von ProjNet4GeoAPI, der Berechnung der Entfernung zwischen zwei Orten.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-159">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="cbc8f-160">Abfrage von Daten</span><span class="sxs-lookup"><span data-stu-id="cbc8f-160">Querying Data</span></span>

<span data-ttu-id="cbc8f-161">In LINQ werden NTS-Methoden und Eigenschaften verfügbar, wie Datenbankfunktionen in SQL übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-161">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="cbc8f-162">Beispielsweise werden die Entfernung und Contains-Methode in den folgenden Abfragen übersetzt.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-162">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="cbc8f-163">Die Tabelle am Ende dieses Artikels zeigt an, welche Elemente von verschiedenen Anbietern für EF Core unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-163">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="cbc8f-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="cbc8f-164">SQL Server</span></span>

<span data-ttu-id="cbc8f-165">Wenn Sie SQL Server verwenden, gibt es jedoch einige zusätzlichen Aufgaben, denen Sie kennen sollten.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-165">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="cbc8f-166">Geografie oder Geometrie</span><span class="sxs-lookup"><span data-stu-id="cbc8f-166">Geography or geometry</span></span>

<span data-ttu-id="cbc8f-167">In der Standardeinstellung räumliche Eigenschaften zugeordnet werden `geography` Spalten in SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-167">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="cbc8f-168">Verwendung von `geometry`, [konfigurieren Sie den Spaltentyp](xref:core/modeling/relational/data-types) in Ihrem Modell.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-168">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="cbc8f-169">Geography-Polygon-Ringe</span><span class="sxs-lookup"><span data-stu-id="cbc8f-169">Geography polygon rings</span></span>

<span data-ttu-id="cbc8f-170">Bei Verwendung der `geography` Spaltentyp, SQL Server gelten zusätzliche Anforderungen für den äußeren Ring (oder die Shell) und inneren Ringe (oder Lücken).</span><span class="sxs-lookup"><span data-stu-id="cbc8f-170">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="cbc8f-171">Der äußere Ring muss gegen den Uhrzeigersinn ausgerichtet sein, und das innere Ringe im Uhrzeigersinn.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-171">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="cbc8f-172">NTS überprüft diese vor dem Senden der Werte in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-172">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="cbc8f-173">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="cbc8f-173">FullGlobe</span></span>

<span data-ttu-id="cbc8f-174">SQL Server hat einen nicht standardmäßigen Geometry-Datentyp, um die vollständige Kugel darstellen, bei Verwendung der `geography` Spaltentyp.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-174">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="cbc8f-175">Sie hat außerdem eine Methode zur Darstellung von Polygonen, die basierend auf die vollständige Kugel (ohne einen äußeren Ring).</span><span class="sxs-lookup"><span data-stu-id="cbc8f-175">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="cbc8f-176">Beide Methoden werden von NTS unterstützt.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-176">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="cbc8f-177">FullGlobe und Polygone, die darauf basierend werden von NTS nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-177">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="cbc8f-178">SQLite</span><span class="sxs-lookup"><span data-stu-id="cbc8f-178">SQLite</span></span>

<span data-ttu-id="cbc8f-179">Hier ist einige zusätzliche Informationen für Benutzer mithilfe von SQLite.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-179">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="cbc8f-180">Installieren von SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="cbc8f-180">Installing SpatiaLite</span></span>

<span data-ttu-id="cbc8f-181">Auf Windows wird die native Mod_spatialite-Bibliothek als eine NuGet-Paket-Abhängigkeit bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-181">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="cbc8f-182">Andere Plattformen müssen separat installieren.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-182">Other platforms need to install it separately.</span></span> <span data-ttu-id="cbc8f-183">Dies erfolgt normalerweise über ein Software-Paket-Manager.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-183">This is typically done using a software package manager.</span></span> <span data-ttu-id="cbc8f-184">Beispielsweise können Sie APT unter Ubuntu und Homebrew unter MacOS.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-184">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="cbc8f-185">Konfigurieren von SRID</span><span class="sxs-lookup"><span data-stu-id="cbc8f-185">Configuring SRID</span></span>

<span data-ttu-id="cbc8f-186">In SpatiaLite müssen Spalten, an dem SRID pro Spalte.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-186">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="cbc8f-187">Die Standard-SRID ist `0`.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-187">The default SRID is `0`.</span></span> <span data-ttu-id="cbc8f-188">Geben Sie an eine andere SRID, die mit der ForSqliteHasSrid-Methode.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-188">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="cbc8f-189">Dimension</span><span class="sxs-lookup"><span data-stu-id="cbc8f-189">Dimension</span></span>

<span data-ttu-id="cbc8f-190">SRID ähnlich, ist einer Spaltenwerts in der Dimension (oder die Koordinaten) auch als Teil der Spalte angegeben.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-190">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="cbc8f-191">Der Standardwert die Koordinaten sind X und y. Aktivieren Sie zusätzliche Koordinaten ("Z" und "M") mithilfe der ForSqliteHasDimension-Methode.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-191">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="cbc8f-192">Übersetzte Vorgänge</span><span class="sxs-lookup"><span data-stu-id="cbc8f-192">Translated Operations</span></span>

<span data-ttu-id="cbc8f-193">Diese Tabelle zeigt, welche Member NTS von den einzelnen Anbietern für EF Core in SQL übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="cbc8f-193">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="cbc8f-194">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="cbc8f-194">NetTopologySuite</span></span> | <span data-ttu-id="cbc8f-195">SQL Server (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-195">SQL Server (geometry)</span></span> | <span data-ttu-id="cbc8f-196">SQL Server (Geography)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-196">SQL Server (geography)</span></span> | <span data-ttu-id="cbc8f-197">SQLite</span><span class="sxs-lookup"><span data-stu-id="cbc8f-197">SQLite</span></span> | <span data-ttu-id="cbc8f-198">Npgsql</span><span class="sxs-lookup"><span data-stu-id="cbc8f-198">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="cbc8f-199">Geometry.Area</span><span class="sxs-lookup"><span data-stu-id="cbc8f-199">Geometry.Area</span></span> | <span data-ttu-id="cbc8f-200">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-200">✔</span></span> | <span data-ttu-id="cbc8f-201">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-201">✔</span></span> | <span data-ttu-id="cbc8f-202">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-202">✔</span></span> | <span data-ttu-id="cbc8f-203">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-203">✔</span></span>
<span data-ttu-id="cbc8f-204">Geometry.AsBinary()</span><span class="sxs-lookup"><span data-stu-id="cbc8f-204">Geometry.AsBinary()</span></span> | <span data-ttu-id="cbc8f-205">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-205">✔</span></span> | <span data-ttu-id="cbc8f-206">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-206">✔</span></span> | <span data-ttu-id="cbc8f-207">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-207">✔</span></span> | <span data-ttu-id="cbc8f-208">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-208">✔</span></span>
<span data-ttu-id="cbc8f-209">Geometry.AsText()</span><span class="sxs-lookup"><span data-stu-id="cbc8f-209">Geometry.AsText()</span></span> | <span data-ttu-id="cbc8f-210">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-210">✔</span></span> | <span data-ttu-id="cbc8f-211">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-211">✔</span></span> | <span data-ttu-id="cbc8f-212">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-212">✔</span></span> | <span data-ttu-id="cbc8f-213">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-213">✔</span></span>
<span data-ttu-id="cbc8f-214">Geometry.Boundary</span><span class="sxs-lookup"><span data-stu-id="cbc8f-214">Geometry.Boundary</span></span> | <span data-ttu-id="cbc8f-215">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-215">✔</span></span> | | <span data-ttu-id="cbc8f-216">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-216">✔</span></span> | <span data-ttu-id="cbc8f-217">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-217">✔</span></span>
<span data-ttu-id="cbc8f-218">Geometry.Buffer(double)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-218">Geometry.Buffer(double)</span></span> | <span data-ttu-id="cbc8f-219">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-219">✔</span></span> | <span data-ttu-id="cbc8f-220">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-220">✔</span></span> | <span data-ttu-id="cbc8f-221">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-221">✔</span></span> | <span data-ttu-id="cbc8f-222">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-222">✔</span></span>
<span data-ttu-id="cbc8f-223">Geometry.Buffer (double, Int)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-223">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="cbc8f-224">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-224">✔</span></span>
<span data-ttu-id="cbc8f-225">Geometry.Centroid</span><span class="sxs-lookup"><span data-stu-id="cbc8f-225">Geometry.Centroid</span></span> | <span data-ttu-id="cbc8f-226">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-226">✔</span></span> | | <span data-ttu-id="cbc8f-227">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-227">✔</span></span> | <span data-ttu-id="cbc8f-228">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-228">✔</span></span>
<span data-ttu-id="cbc8f-229">Geometry.Contains(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="cbc8f-230">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-230">✔</span></span> | <span data-ttu-id="cbc8f-231">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-231">✔</span></span> | <span data-ttu-id="cbc8f-232">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-232">✔</span></span> | <span data-ttu-id="cbc8f-233">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-233">✔</span></span>
<span data-ttu-id="cbc8f-234">Geometry.ConvexHull()</span><span class="sxs-lookup"><span data-stu-id="cbc8f-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="cbc8f-235">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-235">✔</span></span> | <span data-ttu-id="cbc8f-236">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-236">✔</span></span> | <span data-ttu-id="cbc8f-237">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-237">✔</span></span> | <span data-ttu-id="cbc8f-238">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-238">✔</span></span>
<span data-ttu-id="cbc8f-239">Geometry.CoveredBy(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="cbc8f-240">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-240">✔</span></span> | <span data-ttu-id="cbc8f-241">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-241">✔</span></span>
<span data-ttu-id="cbc8f-242">Geometry.Covers(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="cbc8f-243">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-243">✔</span></span> | <span data-ttu-id="cbc8f-244">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-244">✔</span></span>
<span data-ttu-id="cbc8f-245">Geometry.Crosses(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="cbc8f-246">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-246">✔</span></span> | | <span data-ttu-id="cbc8f-247">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-247">✔</span></span> | <span data-ttu-id="cbc8f-248">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-248">✔</span></span>
<span data-ttu-id="cbc8f-249">Geometry.Difference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="cbc8f-250">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-250">✔</span></span> | <span data-ttu-id="cbc8f-251">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-251">✔</span></span> | <span data-ttu-id="cbc8f-252">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-252">✔</span></span> | <span data-ttu-id="cbc8f-253">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-253">✔</span></span>
<span data-ttu-id="cbc8f-254">Geometry.Dimension</span><span class="sxs-lookup"><span data-stu-id="cbc8f-254">Geometry.Dimension</span></span> | <span data-ttu-id="cbc8f-255">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-255">✔</span></span> | <span data-ttu-id="cbc8f-256">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-256">✔</span></span> | <span data-ttu-id="cbc8f-257">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-257">✔</span></span> | <span data-ttu-id="cbc8f-258">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-258">✔</span></span>
<span data-ttu-id="cbc8f-259">Geometry.Disjoint(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="cbc8f-260">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-260">✔</span></span> | <span data-ttu-id="cbc8f-261">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-261">✔</span></span> | <span data-ttu-id="cbc8f-262">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-262">✔</span></span> | <span data-ttu-id="cbc8f-263">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-263">✔</span></span>
<span data-ttu-id="cbc8f-264">Geometry.Distance(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="cbc8f-265">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-265">✔</span></span> | <span data-ttu-id="cbc8f-266">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-266">✔</span></span> | <span data-ttu-id="cbc8f-267">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-267">✔</span></span> | <span data-ttu-id="cbc8f-268">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-268">✔</span></span>
<span data-ttu-id="cbc8f-269">Geometry.Envelope</span><span class="sxs-lookup"><span data-stu-id="cbc8f-269">Geometry.Envelope</span></span> | <span data-ttu-id="cbc8f-270">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-270">✔</span></span> | | <span data-ttu-id="cbc8f-271">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-271">✔</span></span> | <span data-ttu-id="cbc8f-272">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-272">✔</span></span>
<span data-ttu-id="cbc8f-273">Geometry.EqualsExact(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="cbc8f-274">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-274">✔</span></span>
<span data-ttu-id="cbc8f-275">Geometry.EqualsTopologically(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="cbc8f-276">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-276">✔</span></span> | <span data-ttu-id="cbc8f-277">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-277">✔</span></span> | <span data-ttu-id="cbc8f-278">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-278">✔</span></span> | <span data-ttu-id="cbc8f-279">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-279">✔</span></span>
<span data-ttu-id="cbc8f-280">Geometry.GeometryType</span><span class="sxs-lookup"><span data-stu-id="cbc8f-280">Geometry.GeometryType</span></span> | <span data-ttu-id="cbc8f-281">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-281">✔</span></span> | <span data-ttu-id="cbc8f-282">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-282">✔</span></span> | <span data-ttu-id="cbc8f-283">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-283">✔</span></span> | <span data-ttu-id="cbc8f-284">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-284">✔</span></span>
<span data-ttu-id="cbc8f-285">Geometry.GetGeometryN(int)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="cbc8f-286">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-286">✔</span></span> | | <span data-ttu-id="cbc8f-287">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-287">✔</span></span> | <span data-ttu-id="cbc8f-288">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-288">✔</span></span>
<span data-ttu-id="cbc8f-289">Geometry.InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="cbc8f-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="cbc8f-290">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-290">✔</span></span> | | <span data-ttu-id="cbc8f-291">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-291">✔</span></span>
<span data-ttu-id="cbc8f-292">Geometry.Intersection(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-292">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="cbc8f-293">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-293">✔</span></span> | <span data-ttu-id="cbc8f-294">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-294">✔</span></span> | <span data-ttu-id="cbc8f-295">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-295">✔</span></span> | <span data-ttu-id="cbc8f-296">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-296">✔</span></span>
<span data-ttu-id="cbc8f-297">Geometry.Intersects(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-297">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="cbc8f-298">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-298">✔</span></span> | <span data-ttu-id="cbc8f-299">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-299">✔</span></span> | <span data-ttu-id="cbc8f-300">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-300">✔</span></span> | <span data-ttu-id="cbc8f-301">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-301">✔</span></span>
<span data-ttu-id="cbc8f-302">Geometry.IsEmpty</span><span class="sxs-lookup"><span data-stu-id="cbc8f-302">Geometry.IsEmpty</span></span> | <span data-ttu-id="cbc8f-303">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-303">✔</span></span> | <span data-ttu-id="cbc8f-304">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-304">✔</span></span> | <span data-ttu-id="cbc8f-305">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-305">✔</span></span> | <span data-ttu-id="cbc8f-306">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-306">✔</span></span>
<span data-ttu-id="cbc8f-307">Geometry.IsSimple</span><span class="sxs-lookup"><span data-stu-id="cbc8f-307">Geometry.IsSimple</span></span> | <span data-ttu-id="cbc8f-308">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-308">✔</span></span> | | <span data-ttu-id="cbc8f-309">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-309">✔</span></span> | <span data-ttu-id="cbc8f-310">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-310">✔</span></span>
<span data-ttu-id="cbc8f-311">Geometry.IsValid</span><span class="sxs-lookup"><span data-stu-id="cbc8f-311">Geometry.IsValid</span></span> | <span data-ttu-id="cbc8f-312">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-312">✔</span></span> | <span data-ttu-id="cbc8f-313">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-313">✔</span></span> | <span data-ttu-id="cbc8f-314">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-314">✔</span></span> | <span data-ttu-id="cbc8f-315">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-315">✔</span></span>
<span data-ttu-id="cbc8f-316">Geometry.IsWithinDistance (Geometry, double)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-316">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="cbc8f-317">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-317">✔</span></span> | | <span data-ttu-id="cbc8f-318">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-318">✔</span></span>
<span data-ttu-id="cbc8f-319">Geometry.Length</span><span class="sxs-lookup"><span data-stu-id="cbc8f-319">Geometry.Length</span></span> | <span data-ttu-id="cbc8f-320">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-320">✔</span></span> | <span data-ttu-id="cbc8f-321">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-321">✔</span></span> | <span data-ttu-id="cbc8f-322">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-322">✔</span></span> | <span data-ttu-id="cbc8f-323">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-323">✔</span></span>
<span data-ttu-id="cbc8f-324">Geometry.NumGeometries</span><span class="sxs-lookup"><span data-stu-id="cbc8f-324">Geometry.NumGeometries</span></span> | <span data-ttu-id="cbc8f-325">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-325">✔</span></span> | <span data-ttu-id="cbc8f-326">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-326">✔</span></span> | <span data-ttu-id="cbc8f-327">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-327">✔</span></span> | <span data-ttu-id="cbc8f-328">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-328">✔</span></span>
<span data-ttu-id="cbc8f-329">Geometry.NumPoints</span><span class="sxs-lookup"><span data-stu-id="cbc8f-329">Geometry.NumPoints</span></span> | <span data-ttu-id="cbc8f-330">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-330">✔</span></span> | <span data-ttu-id="cbc8f-331">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-331">✔</span></span> | <span data-ttu-id="cbc8f-332">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-332">✔</span></span> | <span data-ttu-id="cbc8f-333">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-333">✔</span></span>
<span data-ttu-id="cbc8f-334">Geometry.OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="cbc8f-334">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="cbc8f-335">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-335">✔</span></span> | <span data-ttu-id="cbc8f-336">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-336">✔</span></span> | <span data-ttu-id="cbc8f-337">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-337">✔</span></span>
<span data-ttu-id="cbc8f-338">Geometry.Overlaps(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-338">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="cbc8f-339">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-339">✔</span></span> | <span data-ttu-id="cbc8f-340">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-340">✔</span></span> | <span data-ttu-id="cbc8f-341">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-341">✔</span></span> | <span data-ttu-id="cbc8f-342">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-342">✔</span></span>
<span data-ttu-id="cbc8f-343">Geometry.PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="cbc8f-343">Geometry.PointOnSurface</span></span> | <span data-ttu-id="cbc8f-344">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-344">✔</span></span> | | <span data-ttu-id="cbc8f-345">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-345">✔</span></span> | <span data-ttu-id="cbc8f-346">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-346">✔</span></span>
<span data-ttu-id="cbc8f-347">Geometry.Relate (Geometry, Zeichenfolge)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-347">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="cbc8f-348">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-348">✔</span></span> | | <span data-ttu-id="cbc8f-349">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-349">✔</span></span> | <span data-ttu-id="cbc8f-350">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-350">✔</span></span>
<span data-ttu-id="cbc8f-351">Geometry.Reverse()</span><span class="sxs-lookup"><span data-stu-id="cbc8f-351">Geometry.Reverse()</span></span> | | | <span data-ttu-id="cbc8f-352">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-352">✔</span></span> | <span data-ttu-id="cbc8f-353">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-353">✔</span></span>
<span data-ttu-id="cbc8f-354">Geometry.SRID</span><span class="sxs-lookup"><span data-stu-id="cbc8f-354">Geometry.SRID</span></span> | <span data-ttu-id="cbc8f-355">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-355">✔</span></span> | <span data-ttu-id="cbc8f-356">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-356">✔</span></span> | <span data-ttu-id="cbc8f-357">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-357">✔</span></span> | <span data-ttu-id="cbc8f-358">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-358">✔</span></span>
<span data-ttu-id="cbc8f-359">Geometry.SymmetricDifference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-359">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="cbc8f-360">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-360">✔</span></span> | <span data-ttu-id="cbc8f-361">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-361">✔</span></span> | <span data-ttu-id="cbc8f-362">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-362">✔</span></span> | <span data-ttu-id="cbc8f-363">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-363">✔</span></span>
<span data-ttu-id="cbc8f-364">Geometry.ToBinary()</span><span class="sxs-lookup"><span data-stu-id="cbc8f-364">Geometry.ToBinary()</span></span> | <span data-ttu-id="cbc8f-365">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-365">✔</span></span> | <span data-ttu-id="cbc8f-366">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-366">✔</span></span> | <span data-ttu-id="cbc8f-367">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-367">✔</span></span> | <span data-ttu-id="cbc8f-368">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-368">✔</span></span>
<span data-ttu-id="cbc8f-369">Geometry.ToText()</span><span class="sxs-lookup"><span data-stu-id="cbc8f-369">Geometry.ToText()</span></span> | <span data-ttu-id="cbc8f-370">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-370">✔</span></span> | <span data-ttu-id="cbc8f-371">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-371">✔</span></span> | <span data-ttu-id="cbc8f-372">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-372">✔</span></span> | <span data-ttu-id="cbc8f-373">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-373">✔</span></span>
<span data-ttu-id="cbc8f-374">Geometry.Touches(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-374">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="cbc8f-375">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-375">✔</span></span> | | <span data-ttu-id="cbc8f-376">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-376">✔</span></span> | <span data-ttu-id="cbc8f-377">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-377">✔</span></span>
<span data-ttu-id="cbc8f-378">Geometry.Union()</span><span class="sxs-lookup"><span data-stu-id="cbc8f-378">Geometry.Union()</span></span> | | | <span data-ttu-id="cbc8f-379">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-379">✔</span></span>
<span data-ttu-id="cbc8f-380">Geometry.Union(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-380">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="cbc8f-381">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-381">✔</span></span> | <span data-ttu-id="cbc8f-382">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-382">✔</span></span> | <span data-ttu-id="cbc8f-383">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-383">✔</span></span> | <span data-ttu-id="cbc8f-384">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-384">✔</span></span>
<span data-ttu-id="cbc8f-385">Geometry.Within(Geometry)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-385">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="cbc8f-386">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-386">✔</span></span> | <span data-ttu-id="cbc8f-387">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-387">✔</span></span> | <span data-ttu-id="cbc8f-388">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-388">✔</span></span> | <span data-ttu-id="cbc8f-389">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-389">✔</span></span>
<span data-ttu-id="cbc8f-390">GeometryCollection.Count</span><span class="sxs-lookup"><span data-stu-id="cbc8f-390">GeometryCollection.Count</span></span> | <span data-ttu-id="cbc8f-391">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-391">✔</span></span> | <span data-ttu-id="cbc8f-392">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-392">✔</span></span> | <span data-ttu-id="cbc8f-393">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-393">✔</span></span> | <span data-ttu-id="cbc8f-394">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-394">✔</span></span>
<span data-ttu-id="cbc8f-395">GeometryCollection [Int]</span><span class="sxs-lookup"><span data-stu-id="cbc8f-395">GeometryCollection[int]</span></span> | <span data-ttu-id="cbc8f-396">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-396">✔</span></span> | <span data-ttu-id="cbc8f-397">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-397">✔</span></span> | <span data-ttu-id="cbc8f-398">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-398">✔</span></span> | <span data-ttu-id="cbc8f-399">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-399">✔</span></span>
<span data-ttu-id="cbc8f-400">LineString.Count</span><span class="sxs-lookup"><span data-stu-id="cbc8f-400">LineString.Count</span></span> | <span data-ttu-id="cbc8f-401">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-401">✔</span></span> | <span data-ttu-id="cbc8f-402">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-402">✔</span></span> | <span data-ttu-id="cbc8f-403">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-403">✔</span></span> | <span data-ttu-id="cbc8f-404">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-404">✔</span></span>
<span data-ttu-id="cbc8f-405">LineString.EndPoint</span><span class="sxs-lookup"><span data-stu-id="cbc8f-405">LineString.EndPoint</span></span> | <span data-ttu-id="cbc8f-406">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-406">✔</span></span> | <span data-ttu-id="cbc8f-407">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-407">✔</span></span> | <span data-ttu-id="cbc8f-408">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-408">✔</span></span> | <span data-ttu-id="cbc8f-409">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-409">✔</span></span>
<span data-ttu-id="cbc8f-410">LineString.GetPointN(int)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-410">LineString.GetPointN(int)</span></span> | <span data-ttu-id="cbc8f-411">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-411">✔</span></span> | <span data-ttu-id="cbc8f-412">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-412">✔</span></span> | <span data-ttu-id="cbc8f-413">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-413">✔</span></span> | <span data-ttu-id="cbc8f-414">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-414">✔</span></span>
<span data-ttu-id="cbc8f-415">LineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="cbc8f-415">LineString.IsClosed</span></span> | <span data-ttu-id="cbc8f-416">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-416">✔</span></span> | <span data-ttu-id="cbc8f-417">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-417">✔</span></span> | <span data-ttu-id="cbc8f-418">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-418">✔</span></span> | <span data-ttu-id="cbc8f-419">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-419">✔</span></span>
<span data-ttu-id="cbc8f-420">LineString.IsRing</span><span class="sxs-lookup"><span data-stu-id="cbc8f-420">LineString.IsRing</span></span> | <span data-ttu-id="cbc8f-421">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-421">✔</span></span> | | <span data-ttu-id="cbc8f-422">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-422">✔</span></span> | <span data-ttu-id="cbc8f-423">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-423">✔</span></span>
<span data-ttu-id="cbc8f-424">LineString.StartPoint</span><span class="sxs-lookup"><span data-stu-id="cbc8f-424">LineString.StartPoint</span></span> | <span data-ttu-id="cbc8f-425">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-425">✔</span></span> | <span data-ttu-id="cbc8f-426">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-426">✔</span></span> | <span data-ttu-id="cbc8f-427">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-427">✔</span></span> | <span data-ttu-id="cbc8f-428">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-428">✔</span></span>
<span data-ttu-id="cbc8f-429">MultiLineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="cbc8f-429">MultiLineString.IsClosed</span></span> | <span data-ttu-id="cbc8f-430">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-430">✔</span></span> | <span data-ttu-id="cbc8f-431">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-431">✔</span></span> | <span data-ttu-id="cbc8f-432">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-432">✔</span></span> | <span data-ttu-id="cbc8f-433">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-433">✔</span></span>
<span data-ttu-id="cbc8f-434">Point.M</span><span class="sxs-lookup"><span data-stu-id="cbc8f-434">Point.M</span></span> | <span data-ttu-id="cbc8f-435">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-435">✔</span></span> | <span data-ttu-id="cbc8f-436">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-436">✔</span></span> | <span data-ttu-id="cbc8f-437">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-437">✔</span></span> | <span data-ttu-id="cbc8f-438">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-438">✔</span></span>
<span data-ttu-id="cbc8f-439">Point.X</span><span class="sxs-lookup"><span data-stu-id="cbc8f-439">Point.X</span></span> | <span data-ttu-id="cbc8f-440">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-440">✔</span></span> | <span data-ttu-id="cbc8f-441">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-441">✔</span></span> | <span data-ttu-id="cbc8f-442">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-442">✔</span></span> | <span data-ttu-id="cbc8f-443">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-443">✔</span></span>
<span data-ttu-id="cbc8f-444">Point.Y</span><span class="sxs-lookup"><span data-stu-id="cbc8f-444">Point.Y</span></span> | <span data-ttu-id="cbc8f-445">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-445">✔</span></span> | <span data-ttu-id="cbc8f-446">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-446">✔</span></span> | <span data-ttu-id="cbc8f-447">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-447">✔</span></span> | <span data-ttu-id="cbc8f-448">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-448">✔</span></span>
<span data-ttu-id="cbc8f-449">Point.Z</span><span class="sxs-lookup"><span data-stu-id="cbc8f-449">Point.Z</span></span> | <span data-ttu-id="cbc8f-450">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-450">✔</span></span> | <span data-ttu-id="cbc8f-451">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-451">✔</span></span> | <span data-ttu-id="cbc8f-452">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-452">✔</span></span> | <span data-ttu-id="cbc8f-453">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-453">✔</span></span>
<span data-ttu-id="cbc8f-454">Polygon.ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="cbc8f-454">Polygon.ExteriorRing</span></span> | <span data-ttu-id="cbc8f-455">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-455">✔</span></span> | <span data-ttu-id="cbc8f-456">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-456">✔</span></span> | <span data-ttu-id="cbc8f-457">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-457">✔</span></span> | <span data-ttu-id="cbc8f-458">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-458">✔</span></span>
<span data-ttu-id="cbc8f-459">Polygon.GetInteriorRingN(int)</span><span class="sxs-lookup"><span data-stu-id="cbc8f-459">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="cbc8f-460">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-460">✔</span></span> | <span data-ttu-id="cbc8f-461">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-461">✔</span></span> | <span data-ttu-id="cbc8f-462">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-462">✔</span></span> | <span data-ttu-id="cbc8f-463">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-463">✔</span></span>
<span data-ttu-id="cbc8f-464">Polygon.NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="cbc8f-464">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="cbc8f-465">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-465">✔</span></span> | <span data-ttu-id="cbc8f-466">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-466">✔</span></span> | <span data-ttu-id="cbc8f-467">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-467">✔</span></span> | <span data-ttu-id="cbc8f-468">✔</span><span class="sxs-lookup"><span data-stu-id="cbc8f-468">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cbc8f-469">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="cbc8f-469">Additional resources</span></span>

* [<span data-ttu-id="cbc8f-470">Räumliche Daten in SQLServer</span><span class="sxs-lookup"><span data-stu-id="cbc8f-470">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="cbc8f-471">SpatiaLite-Homepage</span><span class="sxs-lookup"><span data-stu-id="cbc8f-471">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="cbc8f-472">PostGIS-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="cbc8f-472">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
