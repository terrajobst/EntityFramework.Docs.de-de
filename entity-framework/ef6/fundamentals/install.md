---
title: Abrufen von Entitätsframework – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 7f840a4f9e437ec12f699184339e386976e1528b
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490638"
---
# <a name="get-entity-framework"></a>Beziehen von Entitätsframework
Entitätsframework die EF-Tools für Visual Studio und die EF-Laufzeit besteht.

## <a name="ef-tools-for-visual-studio"></a>EF-Tools für Visual Studio

Die Entity Framework-Tools für Visual Studio umfassen die EF-Designers und des EF-Model-Assistenten und zuerst für die Datenbank erforderlich sind und ersten Workflows zu modellieren. EF-Tools sind in allen jüngeren Versionen von Visual Studio enthalten. Wenn Sie eine benutzerdefinierte Installation von Visual Studio ausführen, die Sie sicherstellen, dass das Element müssen, ist "Entity Framework 6-Tools" ausgewählt, entweder eine arbeitsauslastung, die es enthält oder indem Sie sie als einzelne Komponente auswählen.

Für einige frühere Versionen von Visual Studio sind aktualisierte EF-Tools als Download verfügbar. Finden Sie unter [Visual Studio-Versionen](~/ef6/what-is-new/visual-studio.md) Anleitungen dazu, wie Sie die neueste Version von EF-Tools für Ihre Version von Visual Studio zu erhalten.

## <a name="ef-runtime"></a>EF-Laufzeit

Die neueste Version von Entity Framework steht als die [EntityFramework NuGet-Paket](http://nuget.org/packages/EntityFramework/). Wenn Sie nicht mit dem NuGet-Paket-Manager vertraut sind, empfehlen wir Ihnen das Lesen der [NuGet-Übersicht](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Das EF-NuGet-Paket installieren

Sie können das EntityFramework-Paket installieren, indem Sie mit der rechten Maustaste auf die **Verweise** Ordner des Projekts, und wählen **NuGet-Pakete verwalten...**

![NuGet-Pakete verwalten](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Installieren von Paket-Manager-Konsole

Alternativ können Sie mithilfe des folgenden Befehls EntityFramework installieren die [-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Installieren eine bestimmte Version von Entity Framework

In EF 4.1 oder höher, neue Versionen der EF-Laufzeit veröffentlicht wurden als die [EntityFramework NuGet-Paket](https://www.nuget.org/packages/EntityFramework/). Eine dieser Versionen können hinzugefügt werden ein Projekt .NET Framework-basierten mithilfe des folgenden Befehls im Visual Studio [-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

``` powershell
Install-Package EntityFramework -Version <number>
```

Beachten Sie, dass `<number>` stellt dar, die bestimmte Version von EF zu installieren. So ist beispielsweise 6.2.0 fest, der Versionsnummer für EF 6.2.   

EF-Laufzeiten vor 4.1 waren Teil von .NET Framework und können nicht separat installiert werden.

### <a name="installing-the-latest-preview"></a>Installieren der aktuellsten Preview

Die oben genannten Methoden erhalten Sie die neueste Version von Entity Framework vollständig unterstützt. Es gibt häufig Vorabversionen von Entity Framework verfügbar, die wir sicherlich gern nutzen würden Sie testen, und geben Sie uns Feedback zu.

Um die aktuelle Vorschau von EntityFramework zu installieren, Sie auswählen können, **Include Prerelease** im Fenster NuGet-Pakete verwalten. Wenn keine Vorabversionen verfügbar sind, automatisch erhalten Sie die neueste Version vollständig unterstützte Version von Entity Framework.

![Vorabversion einbeziehen](~/ef6/media/includeprerelease.png)

Alternativ können Sie den folgenden Befehl ausführen, der [-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
