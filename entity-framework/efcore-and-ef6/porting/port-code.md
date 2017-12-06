---
title: Portieren von EF6 auf EF-Core - Portieren eine Code-basiertes Modell
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portieren von EF6 Code basierende Modell EF-Kern

Wenn Sie alle Einschränkungen gelesen haben und Sie bereit, Port sind, sind hier einige Richtlinien, die Ihnen beim Einstieg helfen.

## <a name="install-ef-core-nuget-packages"></a>Installieren von EF Core NuGet-Pakete

Wenn EF Core verwenden möchten, installieren Sie das NuGet-Paket für den Datenbankanbieter, die, den Sie verwenden möchten. Beispielsweise, wenn SQL Server verwenden möchten, Sie würden installieren `Microsoft.EntityFrameworkCore.SqlServer`. Finden Sie unter [Datenbankanbieter](../../core/providers/index.md) Details.

Wenn Sie planen die Migrationen zu verwenden, installieren Sie auch die `Microsoft.EntityFrameworkCore.Tools` Paket.

Es ist in Ordnung, um das EF6 NuGet-Paket (EntityFramework) installiert ist, zu lassen, als EF Core und EF6 verwendete Seite-an-Seite in derselben Anwendung sein können. Jedoch wenn Sie nicht sind EF6 in alle Bereiche der Anwendung zu verwenden, können klicken Sie dann das Paket deinstallieren Kompilierungsfehler auf Codeabschnitte, die Aufmerksamkeit erfordern.

## <a name="swap-namespaces"></a>Swap-namespaces

Die meisten APIs, die in EF6 verwendet werden, der `System.Data.Entity` Namespace (und in verwandten Sub-Namespaces). Die erste Änderung des Codes wird zum Austauschen der `Microsoft.EntityFrameworkCore` Namespace. Sie würden in der Regel beginnen Sie mit der abgeleiteten Kontext Codedatei und dann ausarbeiten dort Adressierung Kompilierungsfehler, sobald sie auftreten.

## <a name="context-configuration-connection-etc"></a>Konfiguration des Sicherheitskontexts (Verbindung usw.).

Wie in beschrieben [sicherzustellen EF wird Hauptarbeit, das für die Anwendung](ensure-requirements.md), EF Core hat weniger Magic, um das Erkennen von der Datenbank für die Verbindung. Müssen Sie überschreiben die `OnConfiguring` Methode auf dem abgeleiteten Kontext und verwenden Sie die Datenbank-API-Anbieter spezifische beim Einrichten der Verbindung mit der Datenbank.

Die meisten EF6 Anwendungen die Verbindungszeichenfolge speichern, die in der Anwendung `App/Web.config` Datei. In der EF-Kern, Sie Lesen dieser Verbindungszeichenfolge mithilfe der `ConfigurationManager` API. Sie müssen möglicherweise einen Verweis auf Hinzufügen der `System.Configuration` Framework-Assembly, um diese API verwenden zu können.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a>Aktualisieren Sie den code

An diesem Punkt ist es nur wenige Adressierung Kompilierungsfehler enthält, und Überprüfen von Code, um festzustellen, ob die verändertes Programmverhalten auswirkt.

## <a name="existing-migrations"></a>Vorhandene Migrationen

Es ist eigentlich möglich ermöglichen die vorhandene EF6-Migrationen zu EF Core.

Wenn möglich, empfiehlt es sich, wird davon ausgegangen, dass alle vorherigen Migrationen von EF6 an die Datenbank angewendet wurden, und zeigen Sie dann das Schema aus, die Migration starten mit EF Core. Zu diesem Zweck verwenden Sie die `Add-Migration` Befehl aus, um eine Migration hinzufügen, nachdem das Modell zu EF Core portiert werden. Sie würden dann entfernen Sie sämtlichen Code aus der `Up` und `Down` Methoden der scaffolded Migration. Nachfolgende Migrationen werden für das Modell verglichen, wenn diese anfänglichen Migration Gerüstbau wurde.

## <a name="test-the-port"></a>Testen Sie den port

Einfach, da die Anwendung kompiliert wird, bedeutet nicht, dass es erfolgreich zu EF Core portiert werden. Sie müssen zum Testen der Anwendung, um sicherzustellen, dass keines der verhaltensänderungen Ihrer Anwendung negativ beeinträchtigt, haben alle Bereiche.
