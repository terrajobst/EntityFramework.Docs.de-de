---
title: Räumliche Daten-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cced53edadb890e4e86753ec2628218ffc4d1d5b
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181391"
---
# <a name="spatial-data"></a><span data-ttu-id="388b3-102">Räumliche Daten</span><span class="sxs-lookup"><span data-stu-id="388b3-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="388b3-103">Diese Funktion wurde in EF Core 2,2 hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="388b3-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="388b3-104">Räumliche Daten repräsentieren den physischen Speicherort und die Form von Objekten.</span><span class="sxs-lookup"><span data-stu-id="388b3-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="388b3-105">Viele Datenbanken unterstützen diese Art von Daten, damit Sie zusammen mit anderen Daten indiziert und abgefragt werden können.</span><span class="sxs-lookup"><span data-stu-id="388b3-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="388b3-106">Häufige Szenarien umfassen das Abfragen von Objekten in einer bestimmten Entfernung von einem Speicherort oder das Auswählen des Objekts, dessen Rahmen eine bestimmte Position enthält.</span><span class="sxs-lookup"><span data-stu-id="388b3-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="388b3-107">EF Core unterstützt die Zuordnung räumlicher Datentypen mithilfe der räumlichen Bibliothek [nettopologysuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="388b3-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="388b3-108">Installation</span><span class="sxs-lookup"><span data-stu-id="388b3-108">Installing</span></span>

<span data-ttu-id="388b3-109">Um räumliche Daten mit EF Core verwenden zu können, müssen Sie das entsprechende unterstützende nuget-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="388b3-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="388b3-110">Welches Paket installiert werden muss, hängt vom verwendeten Anbieter ab.</span><span class="sxs-lookup"><span data-stu-id="388b3-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="388b3-111">EF Core Anbieter</span><span class="sxs-lookup"><span data-stu-id="388b3-111">EF Core Provider</span></span>                        | <span data-ttu-id="388b3-112">Räumliches nuget-Paket</span><span class="sxs-lookup"><span data-stu-id="388b3-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="388b3-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="388b3-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="388b3-114">Microsoft. entityframeworkcore. SqlServer. nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="388b3-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="388b3-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="388b3-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="388b3-116">Microsoft. entityframeworkcore. sqlite. nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="388b3-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="388b3-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="388b3-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="388b3-118">Nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="388b3-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="388b3-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="388b3-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="388b3-120">Npgsql. entityframeworkcore. PostgreSQL. nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="388b3-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="388b3-121">Reverse Engineering</span><span class="sxs-lookup"><span data-stu-id="388b3-121">Reverse engineering</span></span>

<span data-ttu-id="388b3-122">Die räumlichen nuget-Pakete ermöglichen auch das [Reverse Engineering](../managing-schemas/scaffolding.md) von Modellen mit räumlichen Eigenschaften. Sie müssen jedoch das Paket installieren, ***bevor*** Sie `Scaffold-DbContext` oder `dotnet ef dbcontext scaffold` ausführen.</span><span class="sxs-lookup"><span data-stu-id="388b3-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="388b3-123">Wenn Sie dies nicht tun, erhalten Sie Warnungen, wenn Sie keine Typzuordnungen für die Spalten finden und die Spalten übersprungen werden.</span><span class="sxs-lookup"><span data-stu-id="388b3-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="388b3-124">Nettopologysuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="388b3-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="388b3-125">Nettopologysuite ist eine räumliche Bibliothek für .net.</span><span class="sxs-lookup"><span data-stu-id="388b3-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="388b3-126">EF Core ermöglicht die Zuordnung von räumlichen Datentypen in der Datenbank mithilfe von NTS-Typen in Ihrem Modell.</span><span class="sxs-lookup"><span data-stu-id="388b3-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="388b3-127">Um die Zuordnung räumlicher Typen über NTS zu ermöglichen, müssen Sie die usenettopologysuite-Methode für den dbcontext Options-Generator des Anbieters abrufen.</span><span class="sxs-lookup"><span data-stu-id="388b3-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="388b3-128">Beispielsweise können Sie mit SQL Server wie folgt bezeichnen.</span><span class="sxs-lookup"><span data-stu-id="388b3-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="388b3-129">Es gibt mehrere räumliche Datentypen.</span><span class="sxs-lookup"><span data-stu-id="388b3-129">There are several spatial data types.</span></span> <span data-ttu-id="388b3-130">Welcher Typ Sie verwenden, hängt von den Formen der Formen ab, die Sie zulassen möchten.</span><span class="sxs-lookup"><span data-stu-id="388b3-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="388b3-131">Hier ist die Hierarchie der NTS-Typen, die Sie für Eigenschaften im Modell verwenden können.</span><span class="sxs-lookup"><span data-stu-id="388b3-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="388b3-132">Sie befinden sich im `NetTopologySuite.Geometries`-Namespace.</span><span class="sxs-lookup"><span data-stu-id="388b3-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="388b3-133">Et</span><span class="sxs-lookup"><span data-stu-id="388b3-133">Geometry</span></span>
  * <span data-ttu-id="388b3-134">Punkt</span><span class="sxs-lookup"><span data-stu-id="388b3-134">Point</span></span>
  * <span data-ttu-id="388b3-135">LineString</span><span class="sxs-lookup"><span data-stu-id="388b3-135">LineString</span></span>
  * <span data-ttu-id="388b3-136">Polygon</span><span class="sxs-lookup"><span data-stu-id="388b3-136">Polygon</span></span>
  * <span data-ttu-id="388b3-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="388b3-137">GeometryCollection</span></span>
    * <span data-ttu-id="388b3-138">Multipoint</span><span class="sxs-lookup"><span data-stu-id="388b3-138">MultiPoint</span></span>
    * <span data-ttu-id="388b3-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="388b3-139">MultiLineString</span></span>
    * <span data-ttu-id="388b3-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="388b3-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="388b3-141">Circularstring, compoundcurve und curepolygon werden von NTS nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="388b3-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="388b3-142">Mit dem basisgeometry-Typ kann jeder Typ von Form von der-Eigenschaft angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="388b3-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="388b3-143">Die folgenden Entitäts Klassen können verwendet werden, um Tabellen in der [Beispieldatenbank Wide World Importern](https://go.microsoft.com/fwlink/?LinkID=800630)zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="388b3-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="388b3-144">Erstellen von Werten</span><span class="sxs-lookup"><span data-stu-id="388b3-144">Creating values</span></span>

<span data-ttu-id="388b3-145">Sie können Konstruktoren zum Erstellen von Geometry-Objekten verwenden. Allerdings empfiehlt es sich, stattdessen eine Geometry-Factory zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="388b3-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="388b3-146">Auf diese Weise können Sie ein Standardmäßiges SRID angeben (das von den Koordinaten verwendete räumliche Verweis System) und Ihnen die Kontrolle über erweiterte Dinge wie das Genauigkeits Modell (verwendet während der Berechnungen) und die Koordinaten Sequenz (bestimmt, welche ordinates--Dimensionen und Measures--sind verfügbar).</span><span class="sxs-lookup"><span data-stu-id="388b3-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="388b3-147">4326 bezieht sich auf WGS 84, einen in GPS und anderen geografischen Systemen verwendeten Standard.</span><span class="sxs-lookup"><span data-stu-id="388b3-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="388b3-148">Längen-und Breitengrad</span><span class="sxs-lookup"><span data-stu-id="388b3-148">Longitude and Latitude</span></span>

<span data-ttu-id="388b3-149">Die Koordinaten in den NTS sind X-und Y-Werte.</span><span class="sxs-lookup"><span data-stu-id="388b3-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="388b3-150">Um Längen-und Breitengrad darzustellen, verwenden Sie X für Längengrad und Y für Breitengrad.</span><span class="sxs-lookup"><span data-stu-id="388b3-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="388b3-151">Beachten Sie, dass **sich diese** Werte im `latitude, longitude`-Format befinden, in dem Sie normalerweise diese Werte sehen.</span><span class="sxs-lookup"><span data-stu-id="388b3-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="388b3-152">SRID wird bei Client Vorgängen ignoriert.</span><span class="sxs-lookup"><span data-stu-id="388b3-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="388b3-153">NTS ignoriert SRID-Werte während des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="388b3-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="388b3-154">Es wird von einem planaren Koordinatensystem ausgegangen.</span><span class="sxs-lookup"><span data-stu-id="388b3-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="388b3-155">Dies bedeutet Folgendes: Wenn Sie Koordinaten in Bezug auf Längengrad und Breitengrad angeben, werden einige vom Client ausgewertete Werte wie "Distance", "length" und "Area" in Grad und nicht in "Meter" angegeben.</span><span class="sxs-lookup"><span data-stu-id="388b3-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="388b3-156">Wenn Sie aussagekräftigere Werte benötigen, müssen Sie die Koordinaten zunächst mithilfe einer Bibliothek wie [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) auf ein anderes Koordinatensystem projizieren, bevor Sie diese Werte berechnen.</span><span class="sxs-lookup"><span data-stu-id="388b3-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="388b3-157">Wenn ein Vorgang vom Server durch EF Core über SQL ausgewertet wird, wird die Einheit des Ergebnisses von der Datenbank bestimmt.</span><span class="sxs-lookup"><span data-stu-id="388b3-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="388b3-158">Im folgenden finden Sie ein Beispiel für die Verwendung von ProjNet4GeoAPI, um den Abstand zwischen zwei Städten zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="388b3-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="388b3-159">Abfrage von Daten</span><span class="sxs-lookup"><span data-stu-id="388b3-159">Querying Data</span></span>

<span data-ttu-id="388b3-160">In LINQ werden die NTS-Methoden und-Eigenschaften, die als Datenbankfunktionen verfügbar sind, in SQL übersetzt.</span><span class="sxs-lookup"><span data-stu-id="388b3-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="388b3-161">Beispielsweise werden die Distance-Methode und die-Methode in den folgenden Abfragen übersetzt.</span><span class="sxs-lookup"><span data-stu-id="388b3-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="388b3-162">Die Tabelle am Ende dieses Artikels zeigt, welche Member von verschiedenen EF Core Anbietern unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="388b3-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="388b3-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="388b3-163">SQL Server</span></span>

<span data-ttu-id="388b3-164">Wenn Sie SQL Server verwenden, müssen Sie einige zusätzliche Punkte beachten.</span><span class="sxs-lookup"><span data-stu-id="388b3-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="388b3-165">Geography oder Geometry</span><span class="sxs-lookup"><span data-stu-id="388b3-165">Geography or geometry</span></span>

<span data-ttu-id="388b3-166">Standardmäßig werden räumliche Eigenschaften `geography`-Spalten in SQL Server zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="388b3-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="388b3-167">Um `geometry` zu verwenden, [Konfigurieren Sie den Spaltentyp](xref:core/modeling/relational/data-types) in Ihrem Modell.</span><span class="sxs-lookup"><span data-stu-id="388b3-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="388b3-168">Polygon-Polygon Ringe</span><span class="sxs-lookup"><span data-stu-id="388b3-168">Geography polygon rings</span></span>

<span data-ttu-id="388b3-169">Wenn Sie den Spaltentyp `geography` verwenden, setzt SQL Server zusätzliche Anforderungen für den äußeren Ring (oder die Shell) und die inneren Ringe (oder Löcher).</span><span class="sxs-lookup"><span data-stu-id="388b3-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="388b3-170">Der äußere Ring muss gegen den Uhrzeigersinn und die inneren Ringe im Uhrzeigersinn ausgerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="388b3-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="388b3-171">NTS überprüft dies, bevor Werte an die Datenbank gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="388b3-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="388b3-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="388b3-172">FullGlobe</span></span>

<span data-ttu-id="388b3-173">SQL Server weist einen nicht standardmäßigen Geometry-Typ auf, der den vollständigen Globus darstellt, wenn der `geography`-Spaltentyp verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="388b3-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="388b3-174">Außerdem bietet es eine Möglichkeit, Polygone auf der ganzen Welt (ohne äußeren Ring) darzustellen.</span><span class="sxs-lookup"><span data-stu-id="388b3-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="388b3-175">Keines dieser beiden wird von NTS unterstützt.</span><span class="sxs-lookup"><span data-stu-id="388b3-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="388b3-176">Fullglobe und Polygone, die darauf basieren, werden von NTS nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="388b3-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="388b3-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="388b3-177">SQLite</span></span>

<span data-ttu-id="388b3-178">Hier finden Sie einige zusätzliche Informationen für diejenigen, die SQLite verwenden.</span><span class="sxs-lookup"><span data-stu-id="388b3-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="388b3-179">Installieren von spatialite</span><span class="sxs-lookup"><span data-stu-id="388b3-179">Installing SpatiaLite</span></span>

<span data-ttu-id="388b3-180">Unter Windows wird die native mod_spatialite-Bibliothek als eine nuget-Paketabhängigkeit verteilt.</span><span class="sxs-lookup"><span data-stu-id="388b3-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="388b3-181">Andere Plattformen müssen Sie separat installieren.</span><span class="sxs-lookup"><span data-stu-id="388b3-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="388b3-182">Dies erfolgt in der Regel mithilfe eines Softwarepaket-Managers.</span><span class="sxs-lookup"><span data-stu-id="388b3-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="388b3-183">Beispielsweise können Sie apt unter Ubuntu und Homebrew unter MacOS verwenden.</span><span class="sxs-lookup"><span data-stu-id="388b3-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="388b3-184">Konfigurieren von SRID</span><span class="sxs-lookup"><span data-stu-id="388b3-184">Configuring SRID</span></span>

<span data-ttu-id="388b3-185">In spatialite müssen Spalten eine SRID pro Spalte angeben.</span><span class="sxs-lookup"><span data-stu-id="388b3-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="388b3-186">Der Standard-SRID ist `0`.</span><span class="sxs-lookup"><span data-stu-id="388b3-186">The default SRID is `0`.</span></span> <span data-ttu-id="388b3-187">Geben Sie einen anderen SRID mithilfe der forsqlitehassrid-Methode an.</span><span class="sxs-lookup"><span data-stu-id="388b3-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="388b3-188">Dimension</span><span class="sxs-lookup"><span data-stu-id="388b3-188">Dimension</span></span>

<span data-ttu-id="388b3-189">Ähnlich wie SRID wird auch die Dimension (oder ordinates) einer Spalte als Teil der Spalte angegeben.</span><span class="sxs-lookup"><span data-stu-id="388b3-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="388b3-190">Die Standard Ordinate sind X und Y. Aktivieren Sie zusätzliche Ordinate (Z und M) mithilfe der forsqlitehasdimension-Methode.</span><span class="sxs-lookup"><span data-stu-id="388b3-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="388b3-191">Übersetzte Vorgänge</span><span class="sxs-lookup"><span data-stu-id="388b3-191">Translated Operations</span></span>

<span data-ttu-id="388b3-192">Diese Tabelle zeigt, welche NTS-Elemente von den einzelnen EF Core Anbietern in SQL übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="388b3-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="388b3-193">Nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="388b3-193">NetTopologySuite</span></span> | <span data-ttu-id="388b3-194">SQL Server (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-194">SQL Server (geometry)</span></span> | <span data-ttu-id="388b3-195">SQL Server (Geografie)</span><span class="sxs-lookup"><span data-stu-id="388b3-195">SQL Server (geography)</span></span> | <span data-ttu-id="388b3-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="388b3-196">SQLite</span></span> | <span data-ttu-id="388b3-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="388b3-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="388b3-198">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="388b3-198">Geometry.Area</span></span> | <span data-ttu-id="388b3-199">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-199">✔</span></span> | <span data-ttu-id="388b3-200">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-200">✔</span></span> | <span data-ttu-id="388b3-201">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-201">✔</span></span> | <span data-ttu-id="388b3-202">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-202">✔</span></span>
<span data-ttu-id="388b3-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="388b3-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="388b3-204">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-204">✔</span></span> | <span data-ttu-id="388b3-205">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-205">✔</span></span> | <span data-ttu-id="388b3-206">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-206">✔</span></span> | <span data-ttu-id="388b3-207">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-207">✔</span></span>
<span data-ttu-id="388b3-208">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="388b3-208">Geometry.AsText()</span></span> | <span data-ttu-id="388b3-209">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-209">✔</span></span> | <span data-ttu-id="388b3-210">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-210">✔</span></span> | <span data-ttu-id="388b3-211">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-211">✔</span></span> | <span data-ttu-id="388b3-212">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-212">✔</span></span>
<span data-ttu-id="388b3-213">Geometry. Boundary</span><span class="sxs-lookup"><span data-stu-id="388b3-213">Geometry.Boundary</span></span> | <span data-ttu-id="388b3-214">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-214">✔</span></span> | | <span data-ttu-id="388b3-215">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-215">✔</span></span> | <span data-ttu-id="388b3-216">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-216">✔</span></span>
<span data-ttu-id="388b3-217">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="388b3-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="388b3-218">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-218">✔</span></span> | <span data-ttu-id="388b3-219">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-219">✔</span></span> | <span data-ttu-id="388b3-220">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-220">✔</span></span> | <span data-ttu-id="388b3-221">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-221">✔</span></span>
<span data-ttu-id="388b3-222">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="388b3-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="388b3-223">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-223">✔</span></span>
<span data-ttu-id="388b3-224">Geometry. Centroid</span><span class="sxs-lookup"><span data-stu-id="388b3-224">Geometry.Centroid</span></span> | <span data-ttu-id="388b3-225">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-225">✔</span></span> | | <span data-ttu-id="388b3-226">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-226">✔</span></span> | <span data-ttu-id="388b3-227">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-227">✔</span></span>
<span data-ttu-id="388b3-228">Geometry. enthält (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-228">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="388b3-229">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-229">✔</span></span> | <span data-ttu-id="388b3-230">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-230">✔</span></span> | <span data-ttu-id="388b3-231">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-231">✔</span></span> | <span data-ttu-id="388b3-232">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-232">✔</span></span>
<span data-ttu-id="388b3-233">Geometry. config-Hülle ()</span><span class="sxs-lookup"><span data-stu-id="388b3-233">Geometry.ConvexHull()</span></span> | <span data-ttu-id="388b3-234">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-234">✔</span></span> | <span data-ttu-id="388b3-235">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-235">✔</span></span> | <span data-ttu-id="388b3-236">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-236">✔</span></span> | <span data-ttu-id="388b3-237">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-237">✔</span></span>
<span data-ttu-id="388b3-238">Geometry. coveredby (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-238">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="388b3-239">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-239">✔</span></span> | <span data-ttu-id="388b3-240">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-240">✔</span></span>
<span data-ttu-id="388b3-241">Geometry. Cover (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-241">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="388b3-242">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-242">✔</span></span> | <span data-ttu-id="388b3-243">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-243">✔</span></span>
<span data-ttu-id="388b3-244">Geometry. Kreuze (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-244">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="388b3-245">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-245">✔</span></span> | | <span data-ttu-id="388b3-246">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-246">✔</span></span> | <span data-ttu-id="388b3-247">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-247">✔</span></span>
<span data-ttu-id="388b3-248">Geometry. Difference (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-248">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="388b3-249">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-249">✔</span></span> | <span data-ttu-id="388b3-250">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-250">✔</span></span> | <span data-ttu-id="388b3-251">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-251">✔</span></span> | <span data-ttu-id="388b3-252">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-252">✔</span></span>
<span data-ttu-id="388b3-253">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="388b3-253">Geometry.Dimension</span></span> | <span data-ttu-id="388b3-254">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-254">✔</span></span> | <span data-ttu-id="388b3-255">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-255">✔</span></span> | <span data-ttu-id="388b3-256">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-256">✔</span></span> | <span data-ttu-id="388b3-257">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-257">✔</span></span>
<span data-ttu-id="388b3-258">Geometry. disjoint (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-258">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="388b3-259">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-259">✔</span></span> | <span data-ttu-id="388b3-260">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-260">✔</span></span> | <span data-ttu-id="388b3-261">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-261">✔</span></span> | <span data-ttu-id="388b3-262">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-262">✔</span></span>
<span data-ttu-id="388b3-263">Geometry. distance (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-263">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="388b3-264">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-264">✔</span></span> | <span data-ttu-id="388b3-265">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-265">✔</span></span> | <span data-ttu-id="388b3-266">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-266">✔</span></span> | <span data-ttu-id="388b3-267">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-267">✔</span></span>
<span data-ttu-id="388b3-268">Geometry. Umschlag</span><span class="sxs-lookup"><span data-stu-id="388b3-268">Geometry.Envelope</span></span> | <span data-ttu-id="388b3-269">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-269">✔</span></span> | | <span data-ttu-id="388b3-270">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-270">✔</span></span> | <span data-ttu-id="388b3-271">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-271">✔</span></span>
<span data-ttu-id="388b3-272">Geometry. EqualsExact (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-272">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="388b3-273">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-273">✔</span></span>
<span data-ttu-id="388b3-274">Geometry. equalstopologisch (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-274">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="388b3-275">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-275">✔</span></span> | <span data-ttu-id="388b3-276">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-276">✔</span></span> | <span data-ttu-id="388b3-277">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-277">✔</span></span> | <span data-ttu-id="388b3-278">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-278">✔</span></span>
<span data-ttu-id="388b3-279">Geometry. geometrytype</span><span class="sxs-lookup"><span data-stu-id="388b3-279">Geometry.GeometryType</span></span> | <span data-ttu-id="388b3-280">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-280">✔</span></span> | <span data-ttu-id="388b3-281">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-281">✔</span></span> | <span data-ttu-id="388b3-282">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-282">✔</span></span> | <span data-ttu-id="388b3-283">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-283">✔</span></span>
<span data-ttu-id="388b3-284">Geometry. getgeometryn (int)</span><span class="sxs-lookup"><span data-stu-id="388b3-284">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="388b3-285">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-285">✔</span></span> | | <span data-ttu-id="388b3-286">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-286">✔</span></span> | <span data-ttu-id="388b3-287">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-287">✔</span></span>
<span data-ttu-id="388b3-288">Geometry. interiorpoint</span><span class="sxs-lookup"><span data-stu-id="388b3-288">Geometry.InteriorPoint</span></span> | <span data-ttu-id="388b3-289">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-289">✔</span></span> | | <span data-ttu-id="388b3-290">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-290">✔</span></span>
<span data-ttu-id="388b3-291">Geometry. Schnittmenge (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-291">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="388b3-292">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-292">✔</span></span> | <span data-ttu-id="388b3-293">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-293">✔</span></span> | <span data-ttu-id="388b3-294">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-294">✔</span></span> | <span data-ttu-id="388b3-295">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-295">✔</span></span>
<span data-ttu-id="388b3-296">Geometry. intersekten (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-296">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="388b3-297">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-297">✔</span></span> | <span data-ttu-id="388b3-298">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-298">✔</span></span> | <span data-ttu-id="388b3-299">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-299">✔</span></span> | <span data-ttu-id="388b3-300">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-300">✔</span></span>
<span data-ttu-id="388b3-301">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="388b3-301">Geometry.IsEmpty</span></span> | <span data-ttu-id="388b3-302">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-302">✔</span></span> | <span data-ttu-id="388b3-303">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-303">✔</span></span> | <span data-ttu-id="388b3-304">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-304">✔</span></span> | <span data-ttu-id="388b3-305">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-305">✔</span></span>
<span data-ttu-id="388b3-306">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="388b3-306">Geometry.IsSimple</span></span> | <span data-ttu-id="388b3-307">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-307">✔</span></span> | | <span data-ttu-id="388b3-308">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-308">✔</span></span> | <span data-ttu-id="388b3-309">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-309">✔</span></span>
<span data-ttu-id="388b3-310">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="388b3-310">Geometry.IsValid</span></span> | <span data-ttu-id="388b3-311">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-311">✔</span></span> | <span data-ttu-id="388b3-312">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-312">✔</span></span> | <span data-ttu-id="388b3-313">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-313">✔</span></span> | <span data-ttu-id="388b3-314">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-314">✔</span></span>
<span data-ttu-id="388b3-315">Geometry. iswithindistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="388b3-315">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="388b3-316">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-316">✔</span></span> | | <span data-ttu-id="388b3-317">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-317">✔</span></span>
<span data-ttu-id="388b3-318">Geometry. length</span><span class="sxs-lookup"><span data-stu-id="388b3-318">Geometry.Length</span></span> | <span data-ttu-id="388b3-319">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-319">✔</span></span> | <span data-ttu-id="388b3-320">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-320">✔</span></span> | <span data-ttu-id="388b3-321">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-321">✔</span></span> | <span data-ttu-id="388b3-322">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-322">✔</span></span>
<span data-ttu-id="388b3-323">Geometry. numgeometries</span><span class="sxs-lookup"><span data-stu-id="388b3-323">Geometry.NumGeometries</span></span> | <span data-ttu-id="388b3-324">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-324">✔</span></span> | <span data-ttu-id="388b3-325">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-325">✔</span></span> | <span data-ttu-id="388b3-326">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-326">✔</span></span> | <span data-ttu-id="388b3-327">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-327">✔</span></span>
<span data-ttu-id="388b3-328">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="388b3-328">Geometry.NumPoints</span></span> | <span data-ttu-id="388b3-329">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-329">✔</span></span> | <span data-ttu-id="388b3-330">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-330">✔</span></span> | <span data-ttu-id="388b3-331">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-331">✔</span></span> | <span data-ttu-id="388b3-332">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-332">✔</span></span>
<span data-ttu-id="388b3-333">Geometry. ogcgeometrytype</span><span class="sxs-lookup"><span data-stu-id="388b3-333">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="388b3-334">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-334">✔</span></span> | <span data-ttu-id="388b3-335">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-335">✔</span></span> | <span data-ttu-id="388b3-336">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-336">✔</span></span>
<span data-ttu-id="388b3-337">Geometry. Überlappung (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-337">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="388b3-338">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-338">✔</span></span> | <span data-ttu-id="388b3-339">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-339">✔</span></span> | <span data-ttu-id="388b3-340">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-340">✔</span></span> | <span data-ttu-id="388b3-341">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-341">✔</span></span>
<span data-ttu-id="388b3-342">Geometry. pointonsurface</span><span class="sxs-lookup"><span data-stu-id="388b3-342">Geometry.PointOnSurface</span></span> | <span data-ttu-id="388b3-343">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-343">✔</span></span> | | <span data-ttu-id="388b3-344">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-344">✔</span></span> | <span data-ttu-id="388b3-345">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-345">✔</span></span>
<span data-ttu-id="388b3-346">Geometry. in Beziehung (Geometrie, Zeichenfolge)</span><span class="sxs-lookup"><span data-stu-id="388b3-346">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="388b3-347">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-347">✔</span></span> | | <span data-ttu-id="388b3-348">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-348">✔</span></span> | <span data-ttu-id="388b3-349">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-349">✔</span></span>
<span data-ttu-id="388b3-350">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="388b3-350">Geometry.Reverse()</span></span> | | | <span data-ttu-id="388b3-351">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-351">✔</span></span> | <span data-ttu-id="388b3-352">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-352">✔</span></span>
<span data-ttu-id="388b3-353">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="388b3-353">Geometry.SRID</span></span> | <span data-ttu-id="388b3-354">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-354">✔</span></span> | <span data-ttu-id="388b3-355">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-355">✔</span></span> | <span data-ttu-id="388b3-356">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-356">✔</span></span> | <span data-ttu-id="388b3-357">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-357">✔</span></span>
<span data-ttu-id="388b3-358">Geometry. symmetricdifference (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-358">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="388b3-359">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-359">✔</span></span> | <span data-ttu-id="388b3-360">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-360">✔</span></span> | <span data-ttu-id="388b3-361">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-361">✔</span></span> | <span data-ttu-id="388b3-362">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-362">✔</span></span>
<span data-ttu-id="388b3-363">Geometry. $ Binary ()</span><span class="sxs-lookup"><span data-stu-id="388b3-363">Geometry.ToBinary()</span></span> | <span data-ttu-id="388b3-364">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-364">✔</span></span> | <span data-ttu-id="388b3-365">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-365">✔</span></span> | <span data-ttu-id="388b3-366">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-366">✔</span></span> | <span data-ttu-id="388b3-367">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-367">✔</span></span>
<span data-ttu-id="388b3-368">Geometry.-Text ()</span><span class="sxs-lookup"><span data-stu-id="388b3-368">Geometry.ToText()</span></span> | <span data-ttu-id="388b3-369">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-369">✔</span></span> | <span data-ttu-id="388b3-370">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-370">✔</span></span> | <span data-ttu-id="388b3-371">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-371">✔</span></span> | <span data-ttu-id="388b3-372">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-372">✔</span></span>
<span data-ttu-id="388b3-373">Geometry. touches (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-373">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="388b3-374">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-374">✔</span></span> | | <span data-ttu-id="388b3-375">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-375">✔</span></span> | <span data-ttu-id="388b3-376">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-376">✔</span></span>
<span data-ttu-id="388b3-377">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="388b3-377">Geometry.Union()</span></span> | | | <span data-ttu-id="388b3-378">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-378">✔</span></span>
<span data-ttu-id="388b3-379">Geometry. Union (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="388b3-379">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="388b3-380">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-380">✔</span></span> | <span data-ttu-id="388b3-381">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-381">✔</span></span> | <span data-ttu-id="388b3-382">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-382">✔</span></span> | <span data-ttu-id="388b3-383">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-383">✔</span></span>
<span data-ttu-id="388b3-384">Geometry. within (Geometry)</span><span class="sxs-lookup"><span data-stu-id="388b3-384">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="388b3-385">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-385">✔</span></span> | <span data-ttu-id="388b3-386">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-386">✔</span></span> | <span data-ttu-id="388b3-387">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-387">✔</span></span> | <span data-ttu-id="388b3-388">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-388">✔</span></span>
<span data-ttu-id="388b3-389">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="388b3-389">GeometryCollection.Count</span></span> | <span data-ttu-id="388b3-390">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-390">✔</span></span> | <span data-ttu-id="388b3-391">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-391">✔</span></span> | <span data-ttu-id="388b3-392">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-392">✔</span></span> | <span data-ttu-id="388b3-393">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-393">✔</span></span>
<span data-ttu-id="388b3-394">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="388b3-394">GeometryCollection[int]</span></span> | <span data-ttu-id="388b3-395">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-395">✔</span></span> | <span data-ttu-id="388b3-396">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-396">✔</span></span> | <span data-ttu-id="388b3-397">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-397">✔</span></span> | <span data-ttu-id="388b3-398">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-398">✔</span></span>
<span data-ttu-id="388b3-399">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="388b3-399">LineString.Count</span></span> | <span data-ttu-id="388b3-400">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-400">✔</span></span> | <span data-ttu-id="388b3-401">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-401">✔</span></span> | <span data-ttu-id="388b3-402">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-402">✔</span></span> | <span data-ttu-id="388b3-403">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-403">✔</span></span>
<span data-ttu-id="388b3-404">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="388b3-404">LineString.EndPoint</span></span> | <span data-ttu-id="388b3-405">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-405">✔</span></span> | <span data-ttu-id="388b3-406">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-406">✔</span></span> | <span data-ttu-id="388b3-407">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-407">✔</span></span> | <span data-ttu-id="388b3-408">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-408">✔</span></span>
<span data-ttu-id="388b3-409">LineString. getpointn (int)</span><span class="sxs-lookup"><span data-stu-id="388b3-409">LineString.GetPointN(int)</span></span> | <span data-ttu-id="388b3-410">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-410">✔</span></span> | <span data-ttu-id="388b3-411">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-411">✔</span></span> | <span data-ttu-id="388b3-412">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-412">✔</span></span> | <span data-ttu-id="388b3-413">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-413">✔</span></span>
<span data-ttu-id="388b3-414">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="388b3-414">LineString.IsClosed</span></span> | <span data-ttu-id="388b3-415">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-415">✔</span></span> | <span data-ttu-id="388b3-416">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-416">✔</span></span> | <span data-ttu-id="388b3-417">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-417">✔</span></span> | <span data-ttu-id="388b3-418">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-418">✔</span></span>
<span data-ttu-id="388b3-419">LineString. isring</span><span class="sxs-lookup"><span data-stu-id="388b3-419">LineString.IsRing</span></span> | <span data-ttu-id="388b3-420">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-420">✔</span></span> | | <span data-ttu-id="388b3-421">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-421">✔</span></span> | <span data-ttu-id="388b3-422">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-422">✔</span></span>
<span data-ttu-id="388b3-423">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="388b3-423">LineString.StartPoint</span></span> | <span data-ttu-id="388b3-424">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-424">✔</span></span> | <span data-ttu-id="388b3-425">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-425">✔</span></span> | <span data-ttu-id="388b3-426">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-426">✔</span></span> | <span data-ttu-id="388b3-427">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-427">✔</span></span>
<span data-ttu-id="388b3-428">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="388b3-428">MultiLineString.IsClosed</span></span> | <span data-ttu-id="388b3-429">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-429">✔</span></span> | <span data-ttu-id="388b3-430">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-430">✔</span></span> | <span data-ttu-id="388b3-431">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-431">✔</span></span> | <span data-ttu-id="388b3-432">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-432">✔</span></span>
<span data-ttu-id="388b3-433">Punkt. M</span><span class="sxs-lookup"><span data-stu-id="388b3-433">Point.M</span></span> | <span data-ttu-id="388b3-434">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-434">✔</span></span> | <span data-ttu-id="388b3-435">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-435">✔</span></span> | <span data-ttu-id="388b3-436">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-436">✔</span></span> | <span data-ttu-id="388b3-437">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-437">✔</span></span>
<span data-ttu-id="388b3-438">Point. X</span><span class="sxs-lookup"><span data-stu-id="388b3-438">Point.X</span></span> | <span data-ttu-id="388b3-439">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-439">✔</span></span> | <span data-ttu-id="388b3-440">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-440">✔</span></span> | <span data-ttu-id="388b3-441">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-441">✔</span></span> | <span data-ttu-id="388b3-442">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-442">✔</span></span>
<span data-ttu-id="388b3-443">Punkt. Y</span><span class="sxs-lookup"><span data-stu-id="388b3-443">Point.Y</span></span> | <span data-ttu-id="388b3-444">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-444">✔</span></span> | <span data-ttu-id="388b3-445">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-445">✔</span></span> | <span data-ttu-id="388b3-446">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-446">✔</span></span> | <span data-ttu-id="388b3-447">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-447">✔</span></span>
<span data-ttu-id="388b3-448">Punkt. Z</span><span class="sxs-lookup"><span data-stu-id="388b3-448">Point.Z</span></span> | <span data-ttu-id="388b3-449">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-449">✔</span></span> | <span data-ttu-id="388b3-450">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-450">✔</span></span> | <span data-ttu-id="388b3-451">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-451">✔</span></span> | <span data-ttu-id="388b3-452">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-452">✔</span></span>
<span data-ttu-id="388b3-453">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="388b3-453">Polygon.ExteriorRing</span></span> | <span data-ttu-id="388b3-454">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-454">✔</span></span> | <span data-ttu-id="388b3-455">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-455">✔</span></span> | <span data-ttu-id="388b3-456">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-456">✔</span></span> | <span data-ttu-id="388b3-457">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-457">✔</span></span>
<span data-ttu-id="388b3-458">Polygon. getinteriorringn (int)</span><span class="sxs-lookup"><span data-stu-id="388b3-458">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="388b3-459">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-459">✔</span></span> | <span data-ttu-id="388b3-460">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-460">✔</span></span> | <span data-ttu-id="388b3-461">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-461">✔</span></span> | <span data-ttu-id="388b3-462">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-462">✔</span></span>
<span data-ttu-id="388b3-463">Polygon. numinteriorrings</span><span class="sxs-lookup"><span data-stu-id="388b3-463">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="388b3-464">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-464">✔</span></span> | <span data-ttu-id="388b3-465">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-465">✔</span></span> | <span data-ttu-id="388b3-466">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-466">✔</span></span> | <span data-ttu-id="388b3-467">✔</span><span class="sxs-lookup"><span data-stu-id="388b3-467">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="388b3-468">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="388b3-468">Additional resources</span></span>

* [<span data-ttu-id="388b3-469">Räumliche Daten in SQL Server</span><span class="sxs-lookup"><span data-stu-id="388b3-469">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="388b3-470">Spatialite-Homepage</span><span class="sxs-lookup"><span data-stu-id="388b3-470">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="388b3-471">Räumliche Dokumentation zu npgsql</span><span class="sxs-lookup"><span data-stu-id="388b3-471">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="388b3-472">PostGIS-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="388b3-472">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
