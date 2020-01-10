---
title: Testen mit inMemory-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: fcd2f99ad06fd30ef9e36fd1e5a6a09fe0a45d07
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781117"
---
# <a name="testing-with-inmemory"></a>Testen mit InMemory

Der inMemory-Anbieter ist nützlich, wenn Sie Komponenten mit etwas, das eine Verbindung mit der realen Datenbank herstellt, testen möchten, ohne den Aufwand tatsächlicher Daten Bank Vorgänge zu erhöhen.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) finden Sie auf GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory ist keine relationale Datenbank.

EF Core Datenbankanbieter müssen keine relationalen Datenbanken sein. InMemory ist als allgemeine Datenbank für Tests konzipiert und ist nicht für die imitiert einer relationalen Datenbank konzipiert.

Beispiele hierfür sind:

* InMemory ermöglicht Ihnen das Speichern von Daten, die gegen Einschränkungen der referenziellen Integrität in einer relationalen Datenbank verstoßen.
* Wenn Sie defaultvaluesql (String) für eine Eigenschaft im Modell verwenden, handelt es sich hierbei um eine relationale Datenbank-API, die bei der Ausführung für inMemory keine Auswirkung hat.
* Parallelität [über Timestamp/Row-Version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` oder `IsRowVersion`) wird nicht unterstützt. [Dbupdateconcurrency cyexception](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) wird nicht ausgelöst, wenn ein Update mit einem alten Parallelitäts Token ausgeführt wird.

> [!TIP]  
> Für viele Testzwecke sind diese Unterschiede nicht von Bedeutung. Wenn Sie jedoch eine Prüfung auf etwas durchsetzen möchten, das sich wie eine echte relationale Datenbank verhält, sollten Sie den [sqlite-in-Memory-Modus](sqlite.md)verwenden.

## <a name="example-testing-scenario"></a>Beispiel für ein Testszenario

Beachten Sie den folgenden Dienst, der es Anwendungscode ermöglicht, einige mit Blogs verbundene Vorgänge auszuführen. Intern wird eine `DbContext` verwendet, die eine Verbindung mit einer SQL Server Datenbank herstellt. Es wäre hilfreich, diesen Kontext auszutauschen, um eine Verbindung mit einer inMemory-Datenbank herzustellen, damit wir effiziente Tests für diesen Dienst schreiben können, ohne den Code ändern zu müssen, oder viel Arbeit erledigen müssen, um einen Test Double-Kontext zu erstellen.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Vorbereiten Ihres Kontexts

### <a name="avoid-configuring-two-database-providers"></a>Vermeiden der Konfiguration von zwei Datenbankanbietern

In den Tests werden Sie den Kontext extern so konfigurieren, dass der inMemory-Anbieter verwendet wird. Wenn Sie einen Datenbankanbieter konfigurieren, indem Sie `OnConfiguring` in ihrem Kontext überschreiben, müssen Sie einigen bedingten Code hinzufügen, um sicherzustellen, dass Sie den Datenbankanbieter nur dann konfigurieren, wenn noch keiner konfiguriert wurde.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Wenn Sie ASP.net Core verwenden, sollte dieser Code nicht benötigt werden, da der Datenbankanbieter bereits außerhalb des Kontexts (in Startup.cs) konfiguriert wurde.

### <a name="add-a-constructor-for-testing"></a>Hinzufügen eines Konstruktors zum Testen

Die einfachste Möglichkeit, Tests für eine andere Datenbank zu aktivieren, besteht darin, den Kontext so zu ändern, dass er einen Konstruktor verfügbar macht, der eine `DbContextOptions<TContext>`akzeptiert.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` weist den Kontext alle seine Einstellungen zu, z. b. die Datenbank, mit der eine Verbindung hergestellt werden soll. Dabei handelt es sich um das gleiche Objekt, das erstellt wird, indem die on-Konfigurations Methode in ihrem Kontext ausgeführt wird.

## <a name="writing-tests"></a>Schreiben von Tests

Der Schlüssel zum Testen mit diesem Anbieter ist die Möglichkeit, den Kontext anzuweisen, den inMemory-Anbieter zu verwenden, und den Bereich des in-Memory Database zu steuern. In der Regel benötigen Sie eine saubere Datenbank für jede Testmethode.

Im folgenden finden Sie ein Beispiel für eine Testklasse, die die inMemory-Datenbank verwendet. Jede Testmethode gibt einen eindeutigen Datenbanknamen an, was bedeutet, dass jede Methode über eine eigene inMemory-Datenbank verfügt.

>[!TIP]
> Wenn Sie die `.UseInMemoryDatabase()`-Erweiterungsmethode verwenden möchten, verweisen Sie auf das nuget-Paket [Microsoft. entityframeworkcore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
