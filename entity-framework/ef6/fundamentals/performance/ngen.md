---
title: Verbessern der Startleistung mit Ngen-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: 841aec645abdb2a56076d0b70bfb2614b0acafb4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416090"
---
# <a name="improving-startup-performance-with-ngen"></a>Verbessern der Startleistung mit Ngen
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

Der .NET Framework unterstützt die Generierung von systemeigenen Images für verwaltete Anwendungen und Bibliotheken, damit Anwendungen schneller gestartet werden und in manchen Fällen weniger Arbeitsspeicher verwendet werden kann. Systemeigene Images werden durch die Übersetzung von verwalteten Codeassemblys in Dateien erstellt, die vor der Ausführung der Anwendung systemeigene Maschinenanweisungen enthalten, sodass der .net JIT-Compiler (Just-in-Time) die systemeigenen Anweisungen nicht generieren muss. Anwendungs Laufzeit.  

Vor Version 6 waren die Kernbibliotheken der EF-Laufzeit Teil der .NET Framework, und Native Images wurden automatisch für Sie generiert. Ab Version 6 wurde die gesamte EF-Laufzeit in das nuget-Paket "EntityFramework" kombiniert. Native Images müssen jetzt mit dem Befehlszeilen Tool "ngen. exe" generiert werden, um ähnliche Ergebnisse zu erhalten.  

Empirische Beobachtungen zeigen, dass systemeigene Images der EF-Laufzeitassemblys zwischen 1 und 3 Sekunden der Startzeit der Anwendung Ausschneiden können.  

## <a name="how-to-use-ngenexe"></a>Verwenden von "ngen. exe"  

Die grundlegendste Funktion des Tools ngen. exe besteht darin, systemeigene Images für eine Assembly und alle direkten Abhängigkeiten zu installieren (d. h., um Datenträger zu erstellen und auf dem Datenträger zu speichern). So können Sie dies erreichen:  

1. Öffnen Sie ein Eingabeaufforderungsfenster als ein Administrator.
2. Ändern Sie das aktuelle Arbeitsverzeichnis in den Speicherort der Assemblys, für die Sie Native Images generieren möchten:

   ``` console
   cd <*Assemblies location*>  
   ```

3. Abhängig von Ihrem Betriebssystem und der Konfiguration der Anwendung müssen Sie möglicherweise systemeigene Images für die 32-Bit-Architektur, 64 Bit-Architektur oder beides generieren.

   Für 32-Bit-Run:

   ``` console
   %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
   ```

   Für 64-Bit-Run:
  
   ``` console
   %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
   ```

> [!TIP]
> Die Generierung System eigener Images für die falsche Architektur ist ein sehr häufiger Fehler. Im Zweifelsfall können Sie einfach Native Images für alle Architekturen generieren, die für das auf dem Computer installierte Betriebssystem gelten.  

"Ngen. exe" unterstützt auch andere Funktionen, wie z. b. das Deinstallieren und Anzeigen installierter System eigener Images, die Warteschlange der Generierung mehrerer Images usw. Weitere Informationen zur Verwendung finden Sie in der Dokumentation zu " [Ngen. exe](https://msdn.microsoft.com/library/6t9t5wcf.aspx)".  

## <a name="when-to-use-ngenexe"></a>Verwendung von "ngen. exe"  

Wenn Sie entscheiden, welche Assemblys für die Generierung System eigener Images in einer Anwendung, die auf EF, Version 6 oder höher, basieren soll, sollten Sie die folgenden Optionen berücksichtigen:  

- **Die Hauptversion der EF-Runtime (EntityFramework. dll**): eine typische EF-basierte Anwendung führt beim Start oder beim ersten Zugriff auf die Datenbank eine beträchtliche Menge an Code aus dieser Assembly aus. Folglich führt das Erstellen von systemeigenen Images dieser Assembly zu den größten Vorteilen bei der Startleistung.  
- **Eine beliebige EF-Anbieterassembly, die von Ihrer Anwendung verwendet wird**: die Startzeit kann auch leicht von der Generierung System eigener Images profitieren. Wenn die Anwendung z. b. den EF-Anbieter für SQL Server verwendet, sollten Sie ein System eigenes Image für "EntityFramework. SqlServer. dll" generieren.  
- Die **Assemblys der Anwendung und andere Abhängigkeiten**: in der Dokumentation zu " [Ngen. exe](https://msdn.microsoft.com/library/6t9t5wcf.aspx) " werden allgemeine Kriterien für die Auswahl der Assemblys, für die systemeigene Images generiert werden sollen, sowie die Auswirkungen von systemeigenen Images auf die Sicherheit, erweiterte Optionen wie "feste Bindung", Szenarios wie die Verwendung von systemeigenen Images in Debug-  

> [!TIP]
> Stellen Sie sicher, dass Sie die Auswirkungen der Verwendung System eigener Images auf die Startleistung und die Gesamtleistung Ihrer Anwendung sorgfältig messen, und vergleichen Sie Sie mit den tatsächlichen Anforderungen. Während Native Images in der Regel dabei helfen, die Leistung beim Start zu verbessern und in einigen Fällen die Speicherauslastung zu reduzieren, sind nicht alle Szenarien gleichermaßen vorteilhaft. Beispielsweise kann der vom JIT-Compiler generierte Code bei der stabilen Zustands Ausführung (d. h., nachdem alle Methoden, die von der Anwendung verwendet werden, mindestens einmal aufgerufen wurde) eine etwas bessere Leistung als systemeigene Images erzielen.  

## <a name="using-ngenexe-in-a-development-machine"></a>Verwenden von "ngen. exe" auf einem Entwicklungs Computer  

Während der Entwicklung bietet der .net-JIT-Compiler den besten Kompromiss für Code, der häufig geändert wird. Das Generieren von systemeigenen Images für kompilierte Abhängigkeiten, wie z. b. die EF-Laufzeitassemblys, kann dazu beitragen, die Entwicklung und Tests zu beschleunigen, indem am Anfang jeder Ausführung  

Eine gute Stelle zum Auffinden der EF-Laufzeitassemblys ist der Speicherort des nuget-Pakets für die Lösung. Beispielsweise können Sie für eine Anwendung, die EF 6.0.2 mit SQL Server und für .NET 4,5 oder höher verwendet, Folgendes in einem Eingabe Aufforderungs Fenster eingeben (denken Sie daran, es als Administrator zu öffnen):  

```console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Dadurch wird die Tatsache genutzt, dass bei der Installation der systemeigenen Images für den EF-Anbieter für SQL Server standardmäßig auch die systemeigenen Images für die Haupt-EF-Runtime-Assembly installiert werden. Dies funktioniert, weil "ngen. exe" erkennen kann, dass "EntityFramework. dll" eine direkte Abhängigkeit von der EntityFramework. SqlServer. dll-Assembly ist, die sich im selben Verzeichnis befindet.  

## <a name="creating-native-images-during-setup"></a>Erstellen von systemeigenen Images während des Setups  

Das WiX-Toolkit unterstützt die Erstellung von systemeigenen Images für verwaltete Assemblys während des Setups in der Warteschlange, wie in dieser [Anleitung](https://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html)erläutert. Eine weitere Alternative besteht darin, eine benutzerdefinierte Setup Aufgabe zu erstellen, die den Befehl "ngen. exe" ausführt.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Es wird überprüft, ob systemeigene Images für EF verwendet werden.  

Sie können überprüfen, ob eine bestimmte Anwendung eine native Assembly verwendet, indem Sie nach geladenen Assemblys mit der Erweiterung ". ni. dll" oder ". ni. exe" suchen. Beispielsweise wird ein System eigenes Image für die Haupt Laufzeit-Assembly von EF als EntityFramework. ni. dll bezeichnet. Eine einfache Möglichkeit zum Überprüfen der geladenen .NET-Assemblys eines Prozesses ist die Verwendung des [Prozess-Explorers](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Weitere Dinge, die Sie beachten sollten  

**Das Erstellen eines systemeigenen Images einer Assembly sollte nicht mit dem Registrieren der Assembly im [GAC (globaler Assemblycache)](https://msdn.microsoft.com/library/yf1d93sz.aspx)verwechselt werden**. "Ngen. exe" ermöglicht das Erstellen von Images von Assemblys, die sich nicht im globalen Assemblycache befinden. tatsächlich können mehrere Anwendungen, die eine bestimmte Version von EF verwenden, dasselbe systemeigene Image gemeinsam verwenden. Obwohl Windows 8 automatisch systemeigene Images für Assemblys erstellen kann, die im GAC platziert werden, ist die EF-Laufzeit für die Bereitstellung mit der Anwendung optimiert, und wir empfehlen Ihnen nicht, Sie im GAC zu registrieren, da dies negative Auswirkungen auf die Assemblyauflösung hat und die Wartung Ihrer Anwendungen unter anderen Aspekten.  
