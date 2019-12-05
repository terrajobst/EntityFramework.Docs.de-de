---
title: Schlüssel (primär)-EF Core
description: Konfigurieren von Schlüsseln für Entitäts Typen bei Verwendung von Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824621"
---
# <a name="keys-primary"></a>Schlüssel (primäre)

Ein Schlüssel dient als primärer eindeutiger Bezeichner für jede Entitäts Instanz. Wenn eine relationale Datenbank verwendet wird, wird dies dem Konzept eines *Primärschlüssels*zugeordnet. Sie können auch einen eindeutigen Bezeichner konfigurieren, bei dem es sich nicht um den Primärschlüssel handelt (Weitere Informationen finden Sie in den [Alternativen Schlüsseln](alternate-keys.md) ).

Eine der folgenden Methoden kann verwendet werden, um einen Primärschlüssel einzurichten oder zu erstellen.

## <a name="conventions"></a>Konventionen

Standardmäßig wird eine Eigenschaft mit dem Namen `Id` oder `<type name>Id` als Schlüssel für eine Entität konfiguriert.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> [Besitzende Entitäts Typen](xref:core/modeling/owned-entities) verwenden verschiedene Regeln, um Schlüssel zu definieren.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Daten Anmerkungen verwenden, um eine einzelne Eigenschaft als Schlüssel für eine Entität zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um eine einzelne Eigenschaft als Schlüssel für eine Entität zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Sie können auch die fließende API verwenden, um mehrere Eigenschaften als Schlüssel einer Entität (als zusammengesetzten Schlüssel bezeichnet) zu konfigurieren. Zusammengesetzte Schlüssel können nur mithilfe der fließenden API konfiguriert werden, und Sie können niemals einen zusammengesetzten Schlüssel einrichten, und Sie können keine Daten Anmerkungen verwenden, um einen zusammengesetzten Schlüssel zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a>Schlüsseltypen und-Werte

EF Core unterstützt die Verwendung von Eigenschaften eines beliebigen primitiven Typs als Primärschlüssel, einschließlich `string`, `Guid`, `byte[]` und anderen. Diese werden jedoch nicht von allen Datenbanken unterstützt. In einigen Fällen können die Schlüsselwerte automatisch in einen unterstützten Typ konvertiert werden. andernfalls sollte die Konvertierung [manuell angegeben](xref:core/modeling/value-conversions)werden.

Schlüsseleigenschaften müssen immer einen nicht standardmäßigen Wert aufweisen, wenn eine neue Entität zum Kontext hinzugefügt wird, aber einige Typen werden [von der Datenbank generiert](xref:core/modeling/generated-properties). In diesem Fall versucht EF, einen temporären Wert zu generieren, wenn die Entität zu nach Verfolgungs Zwecken hinzugefügt wird. Nachdem [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) aufgerufen wurde, wird der temporäre Wert durch den von der Datenbank generierten Wert ersetzt.

> [!Important]
> Wenn eine Schlüsseleigenschaft einen von der Datenbank generierten Wert aufweist und beim Hinzufügen einer Entität ein nicht standardmäßiger Wert angegeben wird, geht EF davon aus, dass die Entität bereits in der Datenbank vorhanden ist, und versucht dann, Sie zu aktualisieren, anstatt eine neue einzufügen. Um dies zu vermeiden, deaktivieren Sie die Wert Generierung, oder erfahren Sie, [wie Sie explizite Werte für generierte Eigenschaften angeben](../saving/explicit-values-generated-properties.md).