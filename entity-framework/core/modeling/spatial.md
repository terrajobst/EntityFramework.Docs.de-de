---
title: Räumliche Daten-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 5b45f83ca7f02665f52ccfe16b5af506a6046a62
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414704"
---
# <a name="spatial-data"></a><span data-ttu-id="99b3f-102">Räumliche Daten</span><span class="sxs-lookup"><span data-stu-id="99b3f-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="99b3f-103">Diese Funktion wurde in EF Core 2,2 hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="99b3f-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="99b3f-104">Räumliche Daten repräsentieren den physischen Speicherort und die Form von Objekten.</span><span class="sxs-lookup"><span data-stu-id="99b3f-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="99b3f-105">Viele Datenbanken unterstützen diese Art von Daten, damit Sie zusammen mit anderen Daten indiziert und abgefragt werden können.</span><span class="sxs-lookup"><span data-stu-id="99b3f-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="99b3f-106">Häufige Szenarien umfassen das Abfragen von Objekten in einer bestimmten Entfernung von einem Speicherort oder das Auswählen des Objekts, dessen Rahmen eine bestimmte Position enthält.</span><span class="sxs-lookup"><span data-stu-id="99b3f-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="99b3f-107">EF Core unterstützt die Zuordnung räumlicher Datentypen mithilfe der räumlichen Bibliothek [nettopologysuite](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="99b3f-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="99b3f-108">Installation</span><span class="sxs-lookup"><span data-stu-id="99b3f-108">Installing</span></span>

<span data-ttu-id="99b3f-109">Um räumliche Daten mit EF Core verwenden zu können, müssen Sie das entsprechende unterstützende nuget-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="99b3f-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="99b3f-110">Welches Paket installiert werden muss, hängt vom verwendeten Anbieter ab.</span><span class="sxs-lookup"><span data-stu-id="99b3f-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="99b3f-111">EF Core Anbieter</span><span class="sxs-lookup"><span data-stu-id="99b3f-111">EF Core Provider</span></span>                        | <span data-ttu-id="99b3f-112">Räumliches nuget-Paket</span><span class="sxs-lookup"><span data-stu-id="99b3f-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="99b3f-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="99b3f-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="99b3f-114">Microsoft. entityframeworkcore. SqlServer. nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="99b3f-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="99b3f-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="99b3f-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="99b3f-116">Microsoft. entityframeworkcore. sqlite. nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="99b3f-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="99b3f-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="99b3f-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="99b3f-118">Nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="99b3f-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="99b3f-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="99b3f-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="99b3f-120">Npgsql. entityframeworkcore. PostgreSQL. nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="99b3f-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="99b3f-121">Reverse Engineering</span><span class="sxs-lookup"><span data-stu-id="99b3f-121">Reverse engineering</span></span>

<span data-ttu-id="99b3f-122">Die räumlichen nuget-Pakete ermöglichen auch das [Reverse Engineering](../managing-schemas/scaffolding.md) von Modellen mit räumlichen Eigenschaften. Sie müssen das Paket jedoch ***vor*** dem Ausführen `Scaffold-DbContext` oder `dotnet ef dbcontext scaffold`installieren.</span><span class="sxs-lookup"><span data-stu-id="99b3f-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="99b3f-123">Wenn Sie dies nicht tun, erhalten Sie Warnungen, wenn Sie keine Typzuordnungen für die Spalten finden und die Spalten übersprungen werden.</span><span class="sxs-lookup"><span data-stu-id="99b3f-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="99b3f-124">Nettopologysuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="99b3f-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="99b3f-125">Nettopologysuite ist eine räumliche Bibliothek für .net.</span><span class="sxs-lookup"><span data-stu-id="99b3f-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="99b3f-126">EF Core ermöglicht die Zuordnung von räumlichen Datentypen in der Datenbank mithilfe von NTS-Typen in Ihrem Modell.</span><span class="sxs-lookup"><span data-stu-id="99b3f-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="99b3f-127">Um die Zuordnung räumlicher Typen über NTS zu ermöglichen, müssen Sie die usenettopologysuite-Methode für den dbcontext Options-Generator des Anbieters abrufen.</span><span class="sxs-lookup"><span data-stu-id="99b3f-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="99b3f-128">Beispielsweise können Sie mit SQL Server wie folgt bezeichnen.</span><span class="sxs-lookup"><span data-stu-id="99b3f-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="99b3f-129">Es gibt mehrere räumliche Datentypen.</span><span class="sxs-lookup"><span data-stu-id="99b3f-129">There are several spatial data types.</span></span> <span data-ttu-id="99b3f-130">Welcher Typ Sie verwenden, hängt von den Formen der Formen ab, die Sie zulassen möchten.</span><span class="sxs-lookup"><span data-stu-id="99b3f-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="99b3f-131">Hier ist die Hierarchie der NTS-Typen, die Sie für Eigenschaften im Modell verwenden können.</span><span class="sxs-lookup"><span data-stu-id="99b3f-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="99b3f-132">Sie befinden sich im `NetTopologySuite.Geometries`-Namespace.</span><span class="sxs-lookup"><span data-stu-id="99b3f-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="99b3f-133">Geometrie</span><span class="sxs-lookup"><span data-stu-id="99b3f-133">Geometry</span></span>
  * <span data-ttu-id="99b3f-134">Point</span><span class="sxs-lookup"><span data-stu-id="99b3f-134">Point</span></span>
  * <span data-ttu-id="99b3f-135">LineString</span><span class="sxs-lookup"><span data-stu-id="99b3f-135">LineString</span></span>
  * <span data-ttu-id="99b3f-136">Polygon</span><span class="sxs-lookup"><span data-stu-id="99b3f-136">Polygon</span></span>
  * <span data-ttu-id="99b3f-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="99b3f-137">GeometryCollection</span></span>
    * <span data-ttu-id="99b3f-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="99b3f-138">MultiPoint</span></span>
    * <span data-ttu-id="99b3f-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="99b3f-139">MultiLineString</span></span>
    * <span data-ttu-id="99b3f-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="99b3f-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="99b3f-141">Circularstring, compoundcurve und curepolygon werden von NTS nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="99b3f-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="99b3f-142">Mit dem basisgeometry-Typ kann jeder Typ von Form von der-Eigenschaft angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="99b3f-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="99b3f-143">Die folgenden Entitäts Klassen können verwendet werden, um Tabellen in der [Beispieldatenbank Wide World Importern](https://go.microsoft.com/fwlink/?LinkID=800630)zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="99b3f-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="99b3f-144">Erstellen von Werten</span><span class="sxs-lookup"><span data-stu-id="99b3f-144">Creating values</span></span>

<span data-ttu-id="99b3f-145">Sie können Konstruktoren zum Erstellen von Geometry-Objekten verwenden. Allerdings empfiehlt es sich, stattdessen eine Geometry-Factory zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="99b3f-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="99b3f-146">Auf diese Weise können Sie ein Standardmäßiges SRID angeben (das von den Koordinaten verwendete räumliche Verweis System) und Ihnen die Kontrolle über erweiterte Dinge wie das Genauigkeits Modell (verwendet während der Berechnungen) und die Koordinaten Sequenz (bestimmt, welche ordinates--Dimensionen und Measures--sind verfügbar).</span><span class="sxs-lookup"><span data-stu-id="99b3f-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="99b3f-147">4326 bezieht sich auf WGS 84, einen in GPS und anderen geografischen Systemen verwendeten Standard.</span><span class="sxs-lookup"><span data-stu-id="99b3f-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="99b3f-148">Längen-und Breitengrad</span><span class="sxs-lookup"><span data-stu-id="99b3f-148">Longitude and Latitude</span></span>

<span data-ttu-id="99b3f-149">Die Koordinaten in den NTS sind X-und Y-Werte.</span><span class="sxs-lookup"><span data-stu-id="99b3f-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="99b3f-150">Um Längen-und Breitengrad darzustellen, verwenden Sie X für Längengrad und Y für Breitengrad.</span><span class="sxs-lookup"><span data-stu-id="99b3f-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="99b3f-151">Beachten Sie, dass **sich diese** Werte nicht im `latitude, longitude` Format befinden, in dem normalerweise diese Werte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="99b3f-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="99b3f-152">SRID wird bei Client Vorgängen ignoriert.</span><span class="sxs-lookup"><span data-stu-id="99b3f-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="99b3f-153">NTS ignoriert SRID-Werte während des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="99b3f-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="99b3f-154">Es wird von einem planaren Koordinatensystem ausgegangen.</span><span class="sxs-lookup"><span data-stu-id="99b3f-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="99b3f-155">Dies bedeutet Folgendes: Wenn Sie Koordinaten in Bezug auf Längengrad und Breitengrad angeben, werden einige vom Client ausgewertete Werte wie "Distance", "length" und "Area" in Grad und nicht in "Meter" angegeben.</span><span class="sxs-lookup"><span data-stu-id="99b3f-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="99b3f-156">Wenn Sie aussagekräftigere Werte benötigen, müssen Sie die Koordinaten zunächst mithilfe einer Bibliothek wie [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) auf ein anderes Koordinatensystem projizieren, bevor Sie diese Werte berechnen.</span><span class="sxs-lookup"><span data-stu-id="99b3f-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="99b3f-157">Wenn ein Vorgang vom Server durch EF Core über SQL ausgewertet wird, wird die Einheit des Ergebnisses von der Datenbank bestimmt.</span><span class="sxs-lookup"><span data-stu-id="99b3f-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="99b3f-158">Im folgenden finden Sie ein Beispiel für die Verwendung von ProjNet4GeoAPI, um den Abstand zwischen zwei Städten zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="99b3f-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="99b3f-159">Abfrage von Daten</span><span class="sxs-lookup"><span data-stu-id="99b3f-159">Querying Data</span></span>

<span data-ttu-id="99b3f-160">In LINQ werden die NTS-Methoden und-Eigenschaften, die als Datenbankfunktionen verfügbar sind, in SQL übersetzt.</span><span class="sxs-lookup"><span data-stu-id="99b3f-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="99b3f-161">Beispielsweise werden die Distance-Methode und die-Methode in den folgenden Abfragen übersetzt.</span><span class="sxs-lookup"><span data-stu-id="99b3f-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="99b3f-162">Die Tabelle am Ende dieses Artikels zeigt, welche Member von verschiedenen EF Core Anbietern unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="99b3f-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="99b3f-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="99b3f-163">SQL Server</span></span>

<span data-ttu-id="99b3f-164">Wenn Sie SQL Server verwenden, müssen Sie einige zusätzliche Punkte beachten.</span><span class="sxs-lookup"><span data-stu-id="99b3f-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="99b3f-165">Geography oder Geometry</span><span class="sxs-lookup"><span data-stu-id="99b3f-165">Geography or geometry</span></span>

<span data-ttu-id="99b3f-166">Standardmäßig werden räumliche Eigenschaften `geography` Spalten in SQL Server zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="99b3f-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="99b3f-167">Um `geometry`zu verwenden, [Konfigurieren Sie den Spaltentyp](xref:core/modeling/entity-properties#column-data-types) in Ihrem Modell.</span><span class="sxs-lookup"><span data-stu-id="99b3f-167">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="99b3f-168">Polygon-Polygon Ringe</span><span class="sxs-lookup"><span data-stu-id="99b3f-168">Geography polygon rings</span></span>

<span data-ttu-id="99b3f-169">Wenn Sie den Spaltentyp `geography` verwenden, setzt SQL Server zusätzliche Anforderungen für den äußeren Ring (oder die Shell) und die inneren Ringe (oder Löcher).</span><span class="sxs-lookup"><span data-stu-id="99b3f-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="99b3f-170">Der äußere Ring muss gegen den Uhrzeigersinn und die inneren Ringe im Uhrzeigersinn ausgerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="99b3f-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="99b3f-171">NTS überprüft dies, bevor Werte an die Datenbank gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="99b3f-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="99b3f-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="99b3f-172">FullGlobe</span></span>

<span data-ttu-id="99b3f-173">SQL Server weist einen nicht standardmäßigen Geometry-Typ auf, der den vollständigen Globus darstellt, wenn der `geography` Spaltentyp verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="99b3f-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="99b3f-174">Außerdem bietet es eine Möglichkeit, Polygone auf der ganzen Welt (ohne äußeren Ring) darzustellen.</span><span class="sxs-lookup"><span data-stu-id="99b3f-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="99b3f-175">Keines dieser beiden wird von NTS unterstützt.</span><span class="sxs-lookup"><span data-stu-id="99b3f-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="99b3f-176">Fullglobe und Polygone, die darauf basieren, werden von NTS nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="99b3f-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="99b3f-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="99b3f-177">SQLite</span></span>

<span data-ttu-id="99b3f-178">Hier finden Sie einige zusätzliche Informationen für diejenigen, die SQLite verwenden.</span><span class="sxs-lookup"><span data-stu-id="99b3f-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="99b3f-179">Installieren von spatialite</span><span class="sxs-lookup"><span data-stu-id="99b3f-179">Installing SpatiaLite</span></span>

<span data-ttu-id="99b3f-180">Unter Windows wird die native mod_spatialite Bibliothek als eine nuget-Paketabhängigkeit verteilt.</span><span class="sxs-lookup"><span data-stu-id="99b3f-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="99b3f-181">Andere Plattformen müssen Sie separat installieren.</span><span class="sxs-lookup"><span data-stu-id="99b3f-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="99b3f-182">Dies erfolgt in der Regel mithilfe eines Softwarepaket-Managers.</span><span class="sxs-lookup"><span data-stu-id="99b3f-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="99b3f-183">Beispielsweise können Sie apt unter Ubuntu und Homebrew unter MacOS verwenden.</span><span class="sxs-lookup"><span data-stu-id="99b3f-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

<span data-ttu-id="99b3f-184">Leider sind neuere Versionen von proj (eine Abhängigkeit von spatialite) mit dem standardmäßigen [sqlitepclraw-Bundle](/dotnet/standard/data/sqlite/custom-versions#bundles)von EF nicht kompatibel.</span><span class="sxs-lookup"><span data-stu-id="99b3f-184">Unfortunately, newer versions of PROJ (a dependency of SpatiaLite) are incompatible with EF's default [SQLitePCLRaw bundle](/dotnet/standard/data/sqlite/custom-versions#bundles).</span></span> <span data-ttu-id="99b3f-185">Sie können dieses Problem umgehen, indem Sie entweder einen benutzerdefinierten [sqlitepclraw-Anbieter](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) erstellen, der die SQLite-Bibliothek des Systems verwendet, oder Sie können einen benutzerdefinierten Build von spatialite zum Deaktivieren der proj-Unterstützung installieren.</span><span class="sxs-lookup"><span data-stu-id="99b3f-185">You can work around this by either creating a custom [SQLitePCLRaw provider](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) that uses the system SQLite library, or you can install a custom build of SpatiaLite disabling PROJ support.</span></span>

``` sh
curl https://www.gaia-gis.it/gaia-sins/libspatialite-4.3.0a.tar.gz | tar -xz
cd libspatialite-4.3.0a

if [[ `uname -s` == Darwin* ]]; then
    # Mac OS requires some minor patching
    sed -i "" "s/shrext_cmds='\`test \\.\$module = .yes && echo .so \\|\\| echo \\.dylib\`'/shrext_cmds='.dylib'/g" configure
fi

./configure --disable-proj
make
make install
```

### <a name="configuring-srid"></a><span data-ttu-id="99b3f-186">Konfigurieren von SRID</span><span class="sxs-lookup"><span data-stu-id="99b3f-186">Configuring SRID</span></span>

<span data-ttu-id="99b3f-187">In spatialite müssen Spalten eine SRID pro Spalte angeben.</span><span class="sxs-lookup"><span data-stu-id="99b3f-187">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="99b3f-188">Der Standard-SRID ist `0`.</span><span class="sxs-lookup"><span data-stu-id="99b3f-188">The default SRID is `0`.</span></span> <span data-ttu-id="99b3f-189">Geben Sie einen anderen SRID mithilfe der forsqlitehassrid-Methode an.</span><span class="sxs-lookup"><span data-stu-id="99b3f-189">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="99b3f-190">Dimension</span><span class="sxs-lookup"><span data-stu-id="99b3f-190">Dimension</span></span>

<span data-ttu-id="99b3f-191">Ähnlich wie SRID wird auch die Dimension (oder ordinates) einer Spalte als Teil der Spalte angegeben.</span><span class="sxs-lookup"><span data-stu-id="99b3f-191">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="99b3f-192">Die Standard Ordinate sind X und Y. Aktivieren Sie zusätzliche Ordinate (Z und M) mithilfe der forsqlitehasdimension-Methode.</span><span class="sxs-lookup"><span data-stu-id="99b3f-192">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="99b3f-193">Übersetzte Vorgänge</span><span class="sxs-lookup"><span data-stu-id="99b3f-193">Translated Operations</span></span>

<span data-ttu-id="99b3f-194">Diese Tabelle zeigt, welche NTS-Elemente von den einzelnen EF Core Anbietern in SQL übersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="99b3f-194">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="99b3f-195">Nettopologysuite</span><span class="sxs-lookup"><span data-stu-id="99b3f-195">NetTopologySuite</span></span> | <span data-ttu-id="99b3f-196">SQL Server (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-196">SQL Server (geometry)</span></span> | <span data-ttu-id="99b3f-197">SQL Server (Geografie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-197">SQL Server (geography)</span></span> | <span data-ttu-id="99b3f-198">SQLite</span><span class="sxs-lookup"><span data-stu-id="99b3f-198">SQLite</span></span> | <span data-ttu-id="99b3f-199">Npgsql</span><span class="sxs-lookup"><span data-stu-id="99b3f-199">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="99b3f-200">Geometry. Area</span><span class="sxs-lookup"><span data-stu-id="99b3f-200">Geometry.Area</span></span> | <span data-ttu-id="99b3f-201">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-201">✔</span></span> | <span data-ttu-id="99b3f-202">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-202">✔</span></span> | <span data-ttu-id="99b3f-203">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-203">✔</span></span> | <span data-ttu-id="99b3f-204">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-204">✔</span></span>
<span data-ttu-id="99b3f-205">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="99b3f-205">Geometry.AsBinary()</span></span> | <span data-ttu-id="99b3f-206">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-206">✔</span></span> | <span data-ttu-id="99b3f-207">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-207">✔</span></span> | <span data-ttu-id="99b3f-208">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-208">✔</span></span> | <span data-ttu-id="99b3f-209">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-209">✔</span></span>
<span data-ttu-id="99b3f-210">Geometry. AsText ()</span><span class="sxs-lookup"><span data-stu-id="99b3f-210">Geometry.AsText()</span></span> | <span data-ttu-id="99b3f-211">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-211">✔</span></span> | <span data-ttu-id="99b3f-212">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-212">✔</span></span> | <span data-ttu-id="99b3f-213">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-213">✔</span></span> | <span data-ttu-id="99b3f-214">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-214">✔</span></span>
<span data-ttu-id="99b3f-215">Geometry. Boundary</span><span class="sxs-lookup"><span data-stu-id="99b3f-215">Geometry.Boundary</span></span> | <span data-ttu-id="99b3f-216">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-216">✔</span></span> | | <span data-ttu-id="99b3f-217">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-217">✔</span></span> | <span data-ttu-id="99b3f-218">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-218">✔</span></span>
<span data-ttu-id="99b3f-219">Geometry. Buffer (Double)</span><span class="sxs-lookup"><span data-stu-id="99b3f-219">Geometry.Buffer(double)</span></span> | <span data-ttu-id="99b3f-220">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-220">✔</span></span> | <span data-ttu-id="99b3f-221">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-221">✔</span></span> | <span data-ttu-id="99b3f-222">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-222">✔</span></span> | <span data-ttu-id="99b3f-223">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-223">✔</span></span>
<span data-ttu-id="99b3f-224">Geometry. Buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="99b3f-224">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="99b3f-225">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-225">✔</span></span> | <span data-ttu-id="99b3f-226">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-226">✔</span></span>
<span data-ttu-id="99b3f-227">Geometry. Centroid</span><span class="sxs-lookup"><span data-stu-id="99b3f-227">Geometry.Centroid</span></span> | <span data-ttu-id="99b3f-228">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-228">✔</span></span> | | <span data-ttu-id="99b3f-229">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-229">✔</span></span> | <span data-ttu-id="99b3f-230">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-230">✔</span></span>
<span data-ttu-id="99b3f-231">Geometry. enthält (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-231">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="99b3f-232">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-232">✔</span></span> | <span data-ttu-id="99b3f-233">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-233">✔</span></span> | <span data-ttu-id="99b3f-234">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-234">✔</span></span> | <span data-ttu-id="99b3f-235">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-235">✔</span></span>
<span data-ttu-id="99b3f-236">Geometry. config-Hülle ()</span><span class="sxs-lookup"><span data-stu-id="99b3f-236">Geometry.ConvexHull()</span></span> | <span data-ttu-id="99b3f-237">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-237">✔</span></span> | <span data-ttu-id="99b3f-238">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-238">✔</span></span> | <span data-ttu-id="99b3f-239">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-239">✔</span></span> | <span data-ttu-id="99b3f-240">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-240">✔</span></span>
<span data-ttu-id="99b3f-241">Geometry. coveredby (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-241">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="99b3f-242">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-242">✔</span></span> | <span data-ttu-id="99b3f-243">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-243">✔</span></span>
<span data-ttu-id="99b3f-244">Geometry. Cover (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-244">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="99b3f-245">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-245">✔</span></span> | <span data-ttu-id="99b3f-246">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-246">✔</span></span>
<span data-ttu-id="99b3f-247">Geometry. Kreuze (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-247">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="99b3f-248">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-248">✔</span></span> | | <span data-ttu-id="99b3f-249">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-249">✔</span></span> | <span data-ttu-id="99b3f-250">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-250">✔</span></span>
<span data-ttu-id="99b3f-251">Geometry. Difference (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-251">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="99b3f-252">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-252">✔</span></span> | <span data-ttu-id="99b3f-253">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-253">✔</span></span> | <span data-ttu-id="99b3f-254">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-254">✔</span></span> | <span data-ttu-id="99b3f-255">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-255">✔</span></span>
<span data-ttu-id="99b3f-256">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="99b3f-256">Geometry.Dimension</span></span> | <span data-ttu-id="99b3f-257">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-257">✔</span></span> | <span data-ttu-id="99b3f-258">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-258">✔</span></span> | <span data-ttu-id="99b3f-259">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-259">✔</span></span> | <span data-ttu-id="99b3f-260">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-260">✔</span></span>
<span data-ttu-id="99b3f-261">Geometry. disjoint (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-261">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="99b3f-262">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-262">✔</span></span> | <span data-ttu-id="99b3f-263">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-263">✔</span></span> | <span data-ttu-id="99b3f-264">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-264">✔</span></span> | <span data-ttu-id="99b3f-265">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-265">✔</span></span>
<span data-ttu-id="99b3f-266">Geometry. distance (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-266">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="99b3f-267">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-267">✔</span></span> | <span data-ttu-id="99b3f-268">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-268">✔</span></span> | <span data-ttu-id="99b3f-269">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-269">✔</span></span> | <span data-ttu-id="99b3f-270">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-270">✔</span></span>
<span data-ttu-id="99b3f-271">Geometry. Umschlag</span><span class="sxs-lookup"><span data-stu-id="99b3f-271">Geometry.Envelope</span></span> | <span data-ttu-id="99b3f-272">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-272">✔</span></span> | | <span data-ttu-id="99b3f-273">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-273">✔</span></span> | <span data-ttu-id="99b3f-274">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-274">✔</span></span>
<span data-ttu-id="99b3f-275">Geometry. EqualsExact (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-275">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="99b3f-276">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-276">✔</span></span>
<span data-ttu-id="99b3f-277">Geometry. equalstopologisch (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-277">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="99b3f-278">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-278">✔</span></span> | <span data-ttu-id="99b3f-279">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-279">✔</span></span> | <span data-ttu-id="99b3f-280">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-280">✔</span></span> | <span data-ttu-id="99b3f-281">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-281">✔</span></span>
<span data-ttu-id="99b3f-282">Geometry. geometrytype</span><span class="sxs-lookup"><span data-stu-id="99b3f-282">Geometry.GeometryType</span></span> | <span data-ttu-id="99b3f-283">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-283">✔</span></span> | <span data-ttu-id="99b3f-284">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-284">✔</span></span> | <span data-ttu-id="99b3f-285">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-285">✔</span></span> | <span data-ttu-id="99b3f-286">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-286">✔</span></span>
<span data-ttu-id="99b3f-287">Geometry. getgeometryn (int)</span><span class="sxs-lookup"><span data-stu-id="99b3f-287">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="99b3f-288">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-288">✔</span></span> | | <span data-ttu-id="99b3f-289">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-289">✔</span></span> | <span data-ttu-id="99b3f-290">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-290">✔</span></span>
<span data-ttu-id="99b3f-291">Geometry. interiorpoint</span><span class="sxs-lookup"><span data-stu-id="99b3f-291">Geometry.InteriorPoint</span></span> | <span data-ttu-id="99b3f-292">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-292">✔</span></span> | | <span data-ttu-id="99b3f-293">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-293">✔</span></span> | <span data-ttu-id="99b3f-294">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-294">✔</span></span>
<span data-ttu-id="99b3f-295">Geometry. Schnittmenge (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-295">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="99b3f-296">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-296">✔</span></span> | <span data-ttu-id="99b3f-297">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-297">✔</span></span> | <span data-ttu-id="99b3f-298">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-298">✔</span></span> | <span data-ttu-id="99b3f-299">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-299">✔</span></span>
<span data-ttu-id="99b3f-300">Geometry. intersekten (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-300">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="99b3f-301">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-301">✔</span></span> | <span data-ttu-id="99b3f-302">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-302">✔</span></span> | <span data-ttu-id="99b3f-303">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-303">✔</span></span> | <span data-ttu-id="99b3f-304">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-304">✔</span></span>
<span data-ttu-id="99b3f-305">Geometry. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="99b3f-305">Geometry.IsEmpty</span></span> | <span data-ttu-id="99b3f-306">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-306">✔</span></span> | <span data-ttu-id="99b3f-307">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-307">✔</span></span> | <span data-ttu-id="99b3f-308">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-308">✔</span></span> | <span data-ttu-id="99b3f-309">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-309">✔</span></span>
<span data-ttu-id="99b3f-310">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="99b3f-310">Geometry.IsSimple</span></span> | <span data-ttu-id="99b3f-311">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-311">✔</span></span> | | <span data-ttu-id="99b3f-312">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-312">✔</span></span> | <span data-ttu-id="99b3f-313">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-313">✔</span></span>
<span data-ttu-id="99b3f-314">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="99b3f-314">Geometry.IsValid</span></span> | <span data-ttu-id="99b3f-315">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-315">✔</span></span> | <span data-ttu-id="99b3f-316">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-316">✔</span></span> | <span data-ttu-id="99b3f-317">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-317">✔</span></span> | <span data-ttu-id="99b3f-318">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-318">✔</span></span>
<span data-ttu-id="99b3f-319">Geometry. iswithindistance (Geometry, Double)</span><span class="sxs-lookup"><span data-stu-id="99b3f-319">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="99b3f-320">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-320">✔</span></span> | | <span data-ttu-id="99b3f-321">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-321">✔</span></span> | <span data-ttu-id="99b3f-322">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-322">✔</span></span>
<span data-ttu-id="99b3f-323">Geometry. length</span><span class="sxs-lookup"><span data-stu-id="99b3f-323">Geometry.Length</span></span> | <span data-ttu-id="99b3f-324">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-324">✔</span></span> | <span data-ttu-id="99b3f-325">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-325">✔</span></span> | <span data-ttu-id="99b3f-326">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-326">✔</span></span> | <span data-ttu-id="99b3f-327">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-327">✔</span></span>
<span data-ttu-id="99b3f-328">Geometry. numgeometries</span><span class="sxs-lookup"><span data-stu-id="99b3f-328">Geometry.NumGeometries</span></span> | <span data-ttu-id="99b3f-329">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-329">✔</span></span> | <span data-ttu-id="99b3f-330">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-330">✔</span></span> | <span data-ttu-id="99b3f-331">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-331">✔</span></span> | <span data-ttu-id="99b3f-332">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-332">✔</span></span>
<span data-ttu-id="99b3f-333">Geometry. NumPoints</span><span class="sxs-lookup"><span data-stu-id="99b3f-333">Geometry.NumPoints</span></span> | <span data-ttu-id="99b3f-334">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-334">✔</span></span> | <span data-ttu-id="99b3f-335">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-335">✔</span></span> | <span data-ttu-id="99b3f-336">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-336">✔</span></span> | <span data-ttu-id="99b3f-337">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-337">✔</span></span>
<span data-ttu-id="99b3f-338">Geometry. ogcgeometrytype</span><span class="sxs-lookup"><span data-stu-id="99b3f-338">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="99b3f-339">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-339">✔</span></span> | <span data-ttu-id="99b3f-340">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-340">✔</span></span> | <span data-ttu-id="99b3f-341">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-341">✔</span></span> | <span data-ttu-id="99b3f-342">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-342">✔</span></span>
<span data-ttu-id="99b3f-343">Geometry. Überlappung (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-343">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="99b3f-344">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-344">✔</span></span> | <span data-ttu-id="99b3f-345">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-345">✔</span></span> | <span data-ttu-id="99b3f-346">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-346">✔</span></span> | <span data-ttu-id="99b3f-347">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-347">✔</span></span>
<span data-ttu-id="99b3f-348">Geometry. pointonsurface</span><span class="sxs-lookup"><span data-stu-id="99b3f-348">Geometry.PointOnSurface</span></span> | <span data-ttu-id="99b3f-349">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-349">✔</span></span> | | <span data-ttu-id="99b3f-350">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-350">✔</span></span> | <span data-ttu-id="99b3f-351">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-351">✔</span></span>
<span data-ttu-id="99b3f-352">Geometry. in Beziehung (Geometrie, Zeichenfolge)</span><span class="sxs-lookup"><span data-stu-id="99b3f-352">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="99b3f-353">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-353">✔</span></span> | | <span data-ttu-id="99b3f-354">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-354">✔</span></span> | <span data-ttu-id="99b3f-355">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-355">✔</span></span>
<span data-ttu-id="99b3f-356">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="99b3f-356">Geometry.Reverse()</span></span> | | | <span data-ttu-id="99b3f-357">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-357">✔</span></span> | <span data-ttu-id="99b3f-358">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-358">✔</span></span>
<span data-ttu-id="99b3f-359">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="99b3f-359">Geometry.SRID</span></span> | <span data-ttu-id="99b3f-360">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-360">✔</span></span> | <span data-ttu-id="99b3f-361">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-361">✔</span></span> | <span data-ttu-id="99b3f-362">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-362">✔</span></span> | <span data-ttu-id="99b3f-363">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-363">✔</span></span>
<span data-ttu-id="99b3f-364">Geometry. symmetricdifference (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-364">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="99b3f-365">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-365">✔</span></span> | <span data-ttu-id="99b3f-366">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-366">✔</span></span> | <span data-ttu-id="99b3f-367">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-367">✔</span></span> | <span data-ttu-id="99b3f-368">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-368">✔</span></span>
<span data-ttu-id="99b3f-369">Geometry. $ Binary ()</span><span class="sxs-lookup"><span data-stu-id="99b3f-369">Geometry.ToBinary()</span></span> | <span data-ttu-id="99b3f-370">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-370">✔</span></span> | <span data-ttu-id="99b3f-371">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-371">✔</span></span> | <span data-ttu-id="99b3f-372">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-372">✔</span></span> | <span data-ttu-id="99b3f-373">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-373">✔</span></span>
<span data-ttu-id="99b3f-374">Geometry.-Text ()</span><span class="sxs-lookup"><span data-stu-id="99b3f-374">Geometry.ToText()</span></span> | <span data-ttu-id="99b3f-375">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-375">✔</span></span> | <span data-ttu-id="99b3f-376">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-376">✔</span></span> | <span data-ttu-id="99b3f-377">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-377">✔</span></span> | <span data-ttu-id="99b3f-378">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-378">✔</span></span>
<span data-ttu-id="99b3f-379">Geometry. touches (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-379">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="99b3f-380">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-380">✔</span></span> | | <span data-ttu-id="99b3f-381">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-381">✔</span></span> | <span data-ttu-id="99b3f-382">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-382">✔</span></span>
<span data-ttu-id="99b3f-383">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="99b3f-383">Geometry.Union()</span></span> | | | <span data-ttu-id="99b3f-384">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-384">✔</span></span> | <span data-ttu-id="99b3f-385">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-385">✔</span></span>
<span data-ttu-id="99b3f-386">Geometry. Union (Geometrie)</span><span class="sxs-lookup"><span data-stu-id="99b3f-386">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="99b3f-387">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-387">✔</span></span> | <span data-ttu-id="99b3f-388">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-388">✔</span></span> | <span data-ttu-id="99b3f-389">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-389">✔</span></span> | <span data-ttu-id="99b3f-390">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-390">✔</span></span>
<span data-ttu-id="99b3f-391">Geometry. within (Geometry)</span><span class="sxs-lookup"><span data-stu-id="99b3f-391">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="99b3f-392">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-392">✔</span></span> | <span data-ttu-id="99b3f-393">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-393">✔</span></span> | <span data-ttu-id="99b3f-394">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-394">✔</span></span> | <span data-ttu-id="99b3f-395">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-395">✔</span></span>
<span data-ttu-id="99b3f-396">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="99b3f-396">GeometryCollection.Count</span></span> | <span data-ttu-id="99b3f-397">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-397">✔</span></span> | <span data-ttu-id="99b3f-398">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-398">✔</span></span> | <span data-ttu-id="99b3f-399">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-399">✔</span></span> | <span data-ttu-id="99b3f-400">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-400">✔</span></span>
<span data-ttu-id="99b3f-401">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="99b3f-401">GeometryCollection[int]</span></span> | <span data-ttu-id="99b3f-402">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-402">✔</span></span> | <span data-ttu-id="99b3f-403">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-403">✔</span></span> | <span data-ttu-id="99b3f-404">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-404">✔</span></span> | <span data-ttu-id="99b3f-405">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-405">✔</span></span>
<span data-ttu-id="99b3f-406">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="99b3f-406">LineString.Count</span></span> | <span data-ttu-id="99b3f-407">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-407">✔</span></span> | <span data-ttu-id="99b3f-408">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-408">✔</span></span> | <span data-ttu-id="99b3f-409">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-409">✔</span></span> | <span data-ttu-id="99b3f-410">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-410">✔</span></span>
<span data-ttu-id="99b3f-411">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="99b3f-411">LineString.EndPoint</span></span> | <span data-ttu-id="99b3f-412">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-412">✔</span></span> | <span data-ttu-id="99b3f-413">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-413">✔</span></span> | <span data-ttu-id="99b3f-414">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-414">✔</span></span> | <span data-ttu-id="99b3f-415">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-415">✔</span></span>
<span data-ttu-id="99b3f-416">LineString. getpointn (int)</span><span class="sxs-lookup"><span data-stu-id="99b3f-416">LineString.GetPointN(int)</span></span> | <span data-ttu-id="99b3f-417">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-417">✔</span></span> | <span data-ttu-id="99b3f-418">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-418">✔</span></span> | <span data-ttu-id="99b3f-419">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-419">✔</span></span> | <span data-ttu-id="99b3f-420">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-420">✔</span></span>
<span data-ttu-id="99b3f-421">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="99b3f-421">LineString.IsClosed</span></span> | <span data-ttu-id="99b3f-422">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-422">✔</span></span> | <span data-ttu-id="99b3f-423">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-423">✔</span></span> | <span data-ttu-id="99b3f-424">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-424">✔</span></span> | <span data-ttu-id="99b3f-425">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-425">✔</span></span>
<span data-ttu-id="99b3f-426">LineString. isring</span><span class="sxs-lookup"><span data-stu-id="99b3f-426">LineString.IsRing</span></span> | <span data-ttu-id="99b3f-427">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-427">✔</span></span> | | <span data-ttu-id="99b3f-428">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-428">✔</span></span> | <span data-ttu-id="99b3f-429">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-429">✔</span></span>
<span data-ttu-id="99b3f-430">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="99b3f-430">LineString.StartPoint</span></span> | <span data-ttu-id="99b3f-431">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-431">✔</span></span> | <span data-ttu-id="99b3f-432">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-432">✔</span></span> | <span data-ttu-id="99b3f-433">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-433">✔</span></span> | <span data-ttu-id="99b3f-434">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-434">✔</span></span>
<span data-ttu-id="99b3f-435">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="99b3f-435">MultiLineString.IsClosed</span></span> | <span data-ttu-id="99b3f-436">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-436">✔</span></span> | <span data-ttu-id="99b3f-437">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-437">✔</span></span> | <span data-ttu-id="99b3f-438">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-438">✔</span></span> | <span data-ttu-id="99b3f-439">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-439">✔</span></span>
<span data-ttu-id="99b3f-440">Punkt. M</span><span class="sxs-lookup"><span data-stu-id="99b3f-440">Point.M</span></span> | <span data-ttu-id="99b3f-441">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-441">✔</span></span> | <span data-ttu-id="99b3f-442">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-442">✔</span></span> | <span data-ttu-id="99b3f-443">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-443">✔</span></span> | <span data-ttu-id="99b3f-444">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-444">✔</span></span>
<span data-ttu-id="99b3f-445">Point. X</span><span class="sxs-lookup"><span data-stu-id="99b3f-445">Point.X</span></span> | <span data-ttu-id="99b3f-446">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-446">✔</span></span> | <span data-ttu-id="99b3f-447">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-447">✔</span></span> | <span data-ttu-id="99b3f-448">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-448">✔</span></span> | <span data-ttu-id="99b3f-449">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-449">✔</span></span>
<span data-ttu-id="99b3f-450">Punkt. Y</span><span class="sxs-lookup"><span data-stu-id="99b3f-450">Point.Y</span></span> | <span data-ttu-id="99b3f-451">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-451">✔</span></span> | <span data-ttu-id="99b3f-452">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-452">✔</span></span> | <span data-ttu-id="99b3f-453">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-453">✔</span></span> | <span data-ttu-id="99b3f-454">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-454">✔</span></span>
<span data-ttu-id="99b3f-455">Punkt. Z</span><span class="sxs-lookup"><span data-stu-id="99b3f-455">Point.Z</span></span> | <span data-ttu-id="99b3f-456">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-456">✔</span></span> | <span data-ttu-id="99b3f-457">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-457">✔</span></span> | <span data-ttu-id="99b3f-458">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-458">✔</span></span> | <span data-ttu-id="99b3f-459">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-459">✔</span></span>
<span data-ttu-id="99b3f-460">Polygon. ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="99b3f-460">Polygon.ExteriorRing</span></span> | <span data-ttu-id="99b3f-461">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-461">✔</span></span> | <span data-ttu-id="99b3f-462">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-462">✔</span></span> | <span data-ttu-id="99b3f-463">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-463">✔</span></span> | <span data-ttu-id="99b3f-464">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-464">✔</span></span>
<span data-ttu-id="99b3f-465">Polygon. getinteriorringn (int)</span><span class="sxs-lookup"><span data-stu-id="99b3f-465">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="99b3f-466">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-466">✔</span></span> | <span data-ttu-id="99b3f-467">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-467">✔</span></span> | <span data-ttu-id="99b3f-468">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-468">✔</span></span> | <span data-ttu-id="99b3f-469">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-469">✔</span></span>
<span data-ttu-id="99b3f-470">Polygon. numinteriorrings</span><span class="sxs-lookup"><span data-stu-id="99b3f-470">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="99b3f-471">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-471">✔</span></span> | <span data-ttu-id="99b3f-472">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-472">✔</span></span> | <span data-ttu-id="99b3f-473">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-473">✔</span></span> | <span data-ttu-id="99b3f-474">✔</span><span class="sxs-lookup"><span data-stu-id="99b3f-474">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="99b3f-475">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="99b3f-475">Additional resources</span></span>

* [<span data-ttu-id="99b3f-476">Räumliche Daten in SQL Server</span><span class="sxs-lookup"><span data-stu-id="99b3f-476">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="99b3f-477">Spatialite-Homepage</span><span class="sxs-lookup"><span data-stu-id="99b3f-477">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="99b3f-478">Räumliche Dokumentation zu npgsql</span><span class="sxs-lookup"><span data-stu-id="99b3f-478">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="99b3f-479">PostGIS-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="99b3f-479">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
