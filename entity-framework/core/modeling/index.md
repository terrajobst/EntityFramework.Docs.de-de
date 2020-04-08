---
title: Erstellen und Konfigurieren eines Modells – EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412775"
---
# <a name="creating-and-configuring-a-model"></a>Erstellen und Konfigurieren eines Modells

Entity Framework verwendet eine Reihe von Konventionen, um ein Modell basierend auf der Anordnung Ihrer Entitätsklassen zu erstellen. Sie können zusätzliche Konfigurationen angeben, um die gemäß den Konventionen gewonnenen Ergebnisse zu ergänzen und/oder zu überschreiben.

Dieser Artikel behandelt die Konfiguration, die auf ein Modell für einen beliebigen Datenspeicher und auf jede relationale Datenbank angewendet werden kann. Anbieter können auch eine Konfiguration aktivieren, die speziell für einen bestimmten Datenspeicher gilt. Dokumentation zu anbieterspezifischen Konfigurationen finden Sie im Abschnitt  [Datenbankanbieter](../providers/index.md) .

> [!TIP]  
> Das in diesem Artikel verwendete  [Beispiel](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples)  finden Sie auf GitHub.

## <a name="use-fluent-api-to-configure-a-model"></a>Verwenden der Fluent-API zum Konfigurieren eines Modells

Sie können die  `OnModelCreating` -Methode in Ihrem abgeleiteten Kontext überschreiben und mit der  `ModelBuilder API` Ihr Modell konfigurieren. Dies ist die wirksamste Konfigurationsmethode und erlaubt die Angabe der Konfiguration, ohne die Entitätsklassen zu verändern. Die Fluent-API-Konfiguration hat die höchste Priorität und überschreibt Konventionen und Datenanmerkungen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a>Verwenden von Datenanmerkungen zum Konfigurieren eines Modells

Sie können auch Attribute (sogenannte „Datenanmerkungen“) auf Ihre Klassen und Eigenschaften anwenden. Datenanmerkungen setzen Konventionen außer Kraft, werden aber ihrerseits von der Fluent-API-Konfiguration außer Kraft gesetzt.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
