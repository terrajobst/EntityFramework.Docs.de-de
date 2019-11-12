---
title: Verwalten von Datenbankschemas – EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655646"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="dbc6e-102">Verwalten von Datenbankschemas</span><span class="sxs-lookup"><span data-stu-id="dbc6e-102">Managing Database Schemas</span></span>

<span data-ttu-id="dbc6e-103">EF Core bietet zwei wesentliche Möglichkeiten, wie Sie Ihr EF Core-Modell und Ihr Datenbankschema synchron halten können. Um zwischen den beiden Optionen zu wählen, legen Sie fest, ob Ihr EF Core-Modell oder das Datenbankschema die einzige zuverlässige Datenquelle darstellt.</span><span class="sxs-lookup"><span data-stu-id="dbc6e-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="dbc6e-104">Wenn das EF Core-Modell die einzige zuverlässige Datenquelle darstellen soll, verwenden Sie [Migrationen][1].</span><span class="sxs-lookup"><span data-stu-id="dbc6e-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="dbc6e-105">Wenn Sie Änderungen an Ihrem EF Core-Modell vornehmen, werden bei dieser Vorgehensweise schrittweise die entsprechenden Schemaänderungen an Ihrer Datenbank vorgenommen, um die Kompatibilität mit Ihrem EF Core-Modell aufrechtzuerhalten.</span><span class="sxs-lookup"><span data-stu-id="dbc6e-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="dbc6e-106">Verwenden Sie [Reverse Engineering][2], wenn Ihr Datenbankschema die einzige zuverlässige Datenquelle sein soll.</span><span class="sxs-lookup"><span data-stu-id="dbc6e-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="dbc6e-107">Diese Vorgehensweise ermöglicht es Ihnen, ein Gerüst für DbContext und die Entitätstypklassen durch Reverse Engineering Ihres Datenbankschemas in ein EF Core-Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="dbc6e-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="dbc6e-108">Die [APIs zum Erstellen und Löschen][3] können ebenfalls das Datenbankschema anhand Ihres EF Core-Modells erstellen.</span><span class="sxs-lookup"><span data-stu-id="dbc6e-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="dbc6e-109">Diese werden jedoch in erster Linie für Tests, die Prototyperstellung und andere Szenarien eingesetzt, in denen die Datenbank gelegentlich gelöscht werden soll.</span><span class="sxs-lookup"><span data-stu-id="dbc6e-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
