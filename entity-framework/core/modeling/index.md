---
title: Erstellen und Konfigurieren eines Modells – EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412775"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="e0501-102">Erstellen und Konfigurieren eines Modells</span><span class="sxs-lookup"><span data-stu-id="e0501-102">Creating and configuring a model</span></span>

<span data-ttu-id="e0501-103">Entity Framework verwendet eine Reihe von Konventionen, um ein Modell basierend auf der Anordnung Ihrer Entitätsklassen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e0501-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="e0501-104">Sie können zusätzliche Konfigurationen angeben, um die gemäß den Konventionen gewonnenen Ergebnisse zu ergänzen und/oder zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="e0501-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="e0501-105">Dieser Artikel behandelt die Konfiguration, die auf ein Modell für einen beliebigen Datenspeicher und auf jede relationale Datenbank angewendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="e0501-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="e0501-106">Anbieter können auch eine Konfiguration aktivieren, die speziell für einen bestimmten Datenspeicher gilt.</span><span class="sxs-lookup"><span data-stu-id="e0501-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="e0501-107">Dokumentation zu anbieterspezifischen Konfigurationen finden Sie im Abschnitt  [Datenbankanbieter](../providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="e0501-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="e0501-108">Das in diesem Artikel verwendete  [Beispiel](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples)  finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="e0501-108">You can view this article’s [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="e0501-109">Verwenden der Fluent-API zum Konfigurieren eines Modells</span><span class="sxs-lookup"><span data-stu-id="e0501-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="e0501-110">Sie können die  `OnModelCreating` -Methode in Ihrem abgeleiteten Kontext überschreiben und mit der  `ModelBuilder API` Ihr Modell konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e0501-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="e0501-111">Dies ist die wirksamste Konfigurationsmethode und erlaubt die Angabe der Konfiguration, ohne die Entitätsklassen zu verändern.</span><span class="sxs-lookup"><span data-stu-id="e0501-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="e0501-112">Die Fluent-API-Konfiguration hat die höchste Priorität und überschreibt Konventionen und Datenanmerkungen.</span><span class="sxs-lookup"><span data-stu-id="e0501-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="e0501-113">Verwenden von Datenanmerkungen zum Konfigurieren eines Modells</span><span class="sxs-lookup"><span data-stu-id="e0501-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="e0501-114">Sie können auch Attribute (sogenannte „Datenanmerkungen“) auf Ihre Klassen und Eigenschaften anwenden.</span><span class="sxs-lookup"><span data-stu-id="e0501-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="e0501-115">Datenanmerkungen setzen Konventionen außer Kraft, werden aber ihrerseits von der Fluent-API-Konfiguration außer Kraft gesetzt.</span><span class="sxs-lookup"><span data-stu-id="e0501-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
