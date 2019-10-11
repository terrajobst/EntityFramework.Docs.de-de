---
title: 'Portieren von EF6 auf EF Core portieren eines Code basierten Modells: EF'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181214"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portieren eines EF6-Code basierten Modells auf EF Core

Wenn Sie alle Vorsichtsmaßnahmen gelesen haben und Sie zum Portieren bereit sind, finden Sie hier einige Richtlinien, die Ihnen den Einstieg erleichtern.

## <a name="install-ef-core-nuget-packages"></a>Installieren von EF Core nuget-Paketen

Um EF Core zu verwenden, installieren Sie das nuget-Paket für den Datenbankanbieter, den Sie verwenden möchten. Wenn Sie z. b. auf SQL Server abzielen, würden Sie `Microsoft.EntityFrameworkCore.SqlServer` installieren. Weitere Informationen finden Sie unter [Datenbankanbieter](../../core/providers/index.md) .

Wenn Sie beabsichtigen, Migrationen zu verwenden, sollten Sie auch das `Microsoft.EntityFrameworkCore.Tools`-Paket installieren.

Es ist in Ordnung, das EF6 nuget-Paket (EntityFramework) installiert zu lassen, da EF Core und EF6 parallel in der gleichen Anwendung verwendet werden können. Wenn Sie jedoch nicht beabsichtigen, EF6 in einem Bereich Ihrer Anwendung zu verwenden, kann das Deinstallieren des Pakets dabei helfen, Kompilier Fehler für Code Elemente zu erhalten, die eine Aufmerksamkeit erfordern.

## <a name="swap-namespaces"></a>Namespaces austauschen

Die meisten APIs, die Sie in EF6 verwenden, sind im `System.Data.Entity`-Namespace (und den zugehörigen Subnamespaces) enthalten. Die erste Codeänderung besteht darin, den `Microsoft.EntityFrameworkCore`-Namespace auszutauschen. In der Regel beginnen Sie mit der abgeleiteten Kontext Codedatei und können dann von dort aus arbeiten, um Kompilierungsfehler zu beheben, sobald sie auftreten.

## <a name="context-configuration-connection-etc"></a>Kontext Konfiguration (Verbindung usw.)

Wie unter [sicherstellen, dass EF Core für Ihre Anwendung funktioniert](ensure-requirements.md), hat EF Core weniger Magie im Hinblick auf das Erkennen der Datenbank, mit der eine Verbindung hergestellt werden soll. Sie müssen die `OnConfiguring`-Methode in Ihrem abgeleiteten Kontext überschreiben und die Datenbankanbieter spezifische API verwenden, um die Verbindung mit der Datenbank einzurichten.

In den meisten EF6-Anwendungen wird die Verbindungs Zeichenfolge in der Anwendung `App/Web.config` gespeichert. In EF Core lesen Sie diese Verbindungs Zeichenfolge mit der `ConfigurationManager`-API. Sie müssen möglicherweise auf die Framework-Assembly `System.Configuration` verweisen, um diese API verwenden zu können.

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

## <a name="update-your-code"></a>Aktualisieren Ihres Codes

An dieser Stelle ist es wichtig, Kompilierungsfehler zu beheben und Code zu überprüfen, um festzustellen, ob sich die Verhaltensänderungen auf Sie auswirken.

## <a name="existing-migrations"></a>Vorhandene Migrationen

Es ist nicht wirklich möglich, vorhandene EF6-Migrationen auf EF Core zu portieren.

Wenn möglich, sollten Sie davon ausgehen, dass alle vorherigen Migrationen von EF6 auf die Datenbank angewendet wurden und dann EF Core mit der Migration des Schemas von diesem Punkt aus beginnen. Dazu verwenden Sie den Befehl "`Add-Migration`", um eine Migration hinzuzufügen, sobald das Modell in EF Core portiert wurde. Anschließend würden Sie den gesamten Code aus den Methoden `Up` und `Down` der Gerüst Migration entfernen. Nachfolgende Migrationen werden mit dem Modell verglichen, wenn für die anfängliche Migration ein Gerüst erstellt wurde.

## <a name="test-the-port"></a>Testen des Ports

Nur weil Ihre Anwendung kompiliert, bedeutet nicht, dass Sie erfolgreich in EF Core portiert wurde. Sie müssen alle Bereiche Ihrer Anwendung testen, um sicherzustellen, dass keine der Verhaltensänderungen Ihre Anwendung beeinträchtigt hat.
