---
title: Schreiben eine-Datenbankanbieter – EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993665"
---
# <a name="writing-a-database-provider"></a>Schreiben eines Datenbankanbieters

Informationen zum Schreiben von einem Entity Framework Core-Datenbankanbieter, finden Sie unter [einen EF Core-Anbieter schreiben, empfiehlt sich daher](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) von [Arthur Vickers](https://github.com/ajcvickers).

> [!NOTE]
> Diese Beiträge seit EF Core 1.1 nicht aktualisiert wurden, und es wurden bedeutende Änderungen seit diesem Zeitpunkt [Problem 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) Nachverfolgen von Aktualisierungen in dieser Dokumentation wird.

Die Codebasis von EF Core ist open Source und enthält mehrere Datenbankanbietern, die als Verweis verwendet werden können. Sie finden den Quellcode auf https://github.com/aspnet/EntityFrameworkCore. Es kann auch hilfreich sein, z. B. den Code für häufig verwendete Drittanbieter betrachten [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo-MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), und [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). Insbesondere werden diese Projekte erweitern aus, und führen Funktionstests, die wir Veröffentlichen in NuGet-Setup. Diese Art von Setup wird dringend empfohlen.

## <a name="keeping-up-to-date-with-provider-changes"></a>Behalten auf dem neuesten Stand mit Anbieter-Änderungen

Starten mit der Arbeit nach der Version 2.1, wir haben eine [Änderungen protokolliert](provider-log.md) , die möglicherweise die entsprechenden Änderungen in den Anbietercode. Dies soll helfen bei der Aktualisierung eines vorhandenen Anbieters mit einer neuen Version von EF Core funktioniert.

Vor dem 2.1, verwendet der [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) und [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) Bezeichnungen auf unserer GitHub-Problemen und Pull Requests für einen ähnlichen Zweck. Wir werden Continiue, um diese zu Problemen mit dem eine Angabe über das erteilen, welche Arbeitsaufgaben in einer bestimmten Version ebenfalls im Anbieter auszuführenden Arbeit erfordern. Ein `providers-beware` Bezeichnung bedeutet normalerweise, dass es sich bei die Implementierung eines Arbeitselements Anbieter unterbrechen kann während einer `providers-fyi` Bezeichnung bedeutet normalerweise, dass Anbieter werden nicht beeinträchtigt, aber der Code möglicherweise dennoch, z. B. geändert werden, um neue Funktionen zu aktivieren.

## <a name="suggested-naming-of-third-party-providers"></a>Empfohlene Benennung von Drittanbietern

Es wird empfohlen, mit dem folgenden Namen für NuGet-Pakete. Dies ist konsistent mit den Namen der Pakete, die von der EF Core-Team bereitgestellt werden.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Zum Beispiel:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
