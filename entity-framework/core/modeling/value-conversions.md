---
title: Wertkonvertierungen – EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: d5189cef6d44fdf3fd6116a2952ce07ff3a389d4
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614398"
---
# <a name="value-conversions"></a><span data-ttu-id="6ea62-102">Wertkonvertierungen</span><span class="sxs-lookup"><span data-stu-id="6ea62-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="6ea62-103">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="6ea62-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="6ea62-104">Wertkonverter ermöglichen die konvertiert werden, wenn die Variable gelesen oder geschrieben werden, in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="6ea62-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="6ea62-105">Diese Konvertierung kann von einem Wert in eine andere vom selben Typ (z. B. Verschlüsselung Zeichenfolgen) oder aus einem Wert eines Typs in einen Wert eines anderen Typs (z. B. Konvertieren von Enum-Werte in und aus Zeichenfolgen in der Datenbank.)</span><span class="sxs-lookup"><span data-stu-id="6ea62-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="6ea62-106">Grundlagen</span><span class="sxs-lookup"><span data-stu-id="6ea62-106">Fundamentals</span></span>

<span data-ttu-id="6ea62-107">Wertkonverter sind hinsichtlich der angegebenen ein `ModelClrType` und `ProviderClrType`.</span><span class="sxs-lookup"><span data-stu-id="6ea62-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="6ea62-108">Der Typ des Modells ist der Typ .NET die Eigenschaft im Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="6ea62-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="6ea62-109">Der Anbietertyp ist der .NET-Typ, der vom Datenbankanbieter verstanden.</span><span class="sxs-lookup"><span data-stu-id="6ea62-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="6ea62-110">Z. B. um Enumerationen als Zeichenfolgen in der Datenbank zu speichern, der Typ des Modells ist der Typ der Enumeration; Anbietertyp ist `String`.</span><span class="sxs-lookup"><span data-stu-id="6ea62-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="6ea62-111">Diese beiden Typen können identisch sein.</span><span class="sxs-lookup"><span data-stu-id="6ea62-111">These two types can be the same.</span></span>

<span data-ttu-id="6ea62-112">Konvertierungen werden definiert, unter Verwendung zweier `Func` Ausdrucksbaumstrukturen: von `ModelClrType` zu `ProviderClrType` und das andere von `ProviderClrType` zu `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="6ea62-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="6ea62-113">Ausdrucksbaumstrukturen werden verwendet, sodass sie in der Datenbank-Zugriffscode für effiziente Konvertierungen kompiliert werden können.</span><span class="sxs-lookup"><span data-stu-id="6ea62-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="6ea62-114">Bei komplexen Konvertierungen wird möglicherweise die Ausdrucksbaumstruktur einen einfachen Aufruf an eine Methode, die die Konvertierung durchführt.</span><span class="sxs-lookup"><span data-stu-id="6ea62-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="6ea62-115">Konfigurieren einen Wertkonverter</span><span class="sxs-lookup"><span data-stu-id="6ea62-115">Configuring a value converter</span></span>

<span data-ttu-id="6ea62-116">Konvertierungen werden in Eigenschaften definiert die `OnModelCreating` von Ihrem `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6ea62-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="6ea62-117">Betrachten Sie beispielsweise einen Typ für Enumerationen und Entität, die als definiert:</span><span class="sxs-lookup"><span data-stu-id="6ea62-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="6ea62-118">Konvertierungen in definiert werden, können `OnModelCreating` Enum-Werte als Zeichenfolgen (z. B. "Donkey", "Mule",...) in der Datenbank zu speichern:</span><span class="sxs-lookup"><span data-stu-id="6ea62-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="6ea62-119">Ein `null` Wert wird nie auf einen Wertkonverter übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="6ea62-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="6ea62-120">Dies erleichtert die Implementierung der Konvertierungen und ermöglicht es ihnen, die von NULL-Werte zulässt und nicht auf NULL festlegbare Eigenschaften gemeinsam genutzt werden.</span><span class="sxs-lookup"><span data-stu-id="6ea62-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="6ea62-121">Die ValueConverter-Klasse</span><span class="sxs-lookup"><span data-stu-id="6ea62-121">The ValueConverter class</span></span>

<span data-ttu-id="6ea62-122">Aufrufen von `HasConversion` wie oben gezeigt erstellt eine `ValueConverter` -Instanz und für die Eigenschaft festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="6ea62-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="6ea62-123">Die `ValueConverter` stattdessen explizit erstellt werden können.</span><span class="sxs-lookup"><span data-stu-id="6ea62-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="6ea62-124">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6ea62-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="6ea62-125">Dies kann nützlich sein, wenn mehrere Eigenschaften auf die gleiche Konvertierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="6ea62-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="6ea62-126">Derzeit besteht keine Möglichkeit, um an einem Ort anzugeben, dass jede Eigenschaft eines bestimmten Typs den gleichen Wertkonverter verwenden muss.</span><span class="sxs-lookup"><span data-stu-id="6ea62-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="6ea62-127">Dieses Feature wird für ein zukünftiges Release berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="6ea62-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="6ea62-128">Integrierte Konverter</span><span class="sxs-lookup"><span data-stu-id="6ea62-128">Built-in converters</span></span>

<span data-ttu-id="6ea62-129">EF Core umfasst eine Reihe von vordefinierten `ValueConverter` Klassen finden Sie in der `Microsoft.EntityFrameworkCore.Storage.ValueConversion` Namespace.</span><span class="sxs-lookup"><span data-stu-id="6ea62-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="6ea62-130">Diese lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="6ea62-130">These are:</span></span>
* <span data-ttu-id="6ea62-131">`BoolToZeroOneConverter` -"Bool" an, und 0 (null)</span><span class="sxs-lookup"><span data-stu-id="6ea62-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="6ea62-132">`BoolToStringConverter` -"Bool", um Zeichenfolgen wie "Y" und "N"</span><span class="sxs-lookup"><span data-stu-id="6ea62-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="6ea62-133">`BoolToTwoValuesConverter` -"Bool", um zwei Werten</span><span class="sxs-lookup"><span data-stu-id="6ea62-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="6ea62-134">`BytesToStringConverter` -Byte-Array in Base64-codierte Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="6ea62-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="6ea62-135">`CastingConverter` -Konvertierungen, die nur eine C#-Umwandlung erfordern.</span><span class="sxs-lookup"><span data-stu-id="6ea62-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="6ea62-136">`CharToStringConverter` -Von char, um die einzelnen Zeichen bestehende Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="6ea62-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="6ea62-137">`DateTimeOffsetToBinaryConverter` -DateTimeOffset Binär codierte 64-Bit-Wert</span><span class="sxs-lookup"><span data-stu-id="6ea62-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="6ea62-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset Byte-array</span><span class="sxs-lookup"><span data-stu-id="6ea62-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="6ea62-139">`DateTimeOffsetToStringConverter` -DateTimeOffset Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="6ea62-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="6ea62-140">`DateTimeToBinaryConverter` – DateTime, DateTimeKind einschließlich 64-Bit-Wert</span><span class="sxs-lookup"><span data-stu-id="6ea62-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="6ea62-141">`DateTimeToStringConverter` -DateTime Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="6ea62-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="6ea62-142">`DateTimeToTicksConverter` -DateTime in ticks</span><span class="sxs-lookup"><span data-stu-id="6ea62-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="6ea62-143">`EnumToNumberConverter` -Anzahl der zugrunde liegenden Enumeration</span><span class="sxs-lookup"><span data-stu-id="6ea62-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="6ea62-144">`EnumToStringConverter` -Zeichenfolge Enumeration</span><span class="sxs-lookup"><span data-stu-id="6ea62-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="6ea62-145">`GuidToBytesConverter` -Guid in Byte-array</span><span class="sxs-lookup"><span data-stu-id="6ea62-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="6ea62-146">`GuidToStringConverter` -Guid Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="6ea62-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="6ea62-147">`NumberToBytesConverter` – Alle numerischen Wert in Byte-array</span><span class="sxs-lookup"><span data-stu-id="6ea62-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="6ea62-148">`NumberToStringConverter` – Alle numerischen Wert in eine Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="6ea62-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="6ea62-149">`StringToBytesConverter` -Von string in UTF8-bytes</span><span class="sxs-lookup"><span data-stu-id="6ea62-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="6ea62-150">`TimeSpanToStringConverter` -TimeSpan Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="6ea62-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="6ea62-151">`TimeSpanToTicksConverter` -TimeSpan Ticks</span><span class="sxs-lookup"><span data-stu-id="6ea62-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="6ea62-152">Beachten Sie, dass `EnumToStringConverter` in dieser Liste enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="6ea62-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="6ea62-153">Dies bedeutet, dass keine Notwendigkeit besteht, wie oben gezeigt, explizit angeben, die Konvertierung.</span><span class="sxs-lookup"><span data-stu-id="6ea62-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="6ea62-154">Verwenden Sie stattdessen einfach die integrierten Konverter:</span><span class="sxs-lookup"><span data-stu-id="6ea62-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="6ea62-155">Beachten Sie, dass alle integrierten Konverter zustandslos sind und daher eine einzelne Instanz sicher für mehrere Eigenschaften gemeinsam an.</span><span class="sxs-lookup"><span data-stu-id="6ea62-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="6ea62-156">Vordefinierte Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="6ea62-156">Pre-defined conversions</span></span>

<span data-ttu-id="6ea62-157">Für allgemeine Konvertierungen, die für die integrierten Konverter vorhanden ist ist es nicht notwendig, den Konverter explizit anzugeben.</span><span class="sxs-lookup"><span data-stu-id="6ea62-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="6ea62-158">Stattdessen einfach konfigurieren, welcher Typ verwendet werden soll, und EF verwendet automatisch den entsprechenden integrierten Konverter.</span><span class="sxs-lookup"><span data-stu-id="6ea62-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="6ea62-159">Enumeration, zeichenfolgenkonvertierungen werden als Beispiel oben verwendet, aber EF übernimmt dies automatisch, wenn der Anbietertyp konfiguriert ist:</span><span class="sxs-lookup"><span data-stu-id="6ea62-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="6ea62-160">Dasselbe kann durch explizites Angeben des Spaltentyps erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="6ea62-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="6ea62-161">Wenn der Entitätstyp definiert ist z.B. so:</span><span class="sxs-lookup"><span data-stu-id="6ea62-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="6ea62-162">Und dann die Enum-Werte als Zeichenfolgen in der Datenbank ohne weitere Konfiguration in gespeichert werden `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="6ea62-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="6ea62-163">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="6ea62-163">Limitations</span></span>

<span data-ttu-id="6ea62-164">Es gibt einige bekannte aktuelle Einschränkungen des Systems Konvertierung Wert:</span><span class="sxs-lookup"><span data-stu-id="6ea62-164">There are a few known current limitations of the value conversion system:</span></span>
* <span data-ttu-id="6ea62-165">Wie bereits erwähnt, `null` kann nicht konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="6ea62-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="6ea62-166">Derzeit besteht keine Möglichkeit, eine Konvertierung von einer Eigenschaft in mehrere Spalten oder umgekehrt zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="6ea62-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="6ea62-167">Verwendung von Konvertierungen kann es sich um die Fähigkeit von EF Core, die in SQL übersetzen auswirken.</span><span class="sxs-lookup"><span data-stu-id="6ea62-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="6ea62-168">Für solche Fälle wird eine Warnung protokolliert werden.</span><span class="sxs-lookup"><span data-stu-id="6ea62-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="6ea62-169">Zum Entfernen dieser Einschränkungen ist für eine spätere Version in Betracht gezogen.</span><span class="sxs-lookup"><span data-stu-id="6ea62-169">Removal of these limitations is being considered for a future release.</span></span>
