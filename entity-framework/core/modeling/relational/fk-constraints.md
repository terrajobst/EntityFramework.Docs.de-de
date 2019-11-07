---
title: Foreign Key-Einschränkungen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655998"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="02bd7-102">Fremdschlüsseleinschränkungen</span><span class="sxs-lookup"><span data-stu-id="02bd7-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="02bd7-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="02bd7-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="02bd7-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="02bd7-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="02bd7-105">Eine FOREIGN KEY-Einschränkung wird für jede Beziehung im Modell eingeführt.</span><span class="sxs-lookup"><span data-stu-id="02bd7-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="02bd7-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="02bd7-106">Conventions</span></span>

<span data-ttu-id="02bd7-107">Gemäß der Konvention werden Foreign Key-Einschränkungen `FK_<dependent type name>_<principal type name>_<foreign key property name>`benannt.</span><span class="sxs-lookup"><span data-stu-id="02bd7-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="02bd7-108">Bei zusammengesetzten Fremdschlüsseln `<foreign key property name>` zu einer durch Trennzeichen getrennten Liste von Fremdschlüssel Eigenschaftsnamen.</span><span class="sxs-lookup"><span data-stu-id="02bd7-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="02bd7-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="02bd7-109">Data Annotations</span></span>

<span data-ttu-id="02bd7-110">Namen von Foreign Key-Einschränkungen können nicht mithilfe von Daten Anmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="02bd7-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="02bd7-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="02bd7-111">Fluent API</span></span>

<span data-ttu-id="02bd7-112">Sie können die fließende API verwenden, um den Namen der FOREIGN KEY-Einschränkung für eine Beziehung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="02bd7-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
