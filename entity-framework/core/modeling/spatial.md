---
title: Räumliche Daten-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 026df735473e31f1c1463c1fbc6f46c4fd6dfd4f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921733"
---
# <a name="spatial-data"></a><span data-ttu-id="ee093-102">Räumliche Daten</span><span class="sxs-lookup"><span data-stu-id="ee093-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="ee093-103">Diese Funktion wurde in EF Core 2,2 hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ee093-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="ee093-104">Räumliche Daten repräsentieren den physischen Speicherort und die Form von Objekten.</span><span class="sxs-lookup"><span data-stu-id="ee093-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="ee093-105">Viele Datenbanken unterstützen diese Art von Daten, damit Sie zusammen mit anderen Daten indiziert und abgefragt werden können.</span><span class="sxs-lookup"><span data-stu-id="ee093-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="ee093-106">Häufige Szenarien umfassen das Abfragen von Objekten in einer bestimmten Entfernung von einem Speicherort oder das Auswählen des Objekts, dessen Rahmen eine bestimmte Position enthält.</span><span class="sxs-lookup"><span data-stu-id="ee093-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="ee093-107">EF Core unterstützt die Zuordnung räumlicher Datentypen mithilfe der räumlichen Bibliothek [nettopologysuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="ee093-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="ee093-108">Installation</span><span class="sxs-lookup"><span data-stu-id="ee093-108">Installing</span></span>

<span data-ttu-id="ee093-109">Um räumliche Daten mit EF Core verwenden zu können, müssen Sie das entsprechende unterstützende nuget-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="ee093-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="ee093-110">Welches Paket installiert werden muss, hängt vom verwendeten Anbieter ab.</span><span class="sxs-lookup"><span data-stu-id="ee093-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="ee093-111">EF Core Anbieter</span><span class="sxs-lookup"><span data-stu-id="ee093-111">EF Core Provider</span></span>                        | <span data-ttu-id="ee093-112">Räumliches nuget-Paket</span><span class="sxs-lookup"><span data-stu-id="ee093-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="ee093-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="ee093-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="ee093-114">Microsoft. entityframeworkcore. SqlServer. nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="ee093-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="ee093-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="ee093-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="ee093-116">Microsoft. entityframeworkcore. sqlite. nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="ee093-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="ee093-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="ee093-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="ee093-118">Nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="ee093-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="ee093-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="ee093-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="ee093-120">Npgsql. entityframeworkcore. PostgreSQL. nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="ee093-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="ee093-121">Reverse Engineering</span><span class="sxs-lookup"><span data-stu-id="ee093-121">Reverse engineering</span></span>

<span data-ttu-id="ee093-122">Die räumlichen nuget-Pakete ermöglichen auch die [Reverse Engineering](../managing-schemas/scaffolding.md) von Modellen mit räumlichen Eigenschaften. Sie müssen das Paket jedoch vor `Scaffold-DbContext` dem `dotnet ef dbcontext scaffold`Ausführen von oder installieren.</span><span class="sxs-lookup"><span data-stu-id="ee093-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="ee093-123">Wenn Sie dies nicht tun, erhalten Sie Warnungen, wenn Sie keine Typzuordnungen für die Spalten finden und die Spalten übersprungen werden.</span><span class="sxs-lookup"><span data-stu-id="ee093-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="ee093-124">Nettopologysuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="ee093-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="ee093-125">Nettopologysuite ist eine räumliche Bibliothek für .net.</span><span class="sxs-lookup"><span data-stu-id="ee093-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="ee093-126">EF Core ermöglicht die Zuordnung von räumlichen Datentypen in der Datenbank mithilfe von NTS-Typen in Ihrem Modell.</span><span class="sxs-lookup"><span data-stu-id="ee093-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="ee093-127">Um die Zuordnung räumlicher Typen über NTS zu ermöglichen, müssen Sie die usenettopologysuite-Methode für den dbcontext Options-Generator des Anbieters abrufen.</span><span class="sxs-lookup"><span data-stu-id="ee093-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="ee093-128">Beispielsweise können Sie mit SQL Server wie folgt bezeichnen.</span><span class="sxs-lookup"><span data-stu-id="ee093-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="ee093-129">Es gibt mehrere räumliche Datentypen.</span><span class="sxs-lookup"><span data-stu-id="ee093-129">There are several spatial data types.</span></span> <span data-ttu-id="ee093-130">Welcher Typ Sie verwenden, hängt von den Formen der Formen ab, die Sie zulassen möchten.</span><span class="sxs-lookup"><span data-stu-id="ee093-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="ee093-131">Hier ist die Hierarchie der NTS-Typen, die Sie für Eigenschaften im Modell verwenden können.</span><span class="sxs-lookup"><span data-stu-id="ee093-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="ee093-132">Sie befinden sich im `NetTopologySuite.Geometries` -Namespace.</span><span class="sxs-lookup"><span data-stu-id="ee093-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="ee093-133">et</span><span class="sxs-lookup"><span data-stu-id="ee093-133">Geometry</span></span>
  * <span data-ttu-id="ee093-134">Punkt</span><span class="sxs-lookup"><span data-stu-id="ee093-134">Point</span></span>
  * <span data-ttu-id="ee093-135">LineString</span><span class="sxs-lookup"><span data-stu-id="ee093-135">LineString</span></span>
  * <span data-ttu-id="ee093-136">Polygon</span><span class="sxs-lookup"><span data-stu-id="ee093-136">Polygon</span></span>
  * <span data-ttu-id="ee093-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="ee093-137">GeometryCollection</span></span>
    * <span data-ttu-id="ee093-138">Multipoint</span><span class="sxs-lookup"><span data-stu-id="ee093-138">MultiPoint</span></span>
    * <span data-ttu-id="ee093-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="ee093-139">MultiLineString</span></span>
    * <span data-ttu-id="ee093-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="ee093-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="ee093-141">Circularstring, compoundcurve und curepolygon werden von NTS nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="ee093-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="ee093-142">Mit dem basisgeometry-Typ kann jeder Typ von Form von der-Eigenschaft angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="ee093-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="ee093-143">Die folgenden Entitäts Klassen können verwendet werden, um Tabellen in der [Beispieldatenbank Wide World Importern](http://go.microsoft.com/fwlink/?LinkID=800630)zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="ee093-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public Point Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public Geometry Border { get; set; }
}
```

### <a name="creating-values"></a><span data-ttu-id="ee093-144">Erstellen von Werten</span><span class="sxs-lookup"><span data-stu-id="ee093-144">Creating values</span></span>

<span data-ttu-id="ee093-145">Sie können Konstruktoren zum Erstellen von Geometry-Objekten verwenden. Allerdings empfiehlt es sich, stattdessen eine Geometry-Factory zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ee093-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="ee093-146">Auf diese Weise können Sie ein Standardmäßiges SRID angeben (das von den Koordinaten verwendete räumliche Verweis System) und Ihnen die Kontrolle über erweiterte Dinge wie das Genauigkeits Modell (verwendet während der Berechnungen) und die Koordinaten Sequenz (bestimmt, welche ordinates--Dimensionen und Measures--sind verfügbar).</span><span class="sxs-lookup"><span data-stu-id="ee093-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="ee093-147">4326 bezieht sich auf WGS 84, einen in GPS und anderen geografischen Systemen verwendeten Standard.</span><span class="sxs-lookup"><span data-stu-id="ee093-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="ee093-148">Längen-und Breitengrad</span><span class="sxs-lookup"><span data-stu-id="ee093-148">Longitude and Latitude</span></span>

<span data-ttu-id="ee093-149">Die Koordinaten in den NTS sind X-und Y-Werte.</span><span class="sxs-lookup"><span data-stu-id="ee093-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="ee093-150">Um Längen-und Breitengrad darzustellen, verwenden Sie X für Längengrad und Y für Breitengrad.</span><span class="sxs-lookup"><span data-stu-id="ee093-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="ee093-151">Beachten Sie, dass dies nicht das `latitude, longitude` Format hat, in dem normalerweise diese Werte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="ee093-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="ee093-152">SRID wird bei Client Vorgängen ignoriert.</span><span class="sxs-lookup"><span data-stu-id="ee093-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="ee093-153">NTS ignoriert SRID-Werte während des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="ee093-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="ee093-154">Es wird von einem planaren Koordinatensystem ausgegangen.</span><span class="sxs-lookup"><span data-stu-id="ee093-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="ee093-155">Dies bedeutet Folgendes: Wenn Sie Koordinaten in Bezug auf Längengrad und Breitengrad angeben, werden einige vom Client ausgewertete Werte wie "Distance", "length" und "Area" in Grad und nicht in "Meter" angegeben.</span><span class="sxs-lookup"><span data-stu-id="ee093-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="ee093-156">Wenn Sie aussagekräftigere Werte benötigen, müssen Sie die Koordinaten zunächst mithilfe einer Bibliothek wie [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) auf ein anderes Koordinatensystem projizieren, bevor Sie diese Werte berechnen.</span><span class="sxs-lookup"><span data-stu-id="ee093-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="ee093-157">Wenn ein Vorgang vom Server durch EF Core über SQL ausgewertet wird, wird die Einheit des Ergebnisses von der Datenbank bestimmt.</span><span class="sxs-lookup"><span data-stu-id="ee093-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="ee093-158">Im folgenden finden Sie ein Beispiel für die Verwendung von ProjNet4GeoAPI, um den Abstand zwischen zwei Städten zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="ee093-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

``` csharp
static class GeometryExtensions
{
    static readonly CoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                [4326] = GeographicCoordinateSystem.WGS84.WKT,

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

    public static Geometry ProjectTo(this Geometry geometry, int srid)
    {
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        var result = geometry.Copy();
        result.Apply(new MathTransformFilter(transformation.MathTransform));

        return result;
    }

    class MathTransformFilter : ICoordinateSequenceFilter
    {
        readonly MathTransform _transform;

        public MathTransformFilter(MathTransform transform)
            => _transform = transform;

        public bool Done => false;
        public bool GeometryChanged => true;

        public void Filter(CoordinateSequence seq, int i)
        {
            var result = _transform.Transform(
                new[]
                {
                    seq.GetOrdinate(i, Ordinate.X),
                    seq.GetOrdinate(i, Ordinate.Y)
                });
            seq.SetOrdinate(i, Ordinate.X, result[0]);
            seq.SetOrdinate(i, Ordinate.Y, result[1]);
        }
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a><span data-ttu-id="ee093-159">Abfrage von Daten</span><span class="sxs-lookup"><span data-stu-id="ee093-159">Querying Data</span></span>

<span data-ttu-id="ee093-160">In LINQ werden die NTS-Methoden und-Eigenschaften, die als Datenbankfunktionen verfügbar sind, in SQL übersetzt.</span><span class="sxs-lookup"><span data-stu-id="ee093-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="ee093-161">Beispielsweise werden die Distance-Methode und die-Methode in den folgenden Abfragen übersetzt.</span><span class="sxs-lookup"><span data-stu-id="ee093-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="ee093-162">Die Tabelle am Ende dieses Artikels zeigt, welche Member von verschiedenen EF Core Anbietern unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="ee093-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="ee093-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="ee093-163">SQL Server</span></span>

<span data-ttu-id="ee093-164">Wenn Sie SQL Server verwenden, müssen Sie einige zusätzliche Punkte beachten.</span><span class="sxs-lookup"><span data-stu-id="ee093-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="ee093-165">Geography oder Geometry</span><span class="sxs-lookup"><span data-stu-id="ee093-165">Geography or geometry</span></span>

<span data-ttu-id="ee093-166">Standardmäßig sind räumliche Eigenschaften `geography` Spalten in SQL Server zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="ee093-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="ee093-167">Konfigurieren Sie `geometry` [den Spaltentyp](xref:core/modeling/relational/data-types) in Ihrem Modell, um zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ee093-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="ee093-168">Polygon-Polygon Ringe</span><span class="sxs-lookup"><span data-stu-id="ee093-168">Geography polygon rings</span></span>

<span data-ttu-id="ee093-169">Wenn der `geography` Spaltentyp verwendet wird, werden von SQL Server zusätzliche Anforderungen an den äußeren Ring (oder die Shell) und die inneren Ringe (oder Löcher) auferlegt.</span><span class="sxs-lookup"><span data-stu-id="ee093-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="ee093-170">Der äußere Ring muss gegen den Uhrzeigersinn und die inneren Ringe im Uhrzeigersinn ausgerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="ee093-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="ee093-171">NTS überprüft dies, bevor Werte an die Datenbank gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="ee093-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="ee093-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="ee093-172">FullGlobe</span></span>

<span data-ttu-id="ee093-173">SQL Server weist einen nicht standardmäßigen Geometry-Typ auf, der den vollständigen Globus `geography` darstellt, wenn der Spaltentyp verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ee093-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="ee093-174">Außerdem bietet es eine Möglichkeit, Polygone auf der ganzen Welt (ohne äußeren Ring) darzustellen.</span><span class="sxs-lookup"><span data-stu-id="ee093-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="ee093-175">Keines dieser beiden wird von NTS unterstützt.</span><span class="sxs-lookup"><span data-stu-id="ee093-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="ee093-176">Fullglobe und Polygone, die darauf basieren, werden von NTS nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="ee093-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="ee093-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="ee093-177">SQLite</span></span>

<span data-ttu-id="ee093-178">Hier finden Sie einige zusätzliche Informationen für diejenigen, die SQLite verwenden.</span><span class="sxs-lookup"><span data-stu-id="ee093-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="ee093-179">Installieren von spatialite</span><span class="sxs-lookup"><span data-stu-id="ee093-179">Installing SpatiaLite</span></span>

<span data-ttu-id="ee093-180">Unter Windows wird die native mod_spatialite-Bibliothek als eine nuget-Paketabhängigkeit verteilt.</span><span class="sxs-lookup"><span data-stu-id="ee093-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="ee093-181">Andere Plattformen müssen Sie separat installieren.</span><span class="sxs-lookup"><span data-stu-id="ee093-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="ee093-182">Dies erfolgt in der Regel mithilfe eines Softwarepaket-Managers.</span><span class="sxs-lookup"><span data-stu-id="ee093-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="ee093-183">Beispielsweise können Sie apt unter Ubuntu und Homebrew unter MacOS verwenden.</span><span class="sxs-lookup"><span data-stu-id="ee093-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="ee093-184">Konfigurieren von SRID</span><span class="sxs-lookup"><span data-stu-id="ee093-184">Configuring SRID</span></span>

<span data-ttu-id="ee093-185">In spatialite müssen Spalten eine SRID pro Spalte angeben.</span><span class="sxs-lookup"><span data-stu-id="ee093-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="ee093-186">Der Standard-SRID `0`ist.</span><span class="sxs-lookup"><span data-stu-id="ee093-186">The default SRID is `0`.</span></span> <span data-ttu-id="ee093-187">Geben Sie einen anderen SRID mithilfe der forsqlitehassrid-Methode an.</span><span class="sxs-lookup"><span data-stu-id="ee093-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="ee093-188">Dimension</span><span class="sxs-lookup"><span data-stu-id="ee093-188">Dimension</span></span>

<span data-ttu-id="ee093-189">Ähnlich wie SRID wird auch die Dimension (oder ordinates) einer Spalte als Teil der Spalte angegeben.</span><span class="sxs-lookup"><span data-stu-id="ee093-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="ee093-190">Die Standard Ordinate sind X und Y. Aktivieren Sie zusätzliche Ordinate (Z und M) mithilfe der forsqlitehasdimension-Methode.</span><span class="sxs-lookup"><span data-stu-id="ee093-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="ee093-191">Übersetzte Vorgänge</span><span class="sxs-lookup"><span data-stu-id="ee093-191">Translated Operations</span></span>

<span data-ttu-id="ee093-192">Diese Tabelle zeigt, welche NTS-Elemente von den einzelnen EF Core Anbietern in SQL übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="ee093-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="ee093-193">Nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="ee093-193">NetTopologySuite</span></span> | <span data-ttu-id="ee093-194">SQL Server (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-194">SQL Server (geometry)</span></span> | <span data-ttu-id="ee093-195">SQL Server (Geografie)</span><span class="sxs-lookup"><span data-stu-id="ee093-195">SQL Server (geography)</span></span> | <span data-ttu-id="ee093-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="ee093-196">SQLite</span></span> | <span data-ttu-id="ee093-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="ee093-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="ee093-198">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="ee093-198">Geometry.Area</span></span> | <span data-ttu-id="ee093-199">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-199">✔</span></span> | <span data-ttu-id="ee093-200">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-200">✔</span></span> | <span data-ttu-id="ee093-201">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-201">✔</span></span> | <span data-ttu-id="ee093-202">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-202">✔</span></span>
<span data-ttu-id="ee093-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="ee093-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="ee093-204">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-204">✔</span></span> | <span data-ttu-id="ee093-205">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-205">✔</span></span> | <span data-ttu-id="ee093-206">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-206">✔</span></span> | <span data-ttu-id="ee093-207">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-207">✔</span></span>
<span data-ttu-id="ee093-208">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="ee093-208">Geometry.AsText()</span></span> | <span data-ttu-id="ee093-209">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-209">✔</span></span> | <span data-ttu-id="ee093-210">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-210">✔</span></span> | <span data-ttu-id="ee093-211">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-211">✔</span></span> | <span data-ttu-id="ee093-212">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-212">✔</span></span>
<span data-ttu-id="ee093-213">Geometry. Boundary</span><span class="sxs-lookup"><span data-stu-id="ee093-213">Geometry.Boundary</span></span> | <span data-ttu-id="ee093-214">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-214">✔</span></span> | | <span data-ttu-id="ee093-215">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-215">✔</span></span> | <span data-ttu-id="ee093-216">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-216">✔</span></span>
<span data-ttu-id="ee093-217">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="ee093-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="ee093-218">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-218">✔</span></span> | <span data-ttu-id="ee093-219">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-219">✔</span></span> | <span data-ttu-id="ee093-220">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-220">✔</span></span> | <span data-ttu-id="ee093-221">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-221">✔</span></span>
<span data-ttu-id="ee093-222">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="ee093-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="ee093-223">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-223">✔</span></span>
<span data-ttu-id="ee093-224">Geometry. Centroid</span><span class="sxs-lookup"><span data-stu-id="ee093-224">Geometry.Centroid</span></span> | <span data-ttu-id="ee093-225">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-225">✔</span></span> | | <span data-ttu-id="ee093-226">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-226">✔</span></span> | <span data-ttu-id="ee093-227">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-227">✔</span></span>
<span data-ttu-id="ee093-228">Geometry. enthält (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-228">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="ee093-229">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-229">✔</span></span> | <span data-ttu-id="ee093-230">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-230">✔</span></span> | <span data-ttu-id="ee093-231">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-231">✔</span></span> | <span data-ttu-id="ee093-232">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-232">✔</span></span>
<span data-ttu-id="ee093-233">Geometry. config-Hülle ()</span><span class="sxs-lookup"><span data-stu-id="ee093-233">Geometry.ConvexHull()</span></span> | <span data-ttu-id="ee093-234">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-234">✔</span></span> | <span data-ttu-id="ee093-235">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-235">✔</span></span> | <span data-ttu-id="ee093-236">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-236">✔</span></span> | <span data-ttu-id="ee093-237">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-237">✔</span></span>
<span data-ttu-id="ee093-238">Geometry. coveredby (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-238">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="ee093-239">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-239">✔</span></span> | <span data-ttu-id="ee093-240">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-240">✔</span></span>
<span data-ttu-id="ee093-241">Geometry. Cover (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-241">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="ee093-242">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-242">✔</span></span> | <span data-ttu-id="ee093-243">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-243">✔</span></span>
<span data-ttu-id="ee093-244">Geometry. Kreuze (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-244">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="ee093-245">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-245">✔</span></span> | | <span data-ttu-id="ee093-246">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-246">✔</span></span> | <span data-ttu-id="ee093-247">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-247">✔</span></span>
<span data-ttu-id="ee093-248">Geometry. Difference (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-248">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="ee093-249">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-249">✔</span></span> | <span data-ttu-id="ee093-250">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-250">✔</span></span> | <span data-ttu-id="ee093-251">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-251">✔</span></span> | <span data-ttu-id="ee093-252">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-252">✔</span></span>
<span data-ttu-id="ee093-253">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="ee093-253">Geometry.Dimension</span></span> | <span data-ttu-id="ee093-254">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-254">✔</span></span> | <span data-ttu-id="ee093-255">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-255">✔</span></span> | <span data-ttu-id="ee093-256">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-256">✔</span></span> | <span data-ttu-id="ee093-257">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-257">✔</span></span>
<span data-ttu-id="ee093-258">Geometry. disjoint (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-258">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="ee093-259">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-259">✔</span></span> | <span data-ttu-id="ee093-260">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-260">✔</span></span> | <span data-ttu-id="ee093-261">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-261">✔</span></span> | <span data-ttu-id="ee093-262">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-262">✔</span></span>
<span data-ttu-id="ee093-263">Geometry. distance (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-263">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="ee093-264">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-264">✔</span></span> | <span data-ttu-id="ee093-265">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-265">✔</span></span> | <span data-ttu-id="ee093-266">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-266">✔</span></span> | <span data-ttu-id="ee093-267">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-267">✔</span></span>
<span data-ttu-id="ee093-268">Geometry. Umschlag</span><span class="sxs-lookup"><span data-stu-id="ee093-268">Geometry.Envelope</span></span> | <span data-ttu-id="ee093-269">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-269">✔</span></span> | | <span data-ttu-id="ee093-270">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-270">✔</span></span> | <span data-ttu-id="ee093-271">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-271">✔</span></span>
<span data-ttu-id="ee093-272">Geometry. EqualsExact (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-272">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="ee093-273">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-273">✔</span></span>
<span data-ttu-id="ee093-274">Geometry. equalstopologisch (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-274">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="ee093-275">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-275">✔</span></span> | <span data-ttu-id="ee093-276">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-276">✔</span></span> | <span data-ttu-id="ee093-277">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-277">✔</span></span> | <span data-ttu-id="ee093-278">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-278">✔</span></span>
<span data-ttu-id="ee093-279">Geometry. geometrytype</span><span class="sxs-lookup"><span data-stu-id="ee093-279">Geometry.GeometryType</span></span> | <span data-ttu-id="ee093-280">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-280">✔</span></span> | <span data-ttu-id="ee093-281">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-281">✔</span></span> | <span data-ttu-id="ee093-282">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-282">✔</span></span> | <span data-ttu-id="ee093-283">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-283">✔</span></span>
<span data-ttu-id="ee093-284">Geometry. getgeometryn (int)</span><span class="sxs-lookup"><span data-stu-id="ee093-284">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="ee093-285">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-285">✔</span></span> | | <span data-ttu-id="ee093-286">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-286">✔</span></span> | <span data-ttu-id="ee093-287">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-287">✔</span></span>
<span data-ttu-id="ee093-288">Geometry. interiorpoint</span><span class="sxs-lookup"><span data-stu-id="ee093-288">Geometry.InteriorPoint</span></span> | <span data-ttu-id="ee093-289">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-289">✔</span></span> | | <span data-ttu-id="ee093-290">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-290">✔</span></span>
<span data-ttu-id="ee093-291">Geometry. Schnittmenge (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-291">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="ee093-292">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-292">✔</span></span> | <span data-ttu-id="ee093-293">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-293">✔</span></span> | <span data-ttu-id="ee093-294">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-294">✔</span></span> | <span data-ttu-id="ee093-295">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-295">✔</span></span>
<span data-ttu-id="ee093-296">Geometry. intersekten (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-296">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="ee093-297">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-297">✔</span></span> | <span data-ttu-id="ee093-298">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-298">✔</span></span> | <span data-ttu-id="ee093-299">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-299">✔</span></span> | <span data-ttu-id="ee093-300">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-300">✔</span></span>
<span data-ttu-id="ee093-301">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="ee093-301">Geometry.IsEmpty</span></span> | <span data-ttu-id="ee093-302">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-302">✔</span></span> | <span data-ttu-id="ee093-303">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-303">✔</span></span> | <span data-ttu-id="ee093-304">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-304">✔</span></span> | <span data-ttu-id="ee093-305">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-305">✔</span></span>
<span data-ttu-id="ee093-306">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="ee093-306">Geometry.IsSimple</span></span> | <span data-ttu-id="ee093-307">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-307">✔</span></span> | | <span data-ttu-id="ee093-308">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-308">✔</span></span> | <span data-ttu-id="ee093-309">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-309">✔</span></span>
<span data-ttu-id="ee093-310">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="ee093-310">Geometry.IsValid</span></span> | <span data-ttu-id="ee093-311">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-311">✔</span></span> | <span data-ttu-id="ee093-312">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-312">✔</span></span> | <span data-ttu-id="ee093-313">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-313">✔</span></span> | <span data-ttu-id="ee093-314">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-314">✔</span></span>
<span data-ttu-id="ee093-315">Geometry. iswithindistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="ee093-315">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="ee093-316">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-316">✔</span></span> | | <span data-ttu-id="ee093-317">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-317">✔</span></span>
<span data-ttu-id="ee093-318">Geometry. length</span><span class="sxs-lookup"><span data-stu-id="ee093-318">Geometry.Length</span></span> | <span data-ttu-id="ee093-319">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-319">✔</span></span> | <span data-ttu-id="ee093-320">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-320">✔</span></span> | <span data-ttu-id="ee093-321">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-321">✔</span></span> | <span data-ttu-id="ee093-322">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-322">✔</span></span>
<span data-ttu-id="ee093-323">Geometry. numgeometries</span><span class="sxs-lookup"><span data-stu-id="ee093-323">Geometry.NumGeometries</span></span> | <span data-ttu-id="ee093-324">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-324">✔</span></span> | <span data-ttu-id="ee093-325">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-325">✔</span></span> | <span data-ttu-id="ee093-326">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-326">✔</span></span> | <span data-ttu-id="ee093-327">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-327">✔</span></span>
<span data-ttu-id="ee093-328">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="ee093-328">Geometry.NumPoints</span></span> | <span data-ttu-id="ee093-329">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-329">✔</span></span> | <span data-ttu-id="ee093-330">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-330">✔</span></span> | <span data-ttu-id="ee093-331">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-331">✔</span></span> | <span data-ttu-id="ee093-332">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-332">✔</span></span>
<span data-ttu-id="ee093-333">Geometry. ogcgeometrytype</span><span class="sxs-lookup"><span data-stu-id="ee093-333">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="ee093-334">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-334">✔</span></span> | <span data-ttu-id="ee093-335">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-335">✔</span></span> | <span data-ttu-id="ee093-336">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-336">✔</span></span>
<span data-ttu-id="ee093-337">Geometry. Überlappung (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-337">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="ee093-338">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-338">✔</span></span> | <span data-ttu-id="ee093-339">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-339">✔</span></span> | <span data-ttu-id="ee093-340">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-340">✔</span></span> | <span data-ttu-id="ee093-341">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-341">✔</span></span>
<span data-ttu-id="ee093-342">Geometry. pointonsurface</span><span class="sxs-lookup"><span data-stu-id="ee093-342">Geometry.PointOnSurface</span></span> | <span data-ttu-id="ee093-343">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-343">✔</span></span> | | <span data-ttu-id="ee093-344">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-344">✔</span></span> | <span data-ttu-id="ee093-345">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-345">✔</span></span>
<span data-ttu-id="ee093-346">Geometry. in Beziehung (Geometrie, Zeichenfolge)</span><span class="sxs-lookup"><span data-stu-id="ee093-346">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="ee093-347">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-347">✔</span></span> | | <span data-ttu-id="ee093-348">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-348">✔</span></span> | <span data-ttu-id="ee093-349">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-349">✔</span></span>
<span data-ttu-id="ee093-350">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="ee093-350">Geometry.Reverse()</span></span> | | | <span data-ttu-id="ee093-351">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-351">✔</span></span> | <span data-ttu-id="ee093-352">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-352">✔</span></span>
<span data-ttu-id="ee093-353">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="ee093-353">Geometry.SRID</span></span> | <span data-ttu-id="ee093-354">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-354">✔</span></span> | <span data-ttu-id="ee093-355">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-355">✔</span></span> | <span data-ttu-id="ee093-356">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-356">✔</span></span> | <span data-ttu-id="ee093-357">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-357">✔</span></span>
<span data-ttu-id="ee093-358">Geometry. symmetricdifference (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-358">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="ee093-359">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-359">✔</span></span> | <span data-ttu-id="ee093-360">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-360">✔</span></span> | <span data-ttu-id="ee093-361">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-361">✔</span></span> | <span data-ttu-id="ee093-362">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-362">✔</span></span>
<span data-ttu-id="ee093-363">Geometry. $ Binary ()</span><span class="sxs-lookup"><span data-stu-id="ee093-363">Geometry.ToBinary()</span></span> | <span data-ttu-id="ee093-364">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-364">✔</span></span> | <span data-ttu-id="ee093-365">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-365">✔</span></span> | <span data-ttu-id="ee093-366">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-366">✔</span></span> | <span data-ttu-id="ee093-367">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-367">✔</span></span>
<span data-ttu-id="ee093-368">Geometry.-Text ()</span><span class="sxs-lookup"><span data-stu-id="ee093-368">Geometry.ToText()</span></span> | <span data-ttu-id="ee093-369">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-369">✔</span></span> | <span data-ttu-id="ee093-370">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-370">✔</span></span> | <span data-ttu-id="ee093-371">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-371">✔</span></span> | <span data-ttu-id="ee093-372">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-372">✔</span></span>
<span data-ttu-id="ee093-373">Geometry. touches (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-373">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="ee093-374">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-374">✔</span></span> | | <span data-ttu-id="ee093-375">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-375">✔</span></span> | <span data-ttu-id="ee093-376">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-376">✔</span></span>
<span data-ttu-id="ee093-377">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="ee093-377">Geometry.Union()</span></span> | | | <span data-ttu-id="ee093-378">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-378">✔</span></span>
<span data-ttu-id="ee093-379">Geometry. Union (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="ee093-379">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="ee093-380">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-380">✔</span></span> | <span data-ttu-id="ee093-381">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-381">✔</span></span> | <span data-ttu-id="ee093-382">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-382">✔</span></span> | <span data-ttu-id="ee093-383">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-383">✔</span></span>
<span data-ttu-id="ee093-384">Geometry. within (Geometry)</span><span class="sxs-lookup"><span data-stu-id="ee093-384">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="ee093-385">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-385">✔</span></span> | <span data-ttu-id="ee093-386">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-386">✔</span></span> | <span data-ttu-id="ee093-387">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-387">✔</span></span> | <span data-ttu-id="ee093-388">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-388">✔</span></span>
<span data-ttu-id="ee093-389">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="ee093-389">GeometryCollection.Count</span></span> | <span data-ttu-id="ee093-390">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-390">✔</span></span> | <span data-ttu-id="ee093-391">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-391">✔</span></span> | <span data-ttu-id="ee093-392">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-392">✔</span></span> | <span data-ttu-id="ee093-393">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-393">✔</span></span>
<span data-ttu-id="ee093-394">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="ee093-394">GeometryCollection[int]</span></span> | <span data-ttu-id="ee093-395">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-395">✔</span></span> | <span data-ttu-id="ee093-396">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-396">✔</span></span> | <span data-ttu-id="ee093-397">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-397">✔</span></span> | <span data-ttu-id="ee093-398">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-398">✔</span></span>
<span data-ttu-id="ee093-399">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="ee093-399">LineString.Count</span></span> | <span data-ttu-id="ee093-400">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-400">✔</span></span> | <span data-ttu-id="ee093-401">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-401">✔</span></span> | <span data-ttu-id="ee093-402">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-402">✔</span></span> | <span data-ttu-id="ee093-403">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-403">✔</span></span>
<span data-ttu-id="ee093-404">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="ee093-404">LineString.EndPoint</span></span> | <span data-ttu-id="ee093-405">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-405">✔</span></span> | <span data-ttu-id="ee093-406">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-406">✔</span></span> | <span data-ttu-id="ee093-407">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-407">✔</span></span> | <span data-ttu-id="ee093-408">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-408">✔</span></span>
<span data-ttu-id="ee093-409">LineString. getpointn (int)</span><span class="sxs-lookup"><span data-stu-id="ee093-409">LineString.GetPointN(int)</span></span> | <span data-ttu-id="ee093-410">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-410">✔</span></span> | <span data-ttu-id="ee093-411">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-411">✔</span></span> | <span data-ttu-id="ee093-412">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-412">✔</span></span> | <span data-ttu-id="ee093-413">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-413">✔</span></span>
<span data-ttu-id="ee093-414">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="ee093-414">LineString.IsClosed</span></span> | <span data-ttu-id="ee093-415">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-415">✔</span></span> | <span data-ttu-id="ee093-416">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-416">✔</span></span> | <span data-ttu-id="ee093-417">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-417">✔</span></span> | <span data-ttu-id="ee093-418">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-418">✔</span></span>
<span data-ttu-id="ee093-419">LineString. isring</span><span class="sxs-lookup"><span data-stu-id="ee093-419">LineString.IsRing</span></span> | <span data-ttu-id="ee093-420">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-420">✔</span></span> | | <span data-ttu-id="ee093-421">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-421">✔</span></span> | <span data-ttu-id="ee093-422">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-422">✔</span></span>
<span data-ttu-id="ee093-423">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="ee093-423">LineString.StartPoint</span></span> | <span data-ttu-id="ee093-424">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-424">✔</span></span> | <span data-ttu-id="ee093-425">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-425">✔</span></span> | <span data-ttu-id="ee093-426">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-426">✔</span></span> | <span data-ttu-id="ee093-427">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-427">✔</span></span>
<span data-ttu-id="ee093-428">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="ee093-428">MultiLineString.IsClosed</span></span> | <span data-ttu-id="ee093-429">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-429">✔</span></span> | <span data-ttu-id="ee093-430">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-430">✔</span></span> | <span data-ttu-id="ee093-431">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-431">✔</span></span> | <span data-ttu-id="ee093-432">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-432">✔</span></span>
<span data-ttu-id="ee093-433">Punkt. M</span><span class="sxs-lookup"><span data-stu-id="ee093-433">Point.M</span></span> | <span data-ttu-id="ee093-434">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-434">✔</span></span> | <span data-ttu-id="ee093-435">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-435">✔</span></span> | <span data-ttu-id="ee093-436">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-436">✔</span></span> | <span data-ttu-id="ee093-437">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-437">✔</span></span>
<span data-ttu-id="ee093-438">Point. X</span><span class="sxs-lookup"><span data-stu-id="ee093-438">Point.X</span></span> | <span data-ttu-id="ee093-439">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-439">✔</span></span> | <span data-ttu-id="ee093-440">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-440">✔</span></span> | <span data-ttu-id="ee093-441">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-441">✔</span></span> | <span data-ttu-id="ee093-442">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-442">✔</span></span>
<span data-ttu-id="ee093-443">Punkt. Y</span><span class="sxs-lookup"><span data-stu-id="ee093-443">Point.Y</span></span> | <span data-ttu-id="ee093-444">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-444">✔</span></span> | <span data-ttu-id="ee093-445">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-445">✔</span></span> | <span data-ttu-id="ee093-446">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-446">✔</span></span> | <span data-ttu-id="ee093-447">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-447">✔</span></span>
<span data-ttu-id="ee093-448">Punkt. Z</span><span class="sxs-lookup"><span data-stu-id="ee093-448">Point.Z</span></span> | <span data-ttu-id="ee093-449">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-449">✔</span></span> | <span data-ttu-id="ee093-450">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-450">✔</span></span> | <span data-ttu-id="ee093-451">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-451">✔</span></span> | <span data-ttu-id="ee093-452">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-452">✔</span></span>
<span data-ttu-id="ee093-453">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="ee093-453">Polygon.ExteriorRing</span></span> | <span data-ttu-id="ee093-454">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-454">✔</span></span> | <span data-ttu-id="ee093-455">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-455">✔</span></span> | <span data-ttu-id="ee093-456">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-456">✔</span></span> | <span data-ttu-id="ee093-457">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-457">✔</span></span>
<span data-ttu-id="ee093-458">Polygon. getinteriorringn (int)</span><span class="sxs-lookup"><span data-stu-id="ee093-458">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="ee093-459">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-459">✔</span></span> | <span data-ttu-id="ee093-460">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-460">✔</span></span> | <span data-ttu-id="ee093-461">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-461">✔</span></span> | <span data-ttu-id="ee093-462">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-462">✔</span></span>
<span data-ttu-id="ee093-463">Polygon. numinteriorrings</span><span class="sxs-lookup"><span data-stu-id="ee093-463">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="ee093-464">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-464">✔</span></span> | <span data-ttu-id="ee093-465">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-465">✔</span></span> | <span data-ttu-id="ee093-466">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-466">✔</span></span> | <span data-ttu-id="ee093-467">✔</span><span class="sxs-lookup"><span data-stu-id="ee093-467">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ee093-468">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ee093-468">Additional resources</span></span>

* [<span data-ttu-id="ee093-469">Räumliche Daten in SQL Server</span><span class="sxs-lookup"><span data-stu-id="ee093-469">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="ee093-470">Spatialite-Homepage</span><span class="sxs-lookup"><span data-stu-id="ee093-470">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="ee093-471">Räumliche Dokumentation zu npgsql</span><span class="sxs-lookup"><span data-stu-id="ee093-471">Npgsql Spatial Documentation</span></span>](http://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="ee093-472">PostGIS-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="ee093-472">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
