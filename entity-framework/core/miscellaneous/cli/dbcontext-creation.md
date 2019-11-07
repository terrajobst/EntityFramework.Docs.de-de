---
title: Erstellung von dbcontext zur Entwurfszeit EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655528"
---
# <a name="design-time-dbcontext-creation"></a>DbContext-Instanzerstellung zur Entwurfszeit

Einige der EF Core Tools-Befehle (z. b. die [Migrations][1] Befehle) erfordern, dass eine abgeleitete `DbContext` Instanz zur Entwurfszeit erstellt wird, um Details zu den Entitäts Typen der Anwendung und deren Zuordnung zu einem Datenbankschema zu erfassen. In den meisten Fällen ist es wünschenswert, dass die `DbContext`, die auf diese Weise erstellt wird, auf ähnliche Weise wie Sie zur [Laufzeit][2]konfiguriert wird.

Es gibt verschiedene Möglichkeiten, wie die Tools versuchen, den `DbContext`zu erstellen:

## <a name="from-application-services"></a>Aus Anwendungsdiensten

Wenn das Startprojekt den [ASP.net Core Webhost][3] oder den [generischen .net Core-Host][4]verwendet, versuchen die Tools, das dbcontext-Objekt vom Dienstanbieter der Anwendung abzurufen.

Die Tools versuchen zunächst, den Dienstanbieter abzurufen, indem Sie `Program.CreateHostBuilder()`aufrufen, `Build()`aufrufen und dann auf die `Services`-Eigenschaft zugreifen.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> Wenn Sie eine neue ASP.net Core Anwendung erstellen, ist dieser Hook standardmäßig enthalten.

Die `DbContext` selbst und alle Abhängigkeiten im Konstruktor müssen als Dienste im Dienstanbieter der Anwendung registriert werden. Dies kann problemlos erreicht werden, indem [ein Konstruktor für den `DbContext` verwendet wird, der eine Instanz von `DbContextOptions<TContext>` als Argument annimmt][5] und die [`AddDbContext<TContext>`-Methode][6]verwendet.

## <a name="using-a-constructor-with-no-parameters"></a>Verwenden eines Konstruktors ohne Parameter

Wenn dbcontext vom Anwendungs Dienstanbieter nicht abgerufen werden kann, suchen die Tools im Projekt nach dem abgeleiteten `DbContext` Typ. Anschließend wird versucht, eine Instanz mit einem Konstruktor ohne Parameter zu erstellen. Dies kann der Standardkonstruktor sein, wenn die `DbContext` mit der [`OnConfiguring`][7] -Methode konfiguriert wird.

## <a name="from-a-design-time-factory"></a>Aus einer Entwurfszeit Factory

Sie können den Tools auch mitteilen, wie Sie dbcontext erstellen, indem Sie die `IDesignTimeDbContextFactory<TContext>`-Schnittstelle implementieren: Wenn eine Klasse, die diese Schnittstelle implementiert, sich im selben Projekt befindet wie die abgeleitete `DbContext` oder im Startprojekt der Anwendung, umgehen die Tools die andere Möglichkeiten zum Erstellen von dbcontext und Verwenden der entwurfszeitfactory.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> Der `args`-Parameter wird derzeit nicht verwendet. Es gibt [ein Problem][8] bei der Nachverfolgung der Möglichkeit, Entwurfszeit Argumente aus den Tools anzugeben.

Eine Entwurfszeit Factory kann besonders nützlich sein, wenn Sie dbcontext zur Entwurfszeit anders konfigurieren müssen als zur Laufzeit, wenn der `DbContext`-Konstruktor zusätzliche Parameter nicht in di registriert ist, wenn Sie di überhaupt nicht verwenden oder wenn Sie aus irgendeinem Grund Es empfiehlt sich, keine `BuildWebHost`-Methode in der `Main` Klasse Ihrer ASP.net Core Anwendung zu haben.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
