---
title: Primärschlüssel-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656047"
---
# <a name="primary-keys"></a><span data-ttu-id="a19bd-102">Primärschlüssel</span><span class="sxs-lookup"><span data-stu-id="a19bd-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="a19bd-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="a19bd-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a19bd-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="a19bd-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a19bd-105">Eine PRIMARY KEY-Einschränkung wird für den Schlüssel jedes Entitäts Typs eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a19bd-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="a19bd-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="a19bd-106">Conventions</span></span>

<span data-ttu-id="a19bd-107">Gemäß der Konvention wird der Primärschlüssel in der Datenbank `PK_<type name>`benannt.</span><span class="sxs-lookup"><span data-stu-id="a19bd-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a19bd-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a19bd-108">Data Annotations</span></span>

<span data-ttu-id="a19bd-109">Keine relationalen datenbankspezifischen Aspekte eines Primärschlüssels können mithilfe von Daten Anmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="a19bd-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a19bd-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="a19bd-110">Fluent API</span></span>

<span data-ttu-id="a19bd-111">Sie können die fließende API verwenden, um den Namen der PRIMARY KEY-Einschränkung in der Datenbank zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a19bd-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
