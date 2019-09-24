---
title: Arbeiten mit Verweis Typen, die NULL-Werte zulassen-EF Core
author: roji
ms.date: 9/9/2019
ms.assetid: bde4e0ee-fba3-4813-a849-27049323d301
uid: core/miscellaneous/nullable-reference-types
ms.openlocfilehash: 1aa49f1ed518dfe4c13976cef8e868f58d12d5a3
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204402"
---
# <a name="working-with-nullable-reference-types"></a>Arbeiten mit Verweis Typen, die NULL-Werte zulassen

C#in 8 wurde ein neues Feature namens " [Werte zulässt Reference Types](/dotnet/csharp/tutorials/nullable-reference-types)" eingeführt, sodass Verweis Typen mit Anmerkungen versehen werden können. Dies deutet darauf hin, ob es zulässig ist, dass NULL-Werte enthalten sind. Wenn Sie mit dieser Funktion noch nicht vertraut sind, wird empfohlen, dass Sie sich mit der Dokumentation vertraut C# machen, indem Sie die Dokumentation lesen.

Auf dieser Seite finden Sie eine Einführung in EF Core Unterstützung für Verweis Typen, die NULL-Werte zulassen, und die bewährten Methoden für die Arbeit mit

## <a name="required-and-optional-properties"></a>Erforderliche und optionale Eigenschaften

Die Haupt Dokumentation zu den erforderlichen und optionalen Eigenschaften sowie deren Interaktion mit Verweis Typen, die NULL-Werte zulassen, ist die [erforderliche und optionale Eigenschaften](xref:core/modeling/required-optional) Seite. Es wird empfohlen, dass Sie zunächst die Seite lesen.

> [!NOTE]
> Vorsicht beim Aktivieren von Verweis Typen, die NULL-Werte zulassen, für ein vorhandenes Projekt: Verweistyp Eigenschaften, die zuvor als optional konfiguriert wurden, werden nun als erforderlich konfiguriert, es sei denn, Sie sind explizit mit Anmerkungen versehen, die NULL-Werte zulassen. Wenn Sie ein relationales Datenbankschema verwalten, kann dies dazu führen, dass Migrationen generiert werden, die die NULL-Zulässigkeit der Daten Bank Spalte ändern.

## <a name="dbcontext-and-dbset"></a>Dbcontext und dbset

Wenn Verweis Typen, die NULL-Werte zulassen C# , aktiviert sind, gibt der Compiler Warnungen für jede nicht initialisierte Eigenschaft aus, die keine NULL-Werte zulässt, da diese NULL-Werte enthalten. Folglich generiert die gängige Vorgehensweise, eine nicht auf NULL festleg Bare `DbSet` in einem Kontext zu definieren, jetzt eine Warnung. Allerdings werden von EF Core immer alle `DbSet` Eigenschaften von dbcontext-abgeleiteten Typen initialisiert, sodass Sie sicher sind, dass Sie nie NULL sind, auch wenn der Compiler das nicht kennt. Daher wird empfohlen, dass die `DbSet` Eigenschaften nicht auf NULL festgelegt werden können, sodass Sie ohne Null-Überprüfungen darauf zugreifen können, und um die Compilerwarnungen zu stillen, indem Sie Sie mit der Hilfe des NULL-Operator Operators (!) explizit auf NULL festlegen:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/NullableReferenceTypesContext.cs?name=Context&highlight=3-4)]

## <a name="non-nullable-properties-and-initialization"></a>Eigenschaften und Initialisierung, die keine NULL-Werte zulassen

Compilerwarnungen für nicht initialisierte Verweis Typen, die keine NULL-Werte zulassen, sind auch ein Problem für reguläre Eigenschaften ihrer Entitäts Typen. Im obigen Beispiel haben wir diese Warnungen durch die Verwendung von [konstruktorbindung](xref:core/modeling/constructors)vermieden, eine Funktion, die perfekt mit nicht auf NULL festleg baren Eigenschaften funktioniert und sicherstellt, dass Sie immer initialisiert werden. In manchen Szenarios ist die konstruktorbindung jedoch keine Option: Navigations Eigenschaften können z. b. nicht auf diese Weise initialisiert werden.

Erforderliche Navigations Eigenschaften stellen eine zusätzliche Schwierigkeit dar: Obwohl für einen bestimmten Prinzipal immer ein abhängiger vorhanden ist, kann er von einer bestimmten Abfrage geladen werden, je nach den Anforderungen an dieser Stelle im Programm ([Siehe die unterschiedlichen Muster für). Daten werden geladen](xref:core/querying/related-data)). Gleichzeitig ist es nicht wünschenswert, dass diese Eigenschaften auf NULL festgelegt werden, da dadurch der gesamte Zugriff auf NULL erzwungen werden würde, auch wenn Sie erforderlich sind.

Eine Möglichkeit zum Umgang mit diesen Szenarien besteht darin, eine Eigenschaft, die keine NULL-Werte zulässt, mit einem dahinter liegenden [Feld](xref:core/modeling/backing-field)zu haben, das NULL-Werte zulässt

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=12-17)]

Da die Navigations Eigenschaft keine NULL-Werte zulässt, wird eine erforderliche Navigation konfiguriert. solange die Navigation ordnungsgemäß geladen ist, kann auf den abhängigen über die-Eigenschaft zugegriffen werden. Wenn jedoch auf die Eigenschaft zugegriffen wird, ohne dass die zugehörige Entität zuvor ordnungsgemäß geladen wurde, wird eine InvalidOperationException ausgelöst, da der API-Vertrag falsch verwendet wurde.

Als terser Alternative ist es möglich, die-Eigenschaft mit der Hilfe des NULL-Operator (!) einfach mit NULL zu initialisieren:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=19)]

Ein tatsächlicher Null-Wert wird nie beobachtet, außer aufgrund eines Programmierfehlers, z. b. durch den Zugriff auf die Navigations Eigenschaft, ohne dass die zugehörige Entität vorab ordnungsgemäß geladen wird.

> [!NOTE]
> Sammlungs Navigation, die Verweise auf mehrere verknüpfte Entitäten enthalten, sollte immer keine NULL-Werte zulassen. Eine leere Auflistung bedeutet, dass keine verknüpften Entitäten vorhanden sind, aber die Liste selbst darf nie NULL sein.

## <a name="navigating-and-including-nullable-relationships"></a>Navigieren in und einschließen von null-fähigen Beziehungen

Beim Umgang mit optionalen Beziehungen können Compilerwarnungen auftreten, bei denen eine tatsächliche NULL-Verweis Ausnahme nicht möglich wäre. Wenn Sie Ihre LINQ-Abfragen übersetzen und ausführen, stellt EF Core sicher, dass die Navigation zu dieser nicht ausgelöst wird, wenn eine optionale verknüpfte Entität nicht vorhanden ist. Der Compiler kennt diese EF Core Garantie jedoch nicht und führt Warnungen aus, als ob die LINQ-Abfrage im Arbeitsspeicher ausgeführt wurde, mit LINQ to Objects. Daher ist es erforderlich, dass der Compiler den NULL-Operator (!) verwendet, um den Compiler darüber zu informieren, dass ein tatsächlicher Null-Wert nicht möglich ist:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=46)]

Ein ähnliches Problem tritt auf, wenn mehrere Beziehungs Ebenen über optionale Navigationen eingeschlossen werden:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=36-39&highlight=2)]

Wenn Sie dies sehr viel tun und die fraglichen Entitäts Typen vorwiegend (oder exklusiv) in EF Core Abfragen verwendet werden, sollten Sie die Navigations Eigenschaften in Erwägung ziehen, die nicht auf NULL festgelegt werden können, und Sie über die fließende API oder Daten Anmerkungen als optional konfigurieren. Dadurch werden alle Compilerwarnungen entfernt, während die Beziehung optional bleibt. Wenn die Entitäten jedoch außerhalb EF Core durchlaufen werden, werden möglicherweise NULL-Werte beachtet, obwohl die Eigenschaften als nicht auf NULL festleg Bare Werte kommentiert werden.

## <a name="scaffolding"></a>Gerüstbau

[Das C# Feature 8, das NULL-Werte](/dotnet/csharp/tutorials/nullable-reference-types) zulässt, wird in Reverse Engineering derzeit nicht unterstützt: EF Core generiert C# immer Code, der davon ausgeht, dass das Feature deaktiviert ist. Beispielsweise werden Textspalten, die NULL-Werte zulassen, als Eigenschaft mit dem Typ `string` erstellt, `string?`nicht mit der fließenden API oder den Daten Anmerkungen, mit denen konfiguriert wird, ob eine Eigenschaft erforderlich ist. Sie können den Gerüst Code bearbeiten und diese durch C# NULL-Zulässigkeit-Anmerkungen ersetzen. Die Gerüstbau Unterstützung für Verweis Typen, die NULL-Werte zulassen, wird von Issue [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)
