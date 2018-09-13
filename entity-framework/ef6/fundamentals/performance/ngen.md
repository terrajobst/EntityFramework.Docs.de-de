---
title: Verbessern der Leistung mit NGen - EF6 beim Start
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: 2e9a93c7741046da25b890fed084a6b280bf5643
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489959"
---
# <a name="improving-startup-performance-with-ngen"></a>Verbessern der Leistung mit NGen beim Start
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

.NET Framework unterstützt die Generierung von systemeigenen Images für verwaltete Anwendungen und Bibliotheken, die als eine Methode, um Anwendungen schneller gestartet werden und auch in einigen Fällen verwenden Sie weniger Arbeitsspeicher. Systemeigene Images werden durch die Übersetzung von Assemblys mit verwaltetem Code in Dateien, die systemeigene maschinenanweisungen enthält, bevor die Anwendung ausgeführt wird, sind daher weniger erspart, generieren die systemeigenen Anweisungen unter den .NET JIT (Just-In-Time)-Compiler erstellt Laufzeit der Anwendung.  

Vor Version 6 die EF-Runtime-Core-Bibliotheken wurden Teil von .NET Framework, und systemeigene Images automatisch für sie generiert wurden. Ab Version 6 wurde die gesamte EF-Runtime in EntityFramework NuGet-Paket kombiniert. Systemeigene Images haben, jetzt generiert mit dem NGen.exe-Befehlszeilentool, um ähnliche Ergebnisse zu erhalten.  

Empirische Beobachtungen zeigen, dass systemeigene Images die EF Runtime nach Assemblys zwischen 1 und 3 Sekunden der Startzeit der Anwendung verringert werden können.  

## <a name="how-to-use-ngenexe"></a>Gewusst wie: Verwenden von NGen.exe  

Ist die grundlegendste Funktion dem NGen.exe-Tool "installieren" (d. h. erstellen und auf dem Datenträger beibehalten) systemeigene Images für eine Assembly und alle direkten Abhängigkeiten. Hier ist, wie Sie die, die erreichen können:  

1. Öffnen Sie ein Eingabeaufforderungsfenster als administrator  
2. Ändern Sie das aktuelle Arbeitsverzeichnis auf den Speicherort der Assemblys, denen Sie für die systemeigene Abbilder generieren möchten:  

  ``` console
    cd <*Assemblies location*>  
  ```
3. Je nach Betriebssystem und die Konfiguration der Anwendung müssen Sie zum Generieren von systemeigenen Images für 32-Bit-Architektur, 64-Bit-Architektur oder für beides.  

    Für 32-Bit ausführen:  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    Für 64-Bit ausführen:
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> Generieren von systemeigenen Images für der falschen Architektur ist ein sehr häufiger Fehler. Im Zweifelsfall können Sie systemeigene Images für alle Architekturen, die für das Betriebssystem auf dem Computer installiert gelten einfach generieren.  

NGen.exe unterstützt auch andere Funktionen wie z. B. deinstallieren und die installierten systemeigenen Images, die queuing der Generierung von mehreren Images usw. anzeigen. Lesen Sie weitere Informationen zur Verwendung der [NGen.exe Dokumentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx).  

## <a name="when-to-use-ngenexe"></a>Verwendung von NGen.exe  

Wenn es geht zu entscheiden, welche Assemblys sollen für die systemeigene Abbilder in einer Anwendung basierend auf der EF-Version 6 oder höher zu generieren, sollten Sie die folgenden Optionen berücksichtigen:  

- **Die EF Runtime Hauptassembly EntityFramework.dll**: eine typische EF-basierten Anwendung führt eine erhebliche Menge an Code aus dieser Assembly, die beim Start oder auf den ersten Zugriff auf die Datenbank. Erstellen systemeigener Images dieser Assembly wird daher die größten Vorteile bei der Leistung beim Start generiert werden.  
- **Jede EF-Anbieter-Assembly, die von Ihrer Anwendung verwendeten**: Dauer des Verbindungsstarts kann ebenfalls leicht generiert systemeigene Images davon profitieren. Z. B. wenn die Anwendung den EF-Anbieter für SQL Server verwendet. möchten Sie ein systemeigenes Image für EntityFramework.SqlServer.dll zu generieren.  
- **Die Assemblys Ihrer Anwendung und andere Abhängigkeiten**: die [NGen.exe Dokumentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx) behandelt allgemeine Kriterien für die Auswahl von welche Assemblys sollen für die systemeigene Abbilder und die Auswirkungen von systemeigenen Images auf Sicherheit, generieren Erweiterte Optionen wie z. B. "feste Bindung" Szenarios wie die Verwendung im Debug- und Profilerstellungstools Szenarien usw. für systemeigene Abbilder.  

> [!TIP]
> Stellen Sie sicher, dass Sie sorgfältig die Auswirkungen der Verwendung von systemeigenen Images auf die Leistung beim Starten und die gesamtleistung der Anwendung zu messen, und vergleichen Sie sie mit die tatsächlichen Anforderungen. Zwar die systemeigenen Images in der Regel starten Sie die Leistung verbessern und in einigen Fällen die arbeitsspeicherbelegung reduzieren, werden nicht alle Szenarien gleichermaßen profitieren. Z. B. bei der Ausführung von stabilen Zustand kann (d. h., sobald alle Methoden, die von der Anwendung verwendeten mindestens einmal aufgerufen haben) vom JIT-Compiler generierte Code tatsächlich etwas bessere Leistung als systemeigene Abbilder erzielt werden.  

## <a name="using-ngenexe-in-a-development-machine"></a>Verwendung von NGen.exe auf einem Entwicklungscomputer  

Während der Entwicklung der .NET JIT-Compiler bietet Compiler die beste allgemeinen Kompromiss für Code, die häufig geändert wird. Generieren von systemeigenen Images für kompilierte Abhängigkeiten wie z. B. die EF Runtime nach Assemblys können bei Entwicklung und Tests, durch die Reduzierung von ein paar Sekunden die zu Beginn jeder Ausführung zu beschleunigen.  

Ein guter Ausgangspunkt, um die EF Runtime nach Assemblys zu finden ist, den Speicherort des NuGet-Pakets für die Lösung. Z. B. für eine Anwendung mithilfe von EF 6.0.2 mit SQL Server und als Ziel .NET 4.5 oder höher, und Sie können Folgendes in ein Eingabeaufforderungsfenster (Beachten Sie, um sie als Administrator zu öffnen):  

``` console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Diese nutzt die Tatsache, die Installation von systemeigenen Images für EF-Anbieter für SQL Server wird standardmäßig auch die systemeigenen Images für die Hauptassembly der EF-Laufzeit installiert wird. Dies funktioniert, da NGen.exe erkennen kann, dass EntityFramework.dll direkte Abhängigkeit von der EntityFramework.SqlServer.dll-Assembly befindet sich im gleichen Verzeichnis ist.  

## <a name="creating-native-images-during-setup"></a>Erstellen systemeigener Images während des Setups  

Dem WiX-Toolkit unterstützt die Generierung von systemeigenen Images für verwaltete Assemblys während des Setups queuing, wie dies unter [Leitfaden](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html). Eine weitere Alternative ist beim Erstellen einer benutzerdefinierten Installation-Aufgabe, die den NGen.exe-Befehl ausführen.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Überprüfen, dass systemeigene Images für Entity Framework verwendet werden  

Sie können überprüfen, ob eine bestimmte Anwendung eine native Assembly verwendet anhand der geladenen Assemblys mit der Erweiterung ". ni.dll"oder". ni.exe". Beispielsweise wird ein systemeigenes Image für die EF-main Runtime-Assembly EntityFramework.ni.dll aufgerufen. Eine einfache Möglichkeit, überprüfen Sie die geladenen Assemblys von .NET eines Prozesses ist die Verwendung [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Andere Dinge zu beachten  

**Erstellen ein systemeigenes Image einer Assembly darf nicht mit Registrieren der Assembly in verwechselt werden die [GAC (Global Assembly Cache)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**. NGen.exe ermöglicht das Erstellen von Images der Assemblys, die nicht im globalen Assemblycache enthalten sind, und in der Tat können mehrere Anwendungen, die eine bestimmte Version von EF verwenden das gleiche native Image freigeben. Während Windows 8 automatisch native Images für Assemblys im GAC abgelegt erstellen können, die EF Runtime wurde optimiert, um zusammen mit Ihrer Anwendung bereitgestellt werden, und es wird nicht empfohlen, im GAC zu registrieren, da dies negative Auswirkungen auf die Assembly-Auflösung hat und Wartung Ihrer Anwendungen auf andere Aspekte.  
