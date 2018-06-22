---
title: EF6 und EF Core – Verwendung in derselben Anwendung
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: f6eb4bf7d99fbc61f8ffbd0dc7c6c17789395303
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054824"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="7102d-102">Verwenden von EF Core und EF6 in derselben Anwendung</span><span class="sxs-lookup"><span data-stu-id="7102d-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="7102d-103">Es ist möglich, EF Core und EF6 in ein und derselben .NET Framework-Anwendung oder -Bibliothek zu verwenden, indem beide NuGet-Pakete installiert werden.</span><span class="sxs-lookup"><span data-stu-id="7102d-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span> 

<span data-ttu-id="7102d-104">Einige Typen haben in EF Core und EF6 die gleichen Namen und unterscheiden sich nur durch den Namensraum, dies kann die Verwendung von EF Core und EF6 in derselben Codedatei erschweren.</span><span class="sxs-lookup"><span data-stu-id="7102d-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="7102d-105">Die Mehrdeutigkeit lässt sich mithilfe von Direktiven für Namespacealiase leicht beseitigen. Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7102d-105">The ambiguity can be easily removed using namespace alias directives, e.g.:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

<span data-ttu-id="7102d-106">Wenn Sie eine vorhandene Anwendung mit mehreren EF-Modellen portieren, können Sie einige von ihnen selektiv auf EF Core portieren und für die anderen weiterhin EF6 verwenden.</span><span class="sxs-lookup"><span data-stu-id="7102d-106">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
