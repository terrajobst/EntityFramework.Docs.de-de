---
title: Wertkonvertierungen - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 3e97c05a87ad9b4817c03f446031ea6c74704f5b
ms.sourcegitcommit: 605e42232854ce44bae09624a6eebc35b8e2473b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
---
# <a name="value-conversions"></a><span data-ttu-id="d4ea7-102">Wertkonvertierungen</span><span class="sxs-lookup"><span data-stu-id="d4ea7-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="d4ea7-103">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="d4ea7-104">Wertkonverter können Eigenschaftswerte beim Lesen oder Schreiben in die Datenbank konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="d4ea7-105">Diese Konvertierung kann von einem Wert in eine andere des gleichen Typs (z. B. zum Verschlüsseln verwendete Zeichenfolgen) oder aus einem Wert eines Typs in einen Wert eines anderen Typs (z. B. Konvertieren von Enum-Werte in und aus Zeichenfolgen in der Datenbank.)</span><span class="sxs-lookup"><span data-stu-id="d4ea7-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="d4ea7-106">Grundlagen</span><span class="sxs-lookup"><span data-stu-id="d4ea7-106">Fundamentals</span></span>

<span data-ttu-id="d4ea7-107">Wertkonverter werden angegeben, der eine `ModelClrType` und ein `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="d4ea7-108">Das Modell ist der .NET Typ der Eigenschaft in der Entität vom Typ.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="d4ea7-109">Der Anbieter ist der .NET-Typ, von dem Datenbankanbieter verstanden.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="d4ea7-110">Beispielsweise um Enumerationen als Zeichenfolgen in der Datenbank zu speichern, der Modelltyp ist der Typ der Enumeration und der Anbietertyp ist `String`.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="d4ea7-111">Diese beiden Typen können identisch sein.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-111">These two types can be the same.</span></span>

<span data-ttu-id="d4ea7-112">Konvertierungen werden definiert über zwei `Func` Ausdrucksbaumstrukturen: aus `ModelClrType` auf `ProviderClrType` und das andere von `ProviderClrType` auf `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="d4ea7-113">Ausdrucksbaumstrukturen werden verwendet, sodass sie in den Datenbank-Access-Code für die effiziente Konvertierung kompiliert werden können.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="d4ea7-114">Bei komplexen Konvertierungen wird möglicherweise die Ausdrucksbaumstruktur einen einfachen Aufruf einer Methode, die die Konvertierung ausführt.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="d4ea7-115">Konfigurieren einen Wertkonverter</span><span class="sxs-lookup"><span data-stu-id="d4ea7-115">Configuring a value converter</span></span>

<span data-ttu-id="d4ea7-116">Für Eigenschaften in der OnModelCreating von Ihrem DbContext sind wertkonvertierungen definiert.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-116">Value conversions are defined on properties in the OnModelCreating of your DbContext.</span></span> <span data-ttu-id="d4ea7-117">Betrachten Sie beispielsweise einen Enum und Entity-Typ, definiert als:</span><span class="sxs-lookup"><span data-stu-id="d4ea7-117">For example, consider an enum and entity type defined as:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }
    public EquineBeast Mount { get; set; }
}

public enum EquineBeast
{
    Donkey,
    Mule,
    Horse,
    Unicorn
}
```
<span data-ttu-id="d4ea7-118">Klicken Sie dann können in OnModelCreating (z. B. die Enumerationswerte als Zeichenfolgen speichern Konvertierungen definiert werden "Donkey", "Maultieren",...) in der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="d4ea7-118">Then conversions can be defined in OnModelCreating to store the enum values as strings (e.g. "Donkey", "Mule", ...) in the database:</span></span>
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Rider>()
        .Property(e => e.Mount)
        .HasConversion(
            v => v.ToString(),
            v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));
}
```
> [!NOTE]  
> <span data-ttu-id="d4ea7-119">Ein `null` Wert wird nie ein Wertkonverter übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="d4ea7-120">Dies vereinfacht die Implementierung der Konvertierungen und ermöglicht es ihnen, unter der NULL-Werte zulässt und nicht auf NULL festlegbare Eigenschaften freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="d4ea7-121">Die ValueConverter-Klasse</span><span class="sxs-lookup"><span data-stu-id="d4ea7-121">The ValueConverter class</span></span>

<span data-ttu-id="d4ea7-122">Aufrufen von `HasConversion` wie oben gezeigt erstellt eine `ValueConverter` -Instanz, und legen Sie es auf die Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="d4ea7-123">Die `ValueConverter` stattdessen explizit erstellt werden können.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="d4ea7-124">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d4ea7-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="d4ea7-125">Dies kann nützlich sein, wenn mehrere Eigenschaften derselben Konvertierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="d4ea7-126">Zurzeit besteht keine Möglichkeit, um an einem Ort anzugeben, dass jede Eigenschaft eines bestimmten Typs auf den gleichen Wertkonverter verwenden muss.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="d4ea7-127">Diese Funktion wird für eine künftige Version berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="d4ea7-128">Integrierte Konverter</span><span class="sxs-lookup"><span data-stu-id="d4ea7-128">Built-in converters</span></span>

<span data-ttu-id="d4ea7-129">EF Core umfasst eine Reihe von vordefinierten `ValueConverter` Klassen aus, die `Microsoft.EntityFrameworkCore.Storage.ValueConversion` Namespace.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="d4ea7-130">Diese lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d4ea7-130">These are:</span></span>
* <span data-ttu-id="d4ea7-131">`BoolToZeroOneConverter` -Bool 0 (null) und 1</span><span class="sxs-lookup"><span data-stu-id="d4ea7-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="d4ea7-132">`BoolToStringConverter` -Bool Zeichenfolgen, z. B. "Y" und "N"</span><span class="sxs-lookup"><span data-stu-id="d4ea7-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="d4ea7-133">`BoolToTwoValuesConverter` -Bool zwei Werten</span><span class="sxs-lookup"><span data-stu-id="d4ea7-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="d4ea7-134">`BytesToStringConverter` -Byte Array in Base64-codierte Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="d4ea7-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="d4ea7-135">`CastingConverter` -Konvertierungen, die nur eine C#-Umwandlung erfordern.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="d4ea7-136">`CharToStringConverter` -Von char, um die einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="d4ea7-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="d4ea7-137">`DateTimeOffsetToBinaryConverter` -"DateTimeOffset", um binärcodierten 64-Bit-Wert</span><span class="sxs-lookup"><span data-stu-id="d4ea7-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="d4ea7-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset Byte-array</span><span class="sxs-lookup"><span data-stu-id="d4ea7-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="d4ea7-139">`DateTimeOffsetToStringConverter` -DateTimeOffset Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="d4ea7-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="d4ea7-140">`DateTimeToBinaryConverter` -"DateTime", auf 64-Bit-Wert, einschließlich DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="d4ea7-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="d4ea7-141">`DateTimeToStringConverter` -DateTime Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="d4ea7-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="d4ea7-142">`DateTimeToTicksConverter` -"DateTime", um die Teilstriche</span><span class="sxs-lookup"><span data-stu-id="d4ea7-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="d4ea7-143">`EnumToNumberConverter` -Anzahl der zugrunde liegenden Enumeration</span><span class="sxs-lookup"><span data-stu-id="d4ea7-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="d4ea7-144">`EnumToStringConverter` -Enum Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="d4ea7-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="d4ea7-145">`GuidToBytesConverter` -Guid Byte-array</span><span class="sxs-lookup"><span data-stu-id="d4ea7-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="d4ea7-146">`GuidToStringConverter` -Guid Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="d4ea7-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="d4ea7-147">`NumberToBytesConverter` – Beliebige numerischen Wert in Bytearray</span><span class="sxs-lookup"><span data-stu-id="d4ea7-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="d4ea7-148">`NumberToStringConverter` – Beliebige numerischen Wert in eine Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="d4ea7-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="d4ea7-149">`StringToBytesConverter` -Zeichenfolge auf UTF8-bytes</span><span class="sxs-lookup"><span data-stu-id="d4ea7-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="d4ea7-150">`TimeSpanToStringConverter` -TimeSpan Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="d4ea7-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="d4ea7-151">`TimeSpanToTicksConverter` -TimeSpan Ticks</span><span class="sxs-lookup"><span data-stu-id="d4ea7-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="d4ea7-152">Beachten Sie, dass `EnumToStringConverter` in dieser Liste enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="d4ea7-153">Dies bedeutet, dass keine Notwendigkeit besteht, wie oben gezeigt, explizit angeben, die Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="d4ea7-154">Verwenden Sie stattdessen nur das integrierte Konverter:</span><span class="sxs-lookup"><span data-stu-id="d4ea7-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="d4ea7-155">Beachten Sie, dass die integrierten Konverter zustandslos sind und daher eine einzelne Instanz problemlos von gemeinsam werden mehrere Eigenschaften genutzt kann.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="d4ea7-156">Vordefinierte Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="d4ea7-156">Pre-defined conversions</span></span>

<span data-ttu-id="d4ea7-157">Für allgemeine Konvertierungen, die für die integrierten Konverter vorhanden ist ist es nicht notwendig, den Konverter explizit anzugeben.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="d4ea7-158">Stattdessen nur konfigurieren, welche Art von Anbieter verwendet werden soll und EF verwendet automatisch den entsprechenden Build Konverter.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate build-in converter.</span></span> <span data-ttu-id="d4ea7-159">Enumeration zeichenfolgenkonvertierungen dienen als Beispiel oben, aber EF tatsächlich erledigen dies automatisch, wenn der Anbietertyp konfiguriert ist:</span><span class="sxs-lookup"><span data-stu-id="d4ea7-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="d4ea7-160">Dasselbe kann durch die explizite Angabe den Spaltentyp erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="d4ea7-161">Wenn der Entitätstyp definiert ist z. B. wie so:</span><span class="sxs-lookup"><span data-stu-id="d4ea7-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="d4ea7-162">Anschließend werden die Enumerationswerte als Zeichenfolgen in der Datenbank ohne weitere Konfiguration in der OnModelCreating gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-162">Then the enum values will be saved as strings in the database without any further configuration in OnModelCreating.</span></span>

## <a name="limitations"></a><span data-ttu-id="d4ea7-163">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="d4ea7-163">Limitations</span></span>

<span data-ttu-id="d4ea7-164">Es gibt einige bekannte aktuelle Einschränkungen des Systems Wert konvertiert:</span><span class="sxs-lookup"><span data-stu-id="d4ea7-164">There are a few known current limitations of the value convertion system:</span></span>
* <span data-ttu-id="d4ea7-165">Wie bereits erwähnt `null` kann nicht konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="d4ea7-166">Zurzeit besteht keine Möglichkeit, um eine Konvertierung einer Eigenschaft in mehrere Spalten oder umgekehrt zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="d4ea7-167">Verwendung von wertkonvertierungen kann die Fähigkeit des EF Core, die in SQL übersetzen auswirken.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="d4ea7-168">In solchen Fällen wird eine Warnung protokolliert.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="d4ea7-169">Entfernen von diesen Einschränkungen wird für eine künftige Version berücksichtigt wird.</span><span class="sxs-lookup"><span data-stu-id="d4ea7-169">Removal of these limitations is being considered for a future release.</span></span>
