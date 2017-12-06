---
title: Globale Abfragefilter - EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a>Globale Abfragefilter

Globale Abfragefilter werden LINQ-Abfrage-Prädikate (ein boolescher Ausdruck in der Regel übergeben wird, in der LINQ *, in denen* -Abfrage-Operator) auf Entitätstypen im Metadatenmodell angewendet (in der Regel in *OnModelCreating*). Solche Filter werden automatisch an alle LINQ-Abfragen im Zusammenhang mit diesen Entitätstypen, einschließlich von Entitätstypen indirekt, z. B. durch die Verwendung von Include verwiesen wird oder direkte Navigation Eigenschaftenverweise angewendet. Einige allgemeine Anwendungen diese Funktion sind:

* **Vorläufigen löschen** -ein Entitätstyp definiert ein *IsDeleted* Eigenschaft.
* **Mehrinstanzenfähigkeit** -ein Entitätstyp definiert eine *"tenantid"* Eigenschaft.

## <a name="example"></a>Beispiel

Im folgende Beispiel wird gezeigt, wie mit globalen Abfragefilter weiche löschen und mehrinstanzenfähigkeit Abfrage Verhalten in einem einfachen Blogging-Modell implementiert wird.

> [!TIP]
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) auf GitHub.

Zuerst definieren Sie die Entitäten an:

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

Beachten Sie die Deklaration einer __"tenantid"_ Feld der _Blog_ Entität. Dies wird verwendet, einen bestimmten Mandanten jede Blog-Instanz zugeordnet werden soll. Wird auch definiert ein _IsDeleted_ Eigenschaft auf die _Post_ Entitätstyp. Wird verwendet, diese Option, um nachverfolgen, ob ein _Post_ "vorläufig gelöschte" ist die Instanz wurde. D. h. Die Instanz wird als gelöschte Withouth physisch entfernt die zugrunde liegenden Daten gekennzeichnet.

Als Nächstes konfigurieren Sie die Abfragefilter in _OnModelCreating_ mithilfe der ```HasQueryFilter``` API.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

Die Prädikatausdrücke übergeben, um die _HasQueryFilter_ Aufrufe jetzt automatisch gelten für alle LINQ-Abfragen für diese Typen.

> [!TIP]
> Beachten Sie die Verwendung einer Ebene DbContext Instanzenfelds: ```_tenantId``` verwendet, um den aktuellen Mandanten festzulegen. Ebene des Modells Filter werden den Wert aus der Instanz des richtigen Kontexts verwendet. D. h. Die Instanz, die die Abfrage ausgeführt wird.

## <a name="disabling-filters"></a>Deaktivieren der Filter

Filter können für einzelne LINQ-Abfragen deaktiviert werden, mithilfe der ```IgnoreQueryFilters()``` Operator.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Einschränkungen

Globale Abfragefilter weisen die folgenden Einschränkungen:

* Filter sind keine Verweise auf Navigationseigenschaften.
* Filter können nur für den Stamm einer Vererbungshierarchie Entitätstyp definiert werden.
