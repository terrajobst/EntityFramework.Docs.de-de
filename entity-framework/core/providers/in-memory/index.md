---
title: In-Memory Database-Anbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 2aed8db79157f994bcc259acdf1f7c161017a50a
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824402"
---
# <a name="ef-core-in-memory-database-provider"></a>EF Core-In-Memory Database-Anbieter

Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird. Dies kann für Tests nützlich sein, obwohl der SQLite-Anbieter im In-Memory-Modus für Testersatz für relationale Datenbanken besser geeignet sein kann. Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.

## <a name="install"></a>Installieren

Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.InMemory“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

## <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core-CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a>Erste Schritte

Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:

* [Testen mit InMemory](../../miscellaneous/testing/in-memory.md)
* [Tests der UnicornStore-Beispielanwendung](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Unterstützte Datenbank-Engines

In-Process-Speicherdatenbank (nur für Testzwecke konzipiert)
