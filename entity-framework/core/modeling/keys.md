---
title: Schlüssel-EF Core
description: Konfigurieren von Schlüsseln für Entitäts Typen bei Verwendung von Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: abd65a5ea079a49fd7a3bbc84a9337f6ee19fab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413990"
---
# <a name="keys"></a>Schlüssel

Ein Schlüssel dient als eindeutiger Bezeichner für jede Entitäts Instanz. Die meisten Entitäten in EF verfügen über einen einzelnen Schlüssel, der dem Konzept eines *Primärschlüssels* in relationalen Datenbanken zugeordnet ist (für Entitäten ohne Schlüssel siehe [Keyless Entities](xref:core/modeling/keyless-entity-types)). Entitäten können über den Primärschlüssel hinaus zusätzliche Schlüssel aufweisen (Weitere Informationen finden Sie in den [Alternativen Schlüsseln](#alternate-keys) ).

Gemäß der Konvention wird eine Eigenschaft mit dem Namen `Id` oder `<type name>Id` als Primärschlüssel einer Entität konfiguriert.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3,11)]

> [!NOTE]
> [Besitzende Entitäts Typen](xref:core/modeling/owned-entities) verwenden verschiedene Regeln, um Schlüssel zu definieren.

Sie können eine einzelne Eigenschaft wie folgt als Primärschlüssel einer Entität konfigurieren:

## <a name="data-annotations"></a>[Datenanmerkungen](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?name=KeySingle&highlight=3)]

## <a name="fluent-api"></a>[Fließende API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?name=KeySingle&highlight=4)]

***

Sie können auch mehrere Eigenschaften als Schlüssel für eine Entität konfigurieren. Dies wird als zusammengesetzter Schlüssel bezeichnet. Zusammengesetzte Schlüssel können nur mithilfe der fließenden API konfiguriert werden. Konventionen richten nie einen zusammengesetzten Schlüssel ein, und Sie können keine Daten Anmerkungen verwenden, um einen zusammengesetzten Schlüssel zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?name=KeyComposite&highlight=4)]

## <a name="primary-key-name"></a>Primärschlüssel Name

Gemäß der Konvention werden Primärschlüssel für relationale Datenbanken mit dem Namen `PK_<type name>`erstellt. Sie können den Namen der PRIMARY KEY-Einschränkung wie folgt konfigurieren:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyName.cs?name=KeyName&highlight=5)]

## <a name="key-types-and-values"></a>Schlüsseltypen und-Werte

Obwohl EF Core die Verwendung von Eigenschaften eines beliebigen primitiven Typs als Primärschlüssel unterstützt, einschließlich `string`, `Guid`, `byte[]` und anderen, unterstützen nicht alle-Datenbanken alle Typen als Schlüssel. In einigen Fällen können die Schlüsselwerte automatisch in einen unterstützten Typ konvertiert werden. andernfalls sollte die Konvertierung [manuell angegeben](xref:core/modeling/value-conversions)werden.

Schlüsseleigenschaften müssen immer einen nicht standardmäßigen Wert aufweisen, wenn eine neue Entität zum Kontext hinzugefügt wird, aber einige Typen werden [von der Datenbank generiert](xref:core/modeling/generated-properties). In diesem Fall versucht EF, einen temporären Wert zu generieren, wenn die Entität zu nach Verfolgungs Zwecken hinzugefügt wird. Nachdem [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) aufgerufen wurde, wird der temporäre Wert durch den von der Datenbank generierten Wert ersetzt.

> [!Important]
> Wenn eine Schlüsseleigenschaft den von der Datenbank generierten Wert aufweist und beim Hinzufügen einer Entität ein nicht standardmäßiger Wert angegeben wird, geht EF davon aus, dass die Entität bereits in der Datenbank vorhanden ist, und versucht dann, Sie zu aktualisieren, anstatt eine neue einzufügen. Um dies zu vermeiden, deaktivieren Sie die Wert Generierung, oder erfahren Sie, [wie Sie explizite Werte für generierte Eigenschaften angeben](../saving/explicit-values-generated-properties.md).

## <a name="alternate-keys"></a>Alternativschlüssel

Ein alternativer Schlüssel dient als alternativer eindeutiger Bezeichner für jede Entitäts Instanz zusätzlich zum Primärschlüssel. Sie kann als Ziel einer Beziehung verwendet werden. Bei Verwendung einer relationalen Datenbank wird diese dem Konzept eines eindeutigen Indexes/einer eindeutigen Einschränkung in den alternativen Schlüssel Spalten und einer oder mehrerer Foreign Key-Einschränkungen zugeordnet, die auf die Spalte (n) verweisen.

> [!TIP]
> Wenn Sie nur die Eindeutigkeit für eine Spalte erzwingen möchten, definieren Sie einen eindeutigen Index anstelle eines alternativen Schlüssels (Weitere Informationen finden Sie unter [Indizes](indexes.md)). In EF sind alternative Schlüssel schreibgeschützt und bieten zusätzliche Semantik über eindeutige Indizes, da Sie als Ziel eines fremd Schlüssels verwendet werden können.

Alternative Schlüssel werden in der Regel bei Bedarf für Sie eingeführt, und Sie müssen Sie nicht manuell konfigurieren. Gemäß der Konvention wird ein alternativer Schlüssel für Sie eingeführt, wenn Sie eine Eigenschaft identifizieren, die nicht der Primärschlüssel als Ziel einer Beziehung ist.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

Sie können auch eine einzelne Eigenschaft als alternativen Schlüssel konfigurieren:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=4)]

Sie können auch mehrere Eigenschaften als alternativen Schlüssel (als zusammengesetzten alternativen Schlüssel bezeichnet) konfigurieren:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=4)]

Schließlich wird der Index und die Einschränkung, die für einen alternativen Schlüssel eingeführt werden, gemäß der Konvention `AK_<type name>_<property name>` benannt (bei zusammengesetzten Alternativen Schlüsseln `<property name>` zu einer durch Trennzeichen getrennten Liste mit Eigenschaftsnamen). Sie können den Namen des Indexes und der Unique-Einschränkung für den alternativen Schlüssel konfigurieren:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyName.cs?name=AlternateKeyName&highlight=5)]
