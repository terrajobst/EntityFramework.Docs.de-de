---
title: Einschließen & Ausschließen von Typen EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655738"
---
# <a name="including--excluding-types"></a>Einschließen und Ausschließen von Typen

Das Einschließen eines Typs in das Modell bedeutet, dass EF über Metadaten zu diesem Typ verfügt und versucht, Instanzen aus der Datenbank zu lesen und zu schreiben.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention sind Typen, die in `DbSet` Eigenschaften in ihrem Kontext verfügbar gemacht werden, in Ihrem Modell enthalten. Außerdem sind Typen, die in der `OnModelCreating`-Methode erwähnt werden, ebenfalls enthalten. Schließlich sind alle Typen, die gefunden werden, indem die Navigations Eigenschaften von ermittelten Typen rekursiv untersucht werden, auch im Modell enthalten.

**Im folgenden Code werden z. b. alle drei Typen ermittelt:**

* `Blog`, da es in einer `DbSet`-Eigenschaft im Kontext verfügbar gemacht wird.

* `Post`, da Sie über die `Blog.Posts` Navigations Eigenschaft erkannt wird.

* `AuditEntry`, da Sie in `OnModelCreating`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a>Datenanmerkungen

Mithilfe von Daten Anmerkungen können Sie einen Typ aus dem Modell ausschließen.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Fluent-API

Mit der fließend-API können Sie einen Typ aus dem Modell ausschließen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
