---
title: Wert Konvertierungen-EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414554"
---
# <a name="value-conversions"></a>Wertkonvertierungen

> [!NOTE]  
> Dieses Feature ist neu in EF Core 2.1.

Mithilfe von Wert Konvertern können Eigenschaftswerte beim Lesen aus der Datenbank oder beim Schreiben in die Datenbank konvertiert werden. Diese Konvertierung kann von einem Wert in einen anderen desselben Typs (z. b. das Verschlüsseln von Zeichen folgen) oder von einem Wert eines Typs zu einem Wert eines anderen Typs erfolgen (z. b. beim Konvertieren von Enumerationswerten in und aus Zeichen folgen in der Datenbank).

## <a name="fundamentals"></a>Grundlagen

Wert Konverter werden im Hinblick auf eine `ModelClrType` und eine `ProviderClrType`angegeben. Der Modelltyp ist der .NET-Typ der-Eigenschaft im-Entitätstyp. Der Anbietertyp ist der .NET-Typ, der vom Datenbankanbieter verstanden wird. Um z. b. Enumerationen als Zeichen folgen in der Datenbank zu speichern, ist der Modelltyp der Typ der Enumeration, und der Anbietertyp ist `String`. Diese beiden Typen können identisch sein.

Konvertierungen werden mithilfe von zwei `Func` Ausdrucks Baumstrukturen definiert: einer von `ModelClrType` bis `ProviderClrType` und der andere von `ProviderClrType` bis `ModelClrType`. Ausdrucks Baumstrukturen werden verwendet, damit Sie für effiziente Konvertierungen in den Datenbankzugriffs Code kompiliert werden können. Bei komplexen Konvertierungen kann die Ausdrucks Baumstruktur ein einfacher aufrufungs Vorgang für eine Methode sein, die die Konvertierung ausführt.

## <a name="configuring-a-value-converter"></a>Konfigurieren eines Wert Konverters

Wert Konvertierungen werden in den Eigenschaften des `OnModelCreating` der `DbContext`definiert. Stellen Sie sich z. b. eine Aufzählung und einen Entitätstyp vor

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

Anschließend können Konvertierungen in `OnModelCreating` definiert werden, um die Enumerationswerte als Zeichen folgen (z. b. "Esel", "maulzeichen",...) in der Datenbank zu speichern:

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
> Ein `null` Wert wird nie an einen Wert Konverter übermittelt. Dies vereinfacht die Implementierung von Konvertierungen und ermöglicht die gemeinsame Nutzung zwischen NULL-Werten und nicht auf NULL festleg baren Eigenschaften.

## <a name="the-valueconverter-class"></a>Die ValueConverter-Klasse

Wenn Sie `HasConversion` wie oben gezeigt aufrufen, wird eine `ValueConverter` Instanz erstellt und für die-Eigenschaft festgelegt. Der `ValueConverter` kann stattdessen explizit erstellt werden. Beispiel:

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Dies kann hilfreich sein, wenn mehrere Eigenschaften dieselbe Konvertierung verwenden.

> [!NOTE]  
> Es gibt derzeit keine Möglichkeit, an einem Ort anzugeben, dass jede Eigenschaft eines bestimmten Typs denselben Wert Konverter verwenden muss. Diese Funktion wird für eine zukünftige Version in Erwägung gezogen.

## <a name="built-in-converters"></a>Integrierte Konverter

EF Core wird mit einer Reihe vordefinierter `ValueConverter` Klassen ausgeliefert, die sich im `Microsoft.EntityFrameworkCore.Storage.ValueConversion`-Namespace befinden. Dies sind:

* `BoolToZeroOneConverter`-bool auf NULL und eins
* `BoolToStringConverter`-bool in Zeichen folgen wie "Y" und "N".
* `BoolToTwoValuesConverter`-bool auf zwei beliebige Werte
* `BytesToStringConverter`-Byte-Array in Base64-codierte Zeichenfolge
* `CastingConverter` Konvertierungen, die nur eine Typumwandlung erfordern
* Zeichenfolge "`CharToStringConverter`-Char to Single Character"
* `DateTimeOffsetToBinaryConverter`-DateTimeOffset in binary-codierter 64-Bit-Wert
* `DateTimeOffsetToBytesConverter`-DateTimeOffset in Bytearray
* `DateTimeOffsetToStringConverter`-DateTimeOffset in Zeichenfolge
* `DateTimeToBinaryConverter`-DateTime-to-64-Bit-Wert einschließlich DateTimeKind
* `DateTimeToStringConverter`-DateTime zu String
* `DateTimeToTicksConverter`-DateTime-Wert für Ticks
* `EnumToNumberConverter`-Enumeration zur zugrunde liegenden Zahl
* `EnumToStringConverter`-Enum in Zeichenfolge
* `GuidToBytesConverter`-GUID zu Bytearray
* `GuidToStringConverter`-GUID in Zeichenfolge
* `NumberToBytesConverter`-beliebiger numerischer Wert zum Bytearray
* `NumberToStringConverter`-beliebiger numerischer Wert zur Zeichenfolge
* `StringToBytesConverter` Zeichenfolge in UTF8-bytes
* `TimeSpanToStringConverter`-TimeSpan in Zeichenfolge
* `TimeSpanToTicksConverter` Zeitspanne in Ticks

Beachten Sie, dass `EnumToStringConverter` in dieser Liste enthalten ist. Dies bedeutet, dass die Konvertierung nicht explizit angegeben werden muss, wie oben gezeigt. Verwenden Sie stattdessen einfach den integrierten Konverter:

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

Beachten Sie, dass alle integrierten Konverter zustandslos sind, sodass eine einzelne Instanz auf sichere Weise von mehreren Eigenschaften gemeinsam genutzt werden kann.

## <a name="pre-defined-conversions"></a>Vordefinierte Konvertierungen

Für allgemeine Konvertierungen, bei denen ein integrierter Konverter vorhanden ist, muss der Konverter nicht explizit angegeben werden. Stattdessen konfigurieren Sie einfach den zu verwendenden Anbietertyp, und EF verwendet automatisch den entsprechenden integrierten Konverter. Konvertierungen von Enum zu Zeichen folgen werden als Beispiel oben verwendet, EF führt dies jedoch automatisch aus, wenn der Anbietertyp konfiguriert ist:

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

Dies kann auch durch explizites angeben des Spalten Typs erreicht werden. Beispielsweise, wenn der Entitätstyp wie folgt definiert ist:

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

Anschließend werden die Enumerationswerte als Zeichen folgen in der Datenbank gespeichert, ohne dass eine weitere Konfiguration in `OnModelCreating`.

## <a name="limitations"></a>Einschränkungen

Es gibt einige bekannte aktuelle Einschränkungen des Value-Konvertierungs Systems:

* Wie bereits erwähnt, können `null` nicht konvertiert werden.
* Zurzeit gibt es keine Möglichkeit, eine Konvertierung einer Eigenschaft in mehrere Spalten zu verteilen (oder umgekehrt).
* Die Verwendung von Wert Konvertierungen kann sich auf die Fähigkeit von EF Core auswirken, Ausdrücke in SQL zu übersetzen. In solchen Fällen wird eine Warnung protokolliert.
Das Entfernen dieser Einschränkungen wird für eine zukünftige Version in Erwägung gezogen.
