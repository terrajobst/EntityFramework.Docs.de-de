---
title: Aktualisieren von EF Core 1.0 RC2 auf RTM – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 1b95b2ab1943dfb541b3a7c873cff3cb4c16d9c1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998318"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Aktualisieren von EF Core 1.0 RC2 auf RTM-Version

Dieser Artikel enthält Anleitungen zum Verschieben von einer Anwendung erstellt, mit dem RC2-Paketen auf 1.0.0 RTM-Version.

## <a name="package-versions"></a>Paketversionen

Die Namen der Pakete auf oberster Ebene, die Sie in der Regel in eine Anwendung installieren würden wurde zwischen RC2 und RTM-Version nicht geändert werden.

**Sie müssen die installierten Pakete auf die RTM-Versionen zu aktualisieren:**

* Runtime-Pakete (z. B. `Microsoft.EntityFrameworkCore.SqlServer`) von geändert `1.0.0-rc2-final` zu `1.0.0`.

* Die `Microsoft.EntityFrameworkCore.Tools` Paket geändert `1.0.0-preview1-final` zu `1.0.0-preview2-final`. Beachten Sie, dass Tools immer noch eine Vorabversion.

## <a name="existing-migrations-may-need-maxlength-added"></a>Vorhandene Migrationen möglicherweise MaxLength hinzugefügt

In RC2, sah die Spaltendefinition in einer Migration `table.Column<string>(nullable: true)` und die Länge der Spalte in einige Metadaten wir in den Code hinter der Migration speichern gesucht wurde. In der RTM-Version ist jetzt die Länge in dem eingerüsteten Code enthalten `table.Column<string>(maxLength: 450, nullable: true)`.

Alle vorhandenen Migrationen, bei denen Gerüst erstellt wurden, vor der Verwendung von RTM-Version müssen nicht die `maxLength` Argument angegeben. Dies bedeutet, dass die maximale Länge, die von der Datenbank unterstützt werden kann (`nvarchar(max)` auf SQL Server). Dies ist möglicherweise für einige Spalten, aber die Spalten, die Teil eines Schlüssels, foreign key- oder Index müssen aktualisiert werden, um eine maximale Länge enthalten. Gemäß der Konvention ist 450, die maximale Länge für Schlüssel, Fremdschlüssel, verwendet und die indizierten Spalten. Wenn Sie eine Länge explizit in das Modell konfiguriert haben, sollten Sie stattdessen diese Länge verwenden.

**ASP.NET Identity**

Diese Änderung wirkt sich auf die Projekte, die aus einer vorab erstellten und Verwenden von ASP.NET Identity-RTM-Projektvorlage. Die Projektvorlage enthält eine Migration verwendet, um die Datenbank zu erstellen. Diese Migration muss bearbeitet werden, zum Angeben einer maximalen Länge von `256` für die folgenden Spalten.

*  **AspNetRoles**

    * name

    * NormalizedName

*  **AspNetUsers**

   * E-Mail

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Fehler für diese Änderung führt in der folgenden Ausnahme, wenn die ursprüngliche Migration auf eine Datenbank angewendet wird.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET Core: Entfernen von "Imports" in "Project.JSON"

Wenn Sie .NET Core mit RC2 verwenden wurden, mussten Sie fügen `imports` Datei "Project.JSON" als vorübergehende problemumgehung für einige der EF Core Abhängigkeiten, die nicht unterstützen .NET Standard. Diese können jetzt entfernt werden.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> Ab Version 1.0 RTM-Version der [.NET Core SDK](https://www.microsoft.com/net/download/core) unterstützt nicht mehr `project.json` oder .NET Core-Anwendungen mit Visual Studio 2015 entwickeln. Es wird empfohlen, [eine Migration von „project.json“ zu „csproj“ durchzuführen](https://docs.microsoft.com/dotnet/articles/core/migration/). Wenn Sie Visual Studio verwenden, sollten Sie Sie ein upgrade auf [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP: Fügen Sie bindungsumleitungen hinzu

Es wird versucht, EF-Befehle auf der universellen Windows-Plattform (UWP) von Teamprojekten führt zu folgendem Fehler ausgeführt:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Sie müssen das UWP-Projekt manuell bindungsumleitungen hinzufügen. Erstellen Sie eine Datei mit dem Namen `App.config` im Projekt Stammordner, und die richtigen Assemblyversionen umleitungen hinzugefügt.

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
