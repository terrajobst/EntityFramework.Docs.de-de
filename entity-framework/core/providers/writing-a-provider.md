---
title: Schreiben eines Datenbankanbieters-EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 2d9e4a6cdfda80d7dfcfb6e7bf0480eb49f8e057
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414806"
---
# <a name="writing-a-database-provider"></a>Schreiben eines Datenbankanbieters

Weitere Informationen zum Schreiben eines Entity Framework Core Datenbankanbieters finden [Sie unter so können Sie einen EF Core Anbieter](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) von [Arthur Vickers](https://github.com/ajcvickers)schreiben.

> [!NOTE]
> Diese Beiträge wurden seit EF Core 1,1 nicht aktualisiert, und es wurden bedeutende Änderungen seit diesem Zeitpunkt ausgegeben, in dem die Aktualisierung dieser Dokumentation durch das [Problem 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) nachverfolgt wird.

Die EF Core Codebasis ist Open Source und enthält mehrere Datenbankanbieter, die als Referenz verwendet werden können. Sie finden den Quellcode unter <https://github.com/aspnet/EntityFrameworkCore>. Es kann auch hilfreich sein, sich den Code für häufig verwendete Drittanbieter Anbieter anzusehen, wie z. b. [npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)und [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). Insbesondere sind diese Projekte so eingerichtet, dass Sie von den Funktionstests, die auf nuget veröffentlicht werden, erweitert und ausgeführt werden. Diese Art von Setup wird dringend empfohlen.

## <a name="keeping-up-to-date-with-provider-changes"></a>Beibehalten der aktuellen Änderungen an Anbietern

Beginnend mit der Arbeit nach Version 2,1 haben wir ein [Protokoll der Änderungen](provider-log.md) erstellt, die ggf. entsprechende Änderungen im Anbieter Code erfordern. Dies soll beim Aktualisieren eines vorhandenen Anbieters helfen, um mit einer neuen Version von EF Core Arbeiten zu können.

Vor 2,1 haben wir die Bezeichnungen " [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) " und " [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) " auf unseren GitHub-Problemen und Pull Requests für einen ähnlichen Zweck verwendet. Wir werden diese Bezeichnungen für Probleme verwenden, um anzugeben, welche Arbeitsaufgaben in einem bestimmten Release möglicherweise auch die Arbeit in Anbietern erfordern. Eine `providers-beware` Bezeichnung bedeutet in der Regel, dass die Implementierung eines Arbeits Elements die Anbieter unterbrechen kann, während eine `providers-fyi` Bezeichnung normalerweise bedeutet, dass Anbieter nicht beschädigt werden, der Code jedoch möglicherweise trotzdem geändert werden muss, z. b., um neue Funktionen zu aktivieren.

## <a name="suggested-naming-of-third-party-providers"></a>Empfohlene Benennung von Drittanbieter Anbietern

Wir empfehlen, die folgenden Namen für nuget-Pakete zu verwenden. Dies stimmt mit den Namen von Paketen überein, die vom EF Core Team geliefert werden.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Beispiel:

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
