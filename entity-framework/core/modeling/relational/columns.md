---
title: Spalten Zuordnung-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197210"
---
# <a name="column-mapping"></a><span data-ttu-id="4db08-102">Spaltenzuordnung</span><span class="sxs-lookup"><span data-stu-id="4db08-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="4db08-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="4db08-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="4db08-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="4db08-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="4db08-105">Die Spalten Zuordnung identifiziert, welche Spaltendaten aus der Datenbank abgefragt und in dieser gespeichert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="4db08-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="4db08-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="4db08-106">Conventions</span></span>

<span data-ttu-id="4db08-107">Gemäß der Konvention wird jede Eigenschaft so eingerichtet, dass Sie einer Spalte mit dem gleichen Namen wie die Eigenschaft zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="4db08-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4db08-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="4db08-108">Data Annotations</span></span>

<span data-ttu-id="4db08-109">Sie können Daten Anmerkungen verwenden, um die Spalte zu konfigurieren, der eine Eigenschaft zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="4db08-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="4db08-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="4db08-110">Fluent API</span></span>

<span data-ttu-id="4db08-111">Sie können die fließende API verwenden, um die Spalte zu konfigurieren, der eine Eigenschaft zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="4db08-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
