---
title: Schreiben eines Datenbank-Anbieters - EF Core
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 4bddf5858ab2c6b2fd22571a20edb3f7c85e2853
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678959"
---
# <a name="writing-a-database-provider"></a>Schreiben eines Datenbank-Anbieters

Informationen über das Schreiben eines Entity Framework Core-Datenbank-Anbieters finden Sie unter [, damit Sie einen Core EF-Anbieter schreiben möchten](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) von [Arthur Vickers](https://github.com/ajcvickers).

Die Codebasis EF Core ist eine open Source und enthält mehrere Datenbankanbieter, die als Verweis verwendet werden können. Sie finden den Quellcode zur https://github.com/aspnet/EntityFrameworkCore.

## <a name="the-providers-beware-label"></a>Der Anbieter Vorsicht vor Bezeichnung

Nachdem Sie die Arbeit für einen Anbieter beginnen, sehen Sie sich für die [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) Bezeichnung auf unserer GitHub-Problemen und Pull-Anforderungen. Wir verwenden diese Bezeichnung zum Identifizieren von Änderungen, die Anbieterwriter auswirken können.

## <a name="suggested-naming-of-third-party-providers"></a>Vorgeschlagene Benennung von Service-Drittanbietern

Es wird empfohlen, die folgenden Benennung für das NuGet-Pakete verwenden. Dies ist konsistent mit den Namen der Pakete, die vom Team EF Core übermittelt.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Zum Beispiel:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
