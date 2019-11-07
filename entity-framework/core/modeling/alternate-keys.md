---
title: Alternative Schlüssel-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655779"
---
# <a name="alternate-keys"></a>Alternativschlüssel

Ein alternativer Schlüssel dient als alternativer eindeutiger Bezeichner für jede Entitäts Instanz zusätzlich zum Primärschlüssel. Alternative Schlüssel können als Ziel einer Beziehung verwendet werden. Bei Verwendung einer relationalen Datenbank wird diese dem Konzept eines eindeutigen Indexes/einer eindeutigen Einschränkung in den alternativen Schlüssel Spalten und einer oder mehrerer Foreign Key-Einschränkungen zugeordnet, die auf die Spalte (n) verweisen.

> [!TIP]  
> Wenn Sie nur die Eindeutigkeit einer Spalte erzwingen möchten, benötigen Sie einen eindeutigen Index anstelle eines alternativen Schlüssels. Weitere Informationen finden Sie unter [Indizes](indexes.md). In EF bieten alternative Schlüssel mehr Funktionalität als eindeutige Indizes, da Sie als Ziel eines fremd Schlüssels verwendet werden können.

Alternative Schlüssel werden in der Regel bei Bedarf für Sie eingeführt, und Sie müssen Sie nicht manuell konfigurieren. Weitere Informationen finden Sie unter [Konventionen](#conventions) .

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird ein alternativer Schlüssel für Sie eingeführt, wenn Sie eine Eigenschaft, die nicht der Primärschlüssel ist, als Ziel einer Beziehung identifizieren.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a>Datenanmerkungen

Alternative Schlüssel können nicht mithilfe von Daten Anmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um eine einzelne Eigenschaft als alternativen Schlüssel zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

Sie können auch die fließende API verwenden, um mehrere Eigenschaften als alternativen Schlüssel (als zusammengesetzten alternativen Schlüssel bezeichnet) zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
