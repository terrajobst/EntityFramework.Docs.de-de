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
# <a name="value-conversions"></a>Wertkonvertierungen

> [!NOTE]  
> Dieses Feature ist neu in EF Core 2.1.

Wertkonverter können Eigenschaftswerte beim Lesen oder Schreiben in die Datenbank konvertiert werden. Diese Konvertierung kann von einem Wert in eine andere des gleichen Typs (z. B. zum Verschlüsseln verwendete Zeichenfolgen) oder aus einem Wert eines Typs in einen Wert eines anderen Typs (z. B. Konvertieren von Enum-Werte in und aus Zeichenfolgen in der Datenbank.)

## <a name="fundamentals"></a>Grundlagen

Wertkonverter werden angegeben, der eine `ModelClrType` und ein `ProviderClrType`. Das Modell ist der .NET Typ der Eigenschaft in der Entität vom Typ. Der Anbieter ist der .NET-Typ, von dem Datenbankanbieter verstanden. Beispielsweise um Enumerationen als Zeichenfolgen in der Datenbank zu speichern, der Modelltyp ist der Typ der Enumeration und der Anbietertyp ist `String`. Diese beiden Typen können identisch sein.

Konvertierungen werden definiert über zwei `Func` Ausdrucksbaumstrukturen: aus `ModelClrType` auf `ProviderClrType` und das andere von `ProviderClrType` auf `ModelClrType`. Ausdrucksbaumstrukturen werden verwendet, sodass sie in den Datenbank-Access-Code für die effiziente Konvertierung kompiliert werden können. Bei komplexen Konvertierungen wird möglicherweise die Ausdrucksbaumstruktur einen einfachen Aufruf einer Methode, die die Konvertierung ausführt.

## <a name="configuring-a-value-converter"></a>Konfigurieren einen Wertkonverter

Für Eigenschaften in der OnModelCreating von Ihrem DbContext sind wertkonvertierungen definiert. Betrachten Sie beispielsweise einen Enum und Entity-Typ, definiert als:
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
Klicken Sie dann können in OnModelCreating (z. B. die Enumerationswerte als Zeichenfolgen speichern Konvertierungen definiert werden "Donkey", "Maultieren",...) in der Datenbank:
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
> Ein `null` Wert wird nie ein Wertkonverter übergeben werden. Dies vereinfacht die Implementierung der Konvertierungen und ermöglicht es ihnen, unter der NULL-Werte zulässt und nicht auf NULL festlegbare Eigenschaften freigegeben werden.

## <a name="the-valueconverter-class"></a>Die ValueConverter-Klasse

Aufrufen von `HasConversion` wie oben gezeigt erstellt eine `ValueConverter` -Instanz, und legen Sie es auf die Eigenschaft. Die `ValueConverter` stattdessen explizit erstellt werden können. Zum Beispiel:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Dies kann nützlich sein, wenn mehrere Eigenschaften derselben Konvertierung verwenden.

> [!NOTE]  
> Zurzeit besteht keine Möglichkeit, um an einem Ort anzugeben, dass jede Eigenschaft eines bestimmten Typs auf den gleichen Wertkonverter verwenden muss. Diese Funktion wird für eine künftige Version berücksichtigt.

## <a name="built-in-converters"></a>Integrierte Konverter

EF Core umfasst eine Reihe von vordefinierten `ValueConverter` Klassen aus, die `Microsoft.EntityFrameworkCore.Storage.ValueConversion` Namespace. Diese lauten wie folgt:
* `BoolToZeroOneConverter` -Bool 0 (null) und 1
* `BoolToStringConverter` -Bool Zeichenfolgen, z. B. "Y" und "N"
* `BoolToTwoValuesConverter` -Bool zwei Werten
* `BytesToStringConverter` -Byte Array in Base64-codierte Zeichenfolge
* `CastingConverter` -Konvertierungen, die nur eine C#-Umwandlung erfordern.
* `CharToStringConverter` -Von char, um die einzelnen Zeichenfolge
* `DateTimeOffsetToBinaryConverter` -"DateTimeOffset", um binärcodierten 64-Bit-Wert
* `DateTimeOffsetToBytesConverter` -DateTimeOffset Byte-array
* `DateTimeOffsetToStringConverter` -DateTimeOffset Zeichenfolge
* `DateTimeToBinaryConverter` -"DateTime", auf 64-Bit-Wert, einschließlich DateTimeKind
* `DateTimeToStringConverter` -DateTime Zeichenfolge
* `DateTimeToTicksConverter` -"DateTime", um die Teilstriche
* `EnumToNumberConverter` -Anzahl der zugrunde liegenden Enumeration
* `EnumToStringConverter` -Enum Zeichenfolge
* `GuidToBytesConverter` -Guid Byte-array
* `GuidToStringConverter` -Guid Zeichenfolge
* `NumberToBytesConverter` – Beliebige numerischen Wert in Bytearray
* `NumberToStringConverter` – Beliebige numerischen Wert in eine Zeichenfolge
* `StringToBytesConverter` -Zeichenfolge auf UTF8-bytes
* `TimeSpanToStringConverter` -TimeSpan Zeichenfolge
* `TimeSpanToTicksConverter` -TimeSpan Ticks

Beachten Sie, dass `EnumToStringConverter` in dieser Liste enthalten ist. Dies bedeutet, dass keine Notwendigkeit besteht, wie oben gezeigt, explizit angeben, die Konvertierung. Verwenden Sie stattdessen nur das integrierte Konverter:
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
Beachten Sie, dass die integrierten Konverter zustandslos sind und daher eine einzelne Instanz problemlos von gemeinsam werden mehrere Eigenschaften genutzt kann.

## <a name="pre-defined-conversions"></a>Vordefinierte Konvertierungen

Für allgemeine Konvertierungen, die für die integrierten Konverter vorhanden ist ist es nicht notwendig, den Konverter explizit anzugeben. Stattdessen nur konfigurieren, welche Art von Anbieter verwendet werden soll und EF verwendet automatisch den entsprechenden Build Konverter. Enumeration zeichenfolgenkonvertierungen dienen als Beispiel oben, aber EF tatsächlich erledigen dies automatisch, wenn der Anbietertyp konfiguriert ist:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
Dasselbe kann durch die explizite Angabe den Spaltentyp erreicht werden. Wenn der Entitätstyp definiert ist z. B. wie so:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
Anschließend werden die Enumerationswerte als Zeichenfolgen in der Datenbank ohne weitere Konfiguration in der OnModelCreating gespeichert werden.

## <a name="limitations"></a>Einschränkungen

Es gibt einige bekannte aktuelle Einschränkungen des Systems Wert konvertiert:
* Wie bereits erwähnt `null` kann nicht konvertiert werden.
* Zurzeit besteht keine Möglichkeit, um eine Konvertierung einer Eigenschaft in mehrere Spalten oder umgekehrt zu verteilen.
* Verwendung von wertkonvertierungen kann die Fähigkeit des EF Core, die in SQL übersetzen auswirken. In solchen Fällen wird eine Warnung protokolliert.
Entfernen von diesen Einschränkungen wird für eine künftige Version berücksichtigt wird.
