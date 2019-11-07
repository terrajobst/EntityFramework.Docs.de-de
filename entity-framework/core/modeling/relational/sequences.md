---
title: Sequenzen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656116"
---
# <a name="sequences"></a>Sequenzen

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Eine Sequenz generiert einen sequenziellen numerischen Wert in der Datenbank. Sequenzen sind einer bestimmten Tabelle nicht zugeordnet.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden Sequenzen nicht in das Modell eingeführt.

## <a name="data-annotations"></a>Datenanmerkungen

Es ist nicht möglich, eine Sequenz mithilfe von Daten Anmerkungen zu konfigurieren.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um eine Sequenz im Modell zu erstellen.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

Sie können auch zusätzlichen Aspekt der Sequenz konfigurieren, z. b. das Schema, den Startwert und das Inkrement.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

Nachdem eine Sequenz eingeführt wurde, können Sie Sie verwenden, um Werte für Eigenschaften in Ihrem Modell zu generieren. Beispielsweise können Sie die [Standardwerte](default-values.md) verwenden, um den nächsten Wert aus der Sequenz einzufügen.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
