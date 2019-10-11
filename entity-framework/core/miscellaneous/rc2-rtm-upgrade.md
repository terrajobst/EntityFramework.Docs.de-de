---
title: Aktualisieren von EF Core 1,0 rc2 auf RTM-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: e7f121d18931e26e7b5d11842da6da4a9b789efe
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181365"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Aktualisieren von EF Core 1,0 rc2 auf RTM

Dieser Artikel enthält Anleitungen zum Verschieben einer Anwendung, die mit den RC2-Paketen erstellt wurde, in 1.0.0 RTM.

## <a name="package-versions"></a>Paketversionen

Die Namen der Pakete auf oberster Ebene, die normalerweise in einer Anwendung installiert werden, haben sich zwischen RC2 und RTM nicht geändert.

**Sie müssen die installierten Pakete auf die RTM-Versionen aktualisieren:**

* Lauf Zeit Pakete (z. b. `Microsoft.EntityFrameworkCore.SqlServer`) wurden von `1.0.0-rc2-final` in `1.0.0` geändert.

* Das Paket "`Microsoft.EntityFrameworkCore.Tools`" wurde von `1.0.0-preview1-final` in `1.0.0-preview2-final` geändert. Beachten Sie, dass die Tools noch vorab veröffentlicht werden.

## <a name="existing-migrations-may-need-maxlength-added"></a>Vorhandene Migrationen müssen möglicherweise MaxLength hinzugefügt werden.

In RC2 betrachtete die Spaltendefinition in einer Migration wie `table.Column<string>(nullable: true)`, und die Länge der Spalte wurde in einigen Metadaten gesucht, die im Code hinter der Migration gespeichert werden. In RTM ist die Länge jetzt im Gerüsten Code `table.Column<string>(maxLength: 450, nullable: true)` enthalten.

Für alle vorhandenen Migrationen, die vor der Verwendung von RTM erstellt wurden, ist das `maxLength`-Argument nicht angegeben. Dies bedeutet, dass die maximale Länge, die von der Datenbank unterstützt wird, verwendet wird (`nvarchar(max)` auf SQL Server). Dies kann für einige Spalten in Ordnung sein, aber Spalten, die Teil eines Schlüssels, eines fremd Schlüssels oder eines Indexes sind, müssen aktualisiert werden, damit Sie eine maximale Länge enthalten. Gemäß der Konvention ist 450 die maximale Länge, die für Schlüssel, Fremdschlüssel und indizierte Spalten verwendet wird. Wenn Sie im Modell explizit eine Länge konfiguriert haben, sollten Sie diese Länge stattdessen verwenden.

**ASP.NET Identity**

Diese Änderung wirkt sich auf Projekte aus, die ASP.net Identity verwenden und aus einer Pre-RTM-Projektvorlage erstellt wurden. Die Projektvorlage enthält eine Migration, mit der die Datenbank erstellt wird. Diese Migration muss bearbeitet werden, um für die folgenden Spalten eine maximale Länge von `256` anzugeben.

*  **AspNetRoles**

    * Name

    * Normalizedname

*  **AspNetUsers**

   * E-Mail

   * Normalizedebug

   * NormalizedUserName

   * UserName

Wenn Sie diese Änderung nicht vornehmen, wird die folgende Ausnahme ausgelöst, wenn die anfängliche Migration auf eine Datenbank angewendet wird.

```console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a>.NET Core: "Importe" in "Project. JSON" entfernen

Wenn Sie .net Core mit RC2 als Zielplattform verwendet haben, mussten Sie "Project. JSON" `imports` als temporäre Problem Umgehung für einige EF Core Abhängigkeiten hinzufügen, die .NET Standard nicht unterstützen. Diese können nun entfernt werden.

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
> Ab Version 1,0 RTM unterstützt die [.net Core SDK](https://www.microsoft.com/net/download/core) nicht mehr `project.json` oder die Entwicklung von .net Core-Anwendungen mit Visual Studio 2015. Es wird empfohlen, [eine Migration von „project.json“ zu „csproj“ durchzuführen](https://docs.microsoft.com/dotnet/articles/core/migration/). Wenn Sie Visual Studio verwenden, wird empfohlen, ein Upgrade auf [Visual Studio 2017](https://www.visualstudio.com/downloads/)auszuführen.

## <a name="uwp-add-binding-redirects"></a>UWP Bindungs Umleitungen hinzufügen

Der Versuch, EF-Befehle für universelle Windows-Plattform-Projekte (UWP) auszuführen, führt zu folgendem Fehler:

```console
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

Sie müssen dem UWP-Projekt manuell Bindungs Umleitungen hinzufügen. Erstellen Sie eine Datei mit dem Namen "`App.config`" im Projektstamm Ordner, und fügen Sie den richtigen Assemblyversionen Umleitungen hinzu.

```xml
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
