---
title: Überblick über Entity Framework Core – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412835"
---
# <a name="entity-framework-core"></a><span data-ttu-id="96eda-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="96eda-102">Entity Framework Core</span></span>

<span data-ttu-id="96eda-103">Entity Framework Core (EF Core) ist eine einfache, erweiterbare und plattformübergreifende [Open Source](https://github.com/aspnet/EntityFrameworkCore)-Version der beliebten Entity Framework-Datenzugriffstechnologie.</span><span class="sxs-lookup"><span data-stu-id="96eda-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="96eda-104">EF Core kann als objektrelationaler Mapper (O/RM) eingesetzt werden und bietet .NET-Entwicklern so die Möglichkeit, unter Verwendung von .NET-Objekten mit einer Datenbank zu arbeiten. Auf diese Weise entfällt ein Großteil des Datenzugriffscodes, der üblicherweise geschrieben werden muss.</span><span class="sxs-lookup"><span data-stu-id="96eda-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="96eda-105">Einzelheiten zu den von EF Core unterstützten Datenbank-Engines finden Sie unter [Datenbankanbieter](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="96eda-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="96eda-106">Das Modell</span><span class="sxs-lookup"><span data-stu-id="96eda-106">The Model</span></span>

<span data-ttu-id="96eda-107">Bei EF Core erfolgt der Datenzugriff über ein Modell.</span><span class="sxs-lookup"><span data-stu-id="96eda-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="96eda-108">Ein Modell setzt sich aus Entitätsklassen und einem Kontextobjekt zusammen, das eine Sitzung mit der Datenbank darstellt und Ihnen das Abfragen und Speichern von Daten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="96eda-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="96eda-109">Weitere Informationen finden Sie unter [Erstellen eines Modells](modeling/index.md).</span><span class="sxs-lookup"><span data-stu-id="96eda-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="96eda-110">Sie können ein Modell aus einer vorhandenen Datenbank generieren, ein Modell manuell entsprechend Ihrer Datenbank codieren oder mithilfe von [EF-Migrationen](managing-schemas/migrations/index.md) eine Datenbank anhand Ihres Modells erstellen und sie im Laufe der Zeit gemäß Ihres Modells weiterentwickeln.</span><span class="sxs-lookup"><span data-stu-id="96eda-110">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](managing-schemas/migrations/index.md) to create a database from your model, and then evolve it as your model changes over time.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a><span data-ttu-id="96eda-111">Abfragen</span><span class="sxs-lookup"><span data-stu-id="96eda-111">Querying</span></span>

<span data-ttu-id="96eda-112">Instanzen Ihrer Entitätsklassen werden mit Language Integrated Query (LINQ) von der Datenbank abgerufen.</span><span class="sxs-lookup"><span data-stu-id="96eda-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="96eda-113">Weitere Informationen finden Sie unter [Abfragen von Daten](querying/index.md).</span><span class="sxs-lookup"><span data-stu-id="96eda-113">See [Querying Data](querying/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a><span data-ttu-id="96eda-114">Speichern von Daten</span><span class="sxs-lookup"><span data-stu-id="96eda-114">Saving Data</span></span>

<span data-ttu-id="96eda-115">Daten werden in der Datenbank mithilfe von Instanzen Ihrer Entitätsklassen erstellt, gelöscht und geändert.</span><span class="sxs-lookup"><span data-stu-id="96eda-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="96eda-116">Weitere Informationen finden Sie unter [Speichern von Daten](saving/index.md).</span><span class="sxs-lookup"><span data-stu-id="96eda-116">See [Saving Data](saving/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a><span data-ttu-id="96eda-117">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="96eda-117">Next steps</span></span>

<span data-ttu-id="96eda-118">Einführungstutorials finden Sie unter [Erste Schritte mit Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="96eda-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>
