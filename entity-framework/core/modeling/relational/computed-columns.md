---
title: Berechnete Spalten-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655919"
---
# <a name="computed-columns"></a><span data-ttu-id="b9717-102">Berechnete Spalten</span><span class="sxs-lookup"><span data-stu-id="b9717-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="b9717-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="b9717-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b9717-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="b9717-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b9717-105">Eine berechnete Spalte ist eine Spalte, deren Wert in der Datenbank berechnet wird.</span><span class="sxs-lookup"><span data-stu-id="b9717-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="b9717-106">Für eine berechnete Spalte können andere Spalten in der Tabelle verwendet werden, um ihren Wert zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="b9717-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="b9717-107">Konventionen</span><span class="sxs-lookup"><span data-stu-id="b9717-107">Conventions</span></span>

<span data-ttu-id="b9717-108">Gemäß der Konvention werden berechnete Spalten nicht im Modell erstellt.</span><span class="sxs-lookup"><span data-stu-id="b9717-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b9717-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="b9717-109">Data Annotations</span></span>

<span data-ttu-id="b9717-110">Berechnete Spalten können nicht mit Daten Anmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="b9717-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b9717-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="b9717-111">Fluent API</span></span>

<span data-ttu-id="b9717-112">Sie können die fließende API verwenden, um anzugeben, dass eine Eigenschaft einer berechneten Spalte zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="b9717-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
