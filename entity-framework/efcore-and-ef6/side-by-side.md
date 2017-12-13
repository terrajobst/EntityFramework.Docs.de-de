---
title: "EF6 und EF Core – Verwendung in derselben Anwendung"
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
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Verwenden von EF Core und EF6 in derselben Anwendung

Es ist möglich, EF Core und EF6 in ein und derselben .NET Framework-Anwendung oder -Bibliothek zu verwenden, indem beide NuGet-Pakete installiert werden. 

Einige Typen haben in EF Core und EF6 die gleichen Namen und unterscheiden sich nur durch den Namensraum, dies kann die Verwendung von EF Core und EF6 in derselben Codedatei erschweren. Die Mehrdeutigkeit lässt sich mithilfe von Direktiven für Namespacealiase leicht beseitigen. Beispiel:

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

Wenn Sie eine vorhandene Anwendung mit mehreren EF-Modellen portieren, können Sie einige von ihnen selektiv auf EF Core portieren und für die anderen weiterhin EF6 verwenden.
