---
title: Wertkonvertierungen – EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: d6b51a0a70ee527844b6fe995f39bec534dbaba8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996287"
---
# <a name="value-conversions"></a>Wertkonvertierungen

> [!NOTE]  
> Dieses Feature ist neu in EF Core 2.1.

Wertkonverter ermöglichen die konvertiert werden, wenn die Variable gelesen oder geschrieben werden, in der Datenbank. Diese Konvertierung kann von einem Wert in eine andere vom selben Typ (z. B. Verschlüsselung Zeichenfolgen) oder aus einem Wert eines Typs in einen Wert eines anderen Typs (z. B. Konvertieren von Enum-Werte in und aus Zeichenfolgen in der Datenbank.)

## <a name="fundamentals"></a>Grundlagen

Wertkonverter sind hinsichtlich der angegebenen ein `ModelClrType` und `ProviderClrType`. Der Typ des Modells ist der Typ .NET die Eigenschaft im Entitätstyp. Der Anbietertyp ist der .NET-Typ, der vom Datenbankanbieter verstanden. Z. B. um Enumerationen als Zeichenfolgen in der Datenbank zu speichern, der Typ des Modells ist der Typ der Enumeration; Anbietertyp ist `String`. Diese beiden Typen können identisch sein.

Konvertierungen werden definiert, unter Verwendung zweier `Func` Ausdrucksbaumstrukturen: von `ModelClrType` zu `ProviderClrType` und das andere von `ProviderClrType` zu `ModelClrType`. Ausdrucksbaumstrukturen werden verwendet, sodass sie in der Datenbank-Zugriffscode für effiziente Konvertierungen kompiliert werden können. Bei komplexen Konvertierungen wird möglicherweise die Ausdrucksbaumstruktur einen einfachen Aufruf an eine Methode, die die Konvertierung durchführt.

## <a name="configuring-a-value-converter"></a>Konfigurieren einen Wertkonverter

Konvertierungen werden in Eigenschaften definiert die `OnModelCreating` von Ihrem `DbContext`. Betrachten Sie beispielsweise einen Typ für Enumerationen und Entität, die als definiert:
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
Konvertierungen in definiert werden, können `OnModelCreating` Enum-Werte als Zeichenfolgen (z. B. "Donkey", "Mule",...) in der Datenbank zu speichern:
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
> Ein `null` Wert wird nie auf einen Wertkonverter übergeben werden. Dies erleichtert die Implementierung der Konvertierungen und ermöglicht es ihnen, die von NULL-Werte zulässt und nicht auf NULL festlegbare Eigenschaften gemeinsam genutzt werden.

## <a name="the-valueconverter-class"></a>Die ValueConverter-Klasse

Aufrufen von `HasConversion` wie oben gezeigt erstellt eine `ValueConverter` -Instanz und für die Eigenschaft festgelegt wird. Die `ValueConverter` stattdessen explizit erstellt werden können. Zum Beispiel:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Dies kann nützlich sein, wenn mehrere Eigenschaften auf die gleiche Konvertierung verwenden.

> [!NOTE]  
> Derzeit besteht keine Möglichkeit, um an einem Ort anzugeben, dass jede Eigenschaft eines bestimmten Typs den gleichen Wertkonverter verwenden muss. Dieses Feature wird für ein zukünftiges Release berücksichtigt.

## <a name="built-in-converters"></a>Integrierte Konverter

EF Core umfasst eine Reihe von vordefinierten `ValueConverter` Klassen finden Sie in der `Microsoft.EntityFrameworkCore.Storage.ValueConversion` Namespace. Diese lauten wie folgt:
* `BoolToZeroOneConverter` -"Bool" an, und 0 (null)
* `BoolToStringConverter` -"Bool", um Zeichenfolgen wie "Y" und "N"
* `BoolToTwoValuesConverter` -"Bool", um zwei Werten
* `BytesToStringConverter` -Byte-Array in Base64-codierte Zeichenfolge
* `CastingConverter` -Konvertierungen, die nur eine C#-Umwandlung erfordern.
* `CharToStringConverter` -Von char, um die einzelnen Zeichen bestehende Zeichenfolge
* `DateTimeOffsetToBinaryConverter` -DateTimeOffset Binär codierte 64-Bit-Wert
* `DateTimeOffsetToBytesConverter` -DateTimeOffset Byte-array
* `DateTimeOffsetToStringConverter` -DateTimeOffset Zeichenfolge
* `DateTimeToBinaryConverter` – DateTime, DateTimeKind einschließlich 64-Bit-Wert
* `DateTimeToStringConverter` -DateTime Zeichenfolge
* `DateTimeToTicksConverter` -DateTime in ticks
* `EnumToNumberConverter` -Anzahl der zugrunde liegenden Enumeration
* `EnumToStringConverter` -Zeichenfolge Enumeration
* `GuidToBytesConverter` -Guid in Byte-array
* `GuidToStringConverter` -Guid Zeichenfolge
* `NumberToBytesConverter` – Alle numerischen Wert in Byte-array
* `NumberToStringConverter` – Alle numerischen Wert in eine Zeichenfolge
* `StringToBytesConverter` -Von string in UTF8-bytes
* `TimeSpanToStringConverter` -TimeSpan Zeichenfolge
* `TimeSpanToTicksConverter` -TimeSpan Ticks

Beachten Sie, dass `EnumToStringConverter` in dieser Liste enthalten ist. Dies bedeutet, dass keine Notwendigkeit besteht, wie oben gezeigt, explizit angeben, die Konvertierung. Verwenden Sie stattdessen einfach die integrierten Konverter:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Beachten Sie, dass alle integrierten Konverter zustandslos sind und daher eine einzelne Instanz sicher für mehrere Eigenschaften gemeinsam an.

## <a name="pre-defined-conversions"></a>Vordefinierte Konvertierungen

Für allgemeine Konvertierungen, die für die integrierten Konverter vorhanden ist ist es nicht notwendig, den Konverter explizit anzugeben. Stattdessen einfach konfigurieren, welcher Typ verwendet werden soll, und EF verwendet automatisch den entsprechenden integrierten Konverter. Enumeration, zeichenfolgenkonvertierungen werden als Beispiel oben verwendet, aber EF übernimmt dies automatisch, wenn der Anbietertyp konfiguriert ist:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
Dasselbe kann durch explizites Angeben des Spaltentyps erreicht werden. Wenn der Entitätstyp definiert ist z.B. so:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Und dann die Enum-Werte als Zeichenfolgen in der Datenbank ohne weitere Konfiguration in gespeichert werden `OnModelCreating`.

## <a name="limitations"></a>Einschränkungen

Es gibt einige bekannte aktuelle Einschränkungen des Systems Konvertierung Wert:
* Wie bereits erwähnt, `null` kann nicht konvertiert werden.
* Derzeit besteht keine Möglichkeit, eine Konvertierung von einer Eigenschaft in mehrere Spalten oder umgekehrt zu verteilen.
* Verwendung von Konvertierungen kann es sich um die Fähigkeit von EF Core, die in SQL übersetzen auswirken. Für solche Fälle wird eine Warnung protokolliert werden.
Zum Entfernen dieser Einschränkungen ist für eine spätere Version in Betracht gezogen.
