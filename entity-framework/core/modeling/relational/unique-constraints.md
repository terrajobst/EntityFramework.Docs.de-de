---
title: Alternative Schlüssel (Unique-Einschränkungen)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197617"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="fc754-102">Alternativschlüssel (Unique-Einschränkungen)</span><span class="sxs-lookup"><span data-stu-id="fc754-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="fc754-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="fc754-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="fc754-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="fc754-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="fc754-105">Eine Unique-Einschränkung wird für jeden alternativen Schlüssel im Modell eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fc754-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="fc754-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="fc754-106">Conventions</span></span>

<span data-ttu-id="fc754-107">Gemäß der Konvention werden der Index und die Einschränkung, die für einen alternativen Schlüssel eingeführt werden `AK_<type name>_<property name>`, benannt.</span><span class="sxs-lookup"><span data-stu-id="fc754-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="fc754-108">Bei zusammengesetzten alternativen `<property name>` Schlüsseln wird eine durch Trennzeichen getrennte Liste mit Eigenschaftsnamen.</span><span class="sxs-lookup"><span data-stu-id="fc754-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="fc754-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="fc754-109">Data Annotations</span></span>

<span data-ttu-id="fc754-110">Unique-Einschränkungen können nicht mithilfe von Daten Anmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="fc754-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="fc754-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="fc754-111">Fluent API</span></span>

<span data-ttu-id="fc754-112">Sie können die fließende API verwenden, um den Index-und Einschränkungs Namen für einen alternativen Schlüssel zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fc754-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
