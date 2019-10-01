---
title: EF6 und EF Core – Verwendung in derselben Anwendung
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 8bf9f51c0e5c4b1b3adf4a6a9a894689dc13d2d9
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149291"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="7b405-102">Verwenden von EF Core und EF6 in derselben Anwendung</span><span class="sxs-lookup"><span data-stu-id="7b405-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="7b405-103">Es ist möglich, EF Core und EF6 in ein und derselben Anwendung oder Bibliothek zu verwenden, indem beide NuGet-Pakete installiert werden.</span><span class="sxs-lookup"><span data-stu-id="7b405-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="7b405-104">Einige Typen haben in EF Core und EF6 die gleichen Namen und unterscheiden sich nur durch den Namensraum, dies kann die Verwendung von EF Core und EF6 in derselben Codedatei erschweren.</span><span class="sxs-lookup"><span data-stu-id="7b405-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="7b405-105">Die Mehrdeutigkeit lässt sich mithilfe von Anweisungen für Namespacealiase leicht beseitigen.</span><span class="sxs-lookup"><span data-stu-id="7b405-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="7b405-106">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7b405-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="7b405-107">Wenn Sie eine vorhandene Anwendung mit mehreren EF-Modellen portieren, können Sie einige von ihnen selektiv auf EF Core portieren und für die anderen weiterhin EF6 verwenden.</span><span class="sxs-lookup"><span data-stu-id="7b405-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
