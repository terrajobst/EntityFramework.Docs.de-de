---
title: Schlüssel (primär)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655948"
---
# <a name="keys-primary"></a>Schlüssel (primäre)

Ein Schlüssel dient als primärer eindeutiger Bezeichner für jede Entitäts Instanz. Wenn eine relationale Datenbank verwendet wird, wird dies dem Konzept eines *Primärschlüssels*zugeordnet. Sie können auch einen eindeutigen Bezeichner konfigurieren, bei dem es sich nicht um den Primärschlüssel handelt (Weitere Informationen finden Sie in den [Alternativen Schlüsseln](alternate-keys.md) ).

Eine der folgenden Methoden kann verwendet werden, um einen Primärschlüssel einzurichten oder zu erstellen.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird eine Eigenschaft mit dem Namen `Id` oder `<type name>Id` als Schlüssel für eine Entität konfiguriert.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Daten Anmerkungen verwenden, um eine einzelne Eigenschaft als Schlüssel für eine Entität zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um eine einzelne Eigenschaft als Schlüssel für eine Entität zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Sie können auch die fließende API verwenden, um mehrere Eigenschaften als Schlüssel einer Entität (als zusammengesetzten Schlüssel bezeichnet) zu konfigurieren. Zusammengesetzte Schlüssel können nur mithilfe der fließenden API konfiguriert werden, und Sie können niemals einen zusammengesetzten Schlüssel einrichten, und Sie können keine Daten Anmerkungen verwenden, um einen zusammengesetzten Schlüssel zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
