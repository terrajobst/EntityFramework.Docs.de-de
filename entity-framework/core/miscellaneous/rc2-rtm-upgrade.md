---
title: Aktualisieren von EF Core 1.0 RTM - RC2 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 7a1d85949a5f9e1ad7efdbf585a608d815e8ce63
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Aktualisieren von EF Core 1.0 RC2 auf RTM-Version

Dieser Artikel enthält Hinweise zum Verschieben einer Anwendung erstellt, die mit den Paketen 1.0.0 RC2 RTM-Version.

## <a name="package-versions"></a>Paketversionen

Die Namen der obersten Ebene Pakete, die Sie in der Regel in einer Anwendung installieren möchten zwischen RC2 und RTM nicht ändern.

**Sie müssen die installierten Pakete auf die RTM-Versionen aktualisieren:**

* Laufzeit-Pakete (z. B. `Microsoft.EntityFrameworkCore.SqlServer`) angebotskennzeichen `1.0.0-rc2-final` auf `1.0.0`.

* Die `Microsoft.EntityFrameworkCore.Tools` Paket angebotskennzeichen `1.0.0-preview1-final` auf `1.0.0-preview2-final`. Beachten Sie, dass die Tools sind immer noch eine Vorabversion ist.

## <a name="existing-migrations-may-need-maxlength-added"></a>Vorhandene Migrationen möglicherweise MaxLength hinzugefügt

In RC2 die Spaltendefinition in einer Migration aussah `table.Column<string>(nullable: true)` und die Länge der Spalte in einige Metadaten wir in den Code für die Migration speichern nachgeschlagen wurde. In der RTM-Version, die Länge jetzt im scaffolded Code enthalten `table.Column<string>(maxLength: 450, nullable: true)`.

Alle vorhandenen Migrationen, bei denen vor der Verwendung von RTM Gerüstbau wurden keine der `maxLength` Argument wurde angegeben. Dies bedeutet, dass die maximale Länge, die von der Datenbank unterstützt werden kann (`nvarchar(max)` auf SQL Server). Dies ist möglicherweise für einige Spalten, jedoch die Spalten, die Teil eines Schlüssels, Fremdschlüssel, sind in Ordnung, oder Index muss aktualisiert werden, um eine maximale Länge enthalten. Gemäß der Konvention ist 450 die maximale Länge für Schlüssel, Fremdschlüssel, verwendet und volltextindizierte Spalten. Wenn Sie explizit eine Länge in das Modell konfiguriert haben, sollten Sie stattdessen diese Länge verwenden.

**ASP.NET Identity**

Diese Änderung wirkt sich auf Projekte, die ASP.NET Identity verwendet und erstellt wurden, aus einer vor-RTM-Projektvorlage. Die Projektvorlage enthält eine Migration zum Erstellen der Datenbank verwendet. Diese Migration bearbeitet werden muss, zum Angeben einer maximalen Länge von `256` für die folgenden Spalten.

*  **AspNetRoles**

    * Name

    * NormalizedName

*  **AspNetUsers**

   * E-Mail

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Diese Änderung wird in der folgenden Ausnahme ansonsten bei die anfängliche Migration auf eine Datenbank angewendet wird.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET Core: Entfernen von "Imports" in "Project.JSON"

Bei der Ausrichtung auf .NET Core mit RC2 wurden benötigt hinzuzufügende `imports` auf "Project.JSON" als vorübergehende problemumgehung für einige der EF kernabhängigkeiten .NET Standard, nicht unterstützt. Diese können jetzt entfernt werden.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

## <a name="uwp-add-binding-redirects"></a>Universelle Windows-Plattform: Fügen Sie bindungsumleitungen hinzu

Es wird versucht, EF-Befehle auf die universelle Windows-Plattform (UWP) Teamprojekten führt zu folgendem Fehler ausgeführt:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

In diesem Fall müssen Sie eine uwp-Projekt manuell bindungsumleitungen hinzufügen. Erstellen Sie eine Datei mit dem Namen `App.config` im Projekt root-Ordner und die richtigen Assemblyversionen leitet hinzuzufügen.

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
