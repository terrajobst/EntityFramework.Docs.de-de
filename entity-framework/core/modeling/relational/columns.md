---
title: Spaltenzuordnung – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929861"
---
# <a name="column-mapping"></a><span data-ttu-id="926d8-102">Spaltenzuordnung</span><span class="sxs-lookup"><span data-stu-id="926d8-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="926d8-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="926d8-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="926d8-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="926d8-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="926d8-105">Die spaltenzuordnung gibt die Spaltendaten aus abgefragt und in der Datenbank gespeichert werden sollten.</span><span class="sxs-lookup"><span data-stu-id="926d8-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="926d8-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="926d8-106">Conventions</span></span>

<span data-ttu-id="926d8-107">Gemäß der Konvention wird jede Eigenschaft einer Spalte mit dem gleichen Namen wie die Eigenschaft zuordnen eingerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="926d8-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="926d8-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="926d8-108">Data Annotations</span></span>

<span data-ttu-id="926d8-109">Sie können Datenanmerkungen verwenden, um die Spalte zu konfigurieren, die eine Eigenschaft zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="926d8-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="926d8-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="926d8-110">Fluent API</span></span>

<span data-ttu-id="926d8-111">Sie können die Fluent-API verwenden, um die Spalte zu konfigurieren, die eine Eigenschaft zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="926d8-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
