---
title: Wert Konvertierungen-EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654785"
---
# <a name="value-conversions"></a><span data-ttu-id="ae1b1-102">Wertkonvertierungen</span><span class="sxs-lookup"><span data-stu-id="ae1b1-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="ae1b1-103">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="ae1b1-104">Mithilfe von Wert Konvertern können Eigenschaftswerte beim Lesen aus der Datenbank oder beim Schreiben in die Datenbank konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="ae1b1-105">Diese Konvertierung kann von einem Wert in einen anderen desselben Typs (z. b. das Verschlüsseln von Zeichen folgen) oder von einem Wert eines Typs zu einem Wert eines anderen Typs erfolgen (z. b. beim Konvertieren von Enumerationswerten in und aus Zeichen folgen in der Datenbank).</span><span class="sxs-lookup"><span data-stu-id="ae1b1-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="ae1b1-106">Grundlagen</span><span class="sxs-lookup"><span data-stu-id="ae1b1-106">Fundamentals</span></span>

<span data-ttu-id="ae1b1-107">Wert Konverter werden im Hinblick auf eine `ModelClrType` und eine `ProviderClrType`angegeben.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="ae1b1-108">Der Modelltyp ist der .NET-Typ der-Eigenschaft im-Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="ae1b1-109">Der Anbietertyp ist der .NET-Typ, der vom Datenbankanbieter verstanden wird.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="ae1b1-110">Um z. b. Enumerationen als Zeichen folgen in der Datenbank zu speichern, ist der Modelltyp der Typ der Enumeration, und der Anbietertyp ist `String`.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="ae1b1-111">Diese beiden Typen können identisch sein.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-111">These two types can be the same.</span></span>

<span data-ttu-id="ae1b1-112">Konvertierungen werden mithilfe von zwei `Func` Ausdrucks Baumstrukturen definiert: einer von `ModelClrType` bis `ProviderClrType` und der andere von `ProviderClrType` bis `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="ae1b1-113">Ausdrucks Baumstrukturen werden verwendet, damit Sie für effiziente Konvertierungen in den Datenbankzugriffs Code kompiliert werden können.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="ae1b1-114">Bei komplexen Konvertierungen kann die Ausdrucks Baumstruktur ein einfacher aufrufungs Vorgang für eine Methode sein, die die Konvertierung ausführt.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="ae1b1-115">Konfigurieren eines Wert Konverters</span><span class="sxs-lookup"><span data-stu-id="ae1b1-115">Configuring a value converter</span></span>

<span data-ttu-id="ae1b1-116">Wert Konvertierungen werden in den Eigenschaften des `OnModelCreating` der `DbContext`definiert.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="ae1b1-117">Stellen Sie sich z. b. eine Aufzählung und einen Entitätstyp vor</span><span class="sxs-lookup"><span data-stu-id="ae1b1-117">For example, consider an enum and entity type defined as:</span></span>

``` csharp
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

<span data-ttu-id="ae1b1-118">Anschließend können Konvertierungen in `OnModelCreating` definiert werden, um die Enumerationswerte als Zeichen folgen (z. b. "Esel", "maulzeichen",...) in der Datenbank zu speichern:</span><span class="sxs-lookup"><span data-stu-id="ae1b1-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>

``` csharp
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
> <span data-ttu-id="ae1b1-119">Ein `null` Wert wird nie an einen Wert Konverter übermittelt.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="ae1b1-120">Dies vereinfacht die Implementierung von Konvertierungen und ermöglicht die gemeinsame Nutzung zwischen NULL-Werten und nicht auf NULL festleg baren Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="ae1b1-121">Die ValueConverter-Klasse</span><span class="sxs-lookup"><span data-stu-id="ae1b1-121">The ValueConverter class</span></span>

<span data-ttu-id="ae1b1-122">Wenn Sie `HasConversion` wie oben gezeigt aufrufen, wird eine `ValueConverter` Instanz erstellt und für die-Eigenschaft festgelegt.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="ae1b1-123">Der `ValueConverter` kann stattdessen explizit erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="ae1b1-124">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ae1b1-124">For example:</span></span>

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="ae1b1-125">Dies kann hilfreich sein, wenn mehrere Eigenschaften dieselbe Konvertierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="ae1b1-126">Es gibt derzeit keine Möglichkeit, an einem Ort anzugeben, dass jede Eigenschaft eines bestimmten Typs denselben Wert Konverter verwenden muss.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="ae1b1-127">Diese Funktion wird für eine zukünftige Version in Erwägung gezogen.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="ae1b1-128">Integrierte Konverter</span><span class="sxs-lookup"><span data-stu-id="ae1b1-128">Built-in converters</span></span>

<span data-ttu-id="ae1b1-129">EF Core wird mit einer Reihe vordefinierter `ValueConverter` Klassen ausgeliefert, die sich im `Microsoft.EntityFrameworkCore.Storage.ValueConversion`-Namespace befinden.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="ae1b1-130">Diese lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="ae1b1-130">These are:</span></span>

* <span data-ttu-id="ae1b1-131">`BoolToZeroOneConverter`-bool auf NULL und eins</span><span class="sxs-lookup"><span data-stu-id="ae1b1-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="ae1b1-132">`BoolToStringConverter`-bool in Zeichen folgen wie "Y" und "N".</span><span class="sxs-lookup"><span data-stu-id="ae1b1-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="ae1b1-133">`BoolToTwoValuesConverter`-bool auf zwei beliebige Werte</span><span class="sxs-lookup"><span data-stu-id="ae1b1-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="ae1b1-134">`BytesToStringConverter`-Byte-Array in Base64-codierte Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ae1b1-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="ae1b1-135">`CastingConverter` Konvertierungen, die nur eine Typumwandlung erfordern</span><span class="sxs-lookup"><span data-stu-id="ae1b1-135">`CastingConverter` - Conversions that require only a type cast</span></span>
* <span data-ttu-id="ae1b1-136">Zeichenfolge "`CharToStringConverter`-Char to Single Character"</span><span class="sxs-lookup"><span data-stu-id="ae1b1-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="ae1b1-137">`DateTimeOffsetToBinaryConverter`-DateTimeOffset in binary-codierter 64-Bit-Wert</span><span class="sxs-lookup"><span data-stu-id="ae1b1-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="ae1b1-138">`DateTimeOffsetToBytesConverter`-DateTimeOffset in Bytearray</span><span class="sxs-lookup"><span data-stu-id="ae1b1-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="ae1b1-139">`DateTimeOffsetToStringConverter`-DateTimeOffset in Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ae1b1-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="ae1b1-140">`DateTimeToBinaryConverter`-DateTime-to-64-Bit-Wert einschließlich DateTimeKind</span><span class="sxs-lookup"><span data-stu-id="ae1b1-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="ae1b1-141">`DateTimeToStringConverter`-DateTime zu String</span><span class="sxs-lookup"><span data-stu-id="ae1b1-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="ae1b1-142">`DateTimeToTicksConverter`-DateTime-Wert für Ticks</span><span class="sxs-lookup"><span data-stu-id="ae1b1-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="ae1b1-143">`EnumToNumberConverter`-Enumeration zur zugrunde liegenden Zahl</span><span class="sxs-lookup"><span data-stu-id="ae1b1-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="ae1b1-144">`EnumToStringConverter`-Enum in Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ae1b1-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="ae1b1-145">`GuidToBytesConverter`-GUID zu Bytearray</span><span class="sxs-lookup"><span data-stu-id="ae1b1-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="ae1b1-146">`GuidToStringConverter`-GUID in Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ae1b1-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="ae1b1-147">`NumberToBytesConverter`-beliebiger numerischer Wert zum Bytearray</span><span class="sxs-lookup"><span data-stu-id="ae1b1-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="ae1b1-148">`NumberToStringConverter`-beliebiger numerischer Wert zur Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ae1b1-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="ae1b1-149">`StringToBytesConverter` Zeichenfolge in UTF8-bytes</span><span class="sxs-lookup"><span data-stu-id="ae1b1-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="ae1b1-150">`TimeSpanToStringConverter`-TimeSpan in Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="ae1b1-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="ae1b1-151">`TimeSpanToTicksConverter` Zeitspanne in Ticks</span><span class="sxs-lookup"><span data-stu-id="ae1b1-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="ae1b1-152">Beachten Sie, dass `EnumToStringConverter` in dieser Liste enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="ae1b1-153">Dies bedeutet, dass die Konvertierung nicht explizit angegeben werden muss, wie oben gezeigt.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="ae1b1-154">Verwenden Sie stattdessen einfach den integrierten Konverter:</span><span class="sxs-lookup"><span data-stu-id="ae1b1-154">Instead, just use the built-in converter:</span></span>

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="ae1b1-155">Beachten Sie, dass alle integrierten Konverter zustandslos sind, sodass eine einzelne Instanz auf sichere Weise von mehreren Eigenschaften gemeinsam genutzt werden kann.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="ae1b1-156">Vordefinierte Konvertierungen</span><span class="sxs-lookup"><span data-stu-id="ae1b1-156">Pre-defined conversions</span></span>

<span data-ttu-id="ae1b1-157">Für allgemeine Konvertierungen, bei denen ein integrierter Konverter vorhanden ist, muss der Konverter nicht explizit angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="ae1b1-158">Stattdessen konfigurieren Sie einfach den zu verwendenden Anbietertyp, und EF verwendet automatisch den entsprechenden integrierten Konverter.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="ae1b1-159">Konvertierungen von Enum zu Zeichen folgen werden als Beispiel oben verwendet, EF führt dies jedoch automatisch aus, wenn der Anbietertyp konfiguriert ist:</span><span class="sxs-lookup"><span data-stu-id="ae1b1-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

<span data-ttu-id="ae1b1-160">Dies kann auch durch explizites angeben des Spalten Typs erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="ae1b1-161">Beispielsweise, wenn der Entitätstyp wie folgt definiert ist:</span><span class="sxs-lookup"><span data-stu-id="ae1b1-161">For example, if the entity type is defined like so:</span></span>

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

<span data-ttu-id="ae1b1-162">Anschließend werden die Enumerationswerte als Zeichen folgen in der Datenbank gespeichert, ohne dass eine weitere Konfiguration in `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="ae1b1-163">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="ae1b1-163">Limitations</span></span>

<span data-ttu-id="ae1b1-164">Es gibt einige bekannte aktuelle Einschränkungen des Value-Konvertierungs Systems:</span><span class="sxs-lookup"><span data-stu-id="ae1b1-164">There are a few known current limitations of the value conversion system:</span></span>

* <span data-ttu-id="ae1b1-165">Wie bereits erwähnt, können `null` nicht konvertiert werden.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="ae1b1-166">Zurzeit gibt es keine Möglichkeit, eine Konvertierung einer Eigenschaft in mehrere Spalten zu verteilen (oder umgekehrt).</span><span class="sxs-lookup"><span data-stu-id="ae1b1-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="ae1b1-167">Die Verwendung von Wert Konvertierungen kann sich auf die Fähigkeit von EF Core auswirken, Ausdrücke in SQL zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="ae1b1-168">In solchen Fällen wird eine Warnung protokolliert.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="ae1b1-169">Das Entfernen dieser Einschränkungen wird für eine zukünftige Version in Erwägung gezogen.</span><span class="sxs-lookup"><span data-stu-id="ae1b1-169">Removal of these limitations is being considered for a future release.</span></span>
