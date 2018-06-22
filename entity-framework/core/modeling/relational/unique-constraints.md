---
title: Alternativschlüssel (Unique-Einschränkungen) – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052790"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="dbc4c-102">Alternativschlüssel (Unique-Einschränkungen)</span><span class="sxs-lookup"><span data-stu-id="dbc4c-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="dbc4c-103">Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="dbc4c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="dbc4c-104">Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).</span><span class="sxs-lookup"><span data-stu-id="dbc4c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="dbc4c-105">Eine unique-Einschränkung wird für jede Alternativschlüssel im Modell eingeführt.</span><span class="sxs-lookup"><span data-stu-id="dbc4c-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="dbc4c-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="dbc4c-106">Conventions</span></span>

<span data-ttu-id="dbc4c-107">Gemäß der Konvention werden die Index- und einschränkungsobjekten, die für einen alternativen Schlüssel eingeführt werden Namen `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="dbc4c-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="dbc4c-108">Für zusammengesetzte alternativen Schlüssel `<property name>` wird eine Unterstrich getrennte Liste von Eigenschaftsnamen.</span><span class="sxs-lookup"><span data-stu-id="dbc4c-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="dbc4c-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="dbc4c-109">Data Annotations</span></span>

<span data-ttu-id="dbc4c-110">Unique-Einschränkungen können nicht mithilfe von Datenanmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="dbc4c-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="dbc4c-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="dbc4c-111">Fluent API</span></span>

<span data-ttu-id="dbc4c-112">Sie können die Fluent-API verwenden, so konfigurieren Sie den Index und Namen für einen alternativen Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="dbc4c-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
