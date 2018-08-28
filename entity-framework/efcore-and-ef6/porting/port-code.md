---
title: Portieren von EF6 auf EF Core – Portieren eines codebasierten Modells
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997048"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portieren einer codebasierten Modells mit EF6 zu EF Core

Wenn Sie alle die Einschränkungen gelesen haben und portieren möchten, klicken Sie dann sind hier einige Richtlinien, die Ihnen beim Einstieg helfen.

## <a name="install-ef-core-nuget-packages"></a>Installieren von EF Core NuGet-Paketen

Um EF Core zu verwenden, installieren Sie das NuGet-Paket für den Anbieter, die, den Sie verwenden möchten. Beispielsweise, wenn SQL Server angezielt wurde, Sie würden installieren `Microsoft.EntityFrameworkCore.SqlServer`. Finden Sie unter [Datenbankanbieter](../../core/providers/index.md) Details.

Wenn Sie Migrationen verwenden möchten, sollten Sie auch installieren die `Microsoft.EntityFrameworkCore.Tools` Paket.

Es ist in Ordnung, das EF6-NuGet-Paket ("EntityFramework"), die installiert ist, zu verlassen, da EF Core und EF6 verwendete Seite-an-Seite in der gleichen Anwendung sein kann. Jedoch wenn Sie nicht sind EF6 in alle Bereiche der Anwendung verwenden, können klicken Sie dann das Paket deinstallieren Kompilierungsfehler auf Teile des Codes, die Ihre Aufmerksamkeit erfordern.

## <a name="swap-namespaces"></a>Swap-namespaces

Die meisten APIs, die in EF6 verwendet werden, der `System.Data.Entity` Namespace (und verwandte untergeordneten Namespaces). Die erste Änderung des Codes wird zum Austauschen der `Microsoft.EntityFrameworkCore` Namespace. Sie würden normalerweise beginnen Sie mit Ihrem abgeleiteten Kontext Codedatei und dann ausarbeiten dort Adressierung Kompilierungsfehler, sobald sie auftreten.

## <a name="context-configuration-connection-etc"></a>Kontextbezogene Konfigurationen der (Verbindung usw.).

Siehe [stellen Sie sicher EF Core wird Arbeit für die Anwendung](ensure-requirements.md), EF Core verfügt über weniger Magic-Befehl in der Datenbank für die Verbindung zu erkennen. Sie benötigen zum Überschreiben der `OnConfiguring` Methode auf Ihrem abgeleiteten Kontext ein, und verwenden Sie die jeweilige Datenbank-Anbieter-API, um das Einrichten der Verbindung mit der Datenbank.

Die meisten EF6-Anwendungen speichern die Verbindungszeichenfolge in den Anwendungen `App/Web.config` Datei. In EF Core Sie lesen diese Verbindungszeichenfolge mithilfe der `ConfigurationManager` API. Sie müssen möglicherweise einen Verweis auf Hinzufügen der `System.Configuration` Framework-Assembly, um diese API verwenden zu können.

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

## <a name="update-your-code"></a>Aktualisieren Sie Ihren code

An diesem Punkt ist es lediglich die Adressierung von Kompilierungsfehlern und Überprüfen von Code, um festzustellen, ob die verhaltensänderungen Auswirkungen informiert.

## <a name="existing-migrations"></a>Vorhandener Migrationen

Derzeit nicht wirklich möglich, vorhandene EF6-Migrationen zu EF Core zu portieren.

Wenn möglich, empfiehlt es sich davon ausgehen, dass alle vorherige Migrationen von EF 6 auf die Datenbank angewendet wurden, und zeigen Sie dann das Schema aus, die Migration starten mit EF Core. Zu diesem Zweck verwenden Sie die `Add-Migration` Befehl aus, um eine Migration hinzufügen, nachdem das Modell zu EF Core portiert wird. Sie würden dann entfernen Sie sämtlichen Code aus der `Up` und `Down` Methoden der erstellten Migration. Bei der anfänglichen Migration Gerüst erstellt wurde, werden die nachfolgende Migrationen für das Modell verglichen.

## <a name="test-the-port"></a>Testen Sie den port

Nur weil die Anwendung kompiliert wird, bedeutet nicht, dass es erfolgreich in EF Core portiert werden. Sie benötigen, um alle Bereiche der Anwendung, um sicherzustellen, dass keine der Änderungen des Verhaltens Ihrer Anwendung beeinträchtigt haben zu testen.
