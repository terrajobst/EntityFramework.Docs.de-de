---
title: Anbieterunterstützung für räumliche Typen - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: ffd22222f59a541d8135d3738d37a7e8f5dc5d7c
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489751"
---
# <a name="provider-support-for-spatial-types"></a>Unterstützung für räumliche Typen
Entitätsframework unterstützt die Arbeit mit räumlichen Daten über die DbGeography oder DbGeometry-Klassen. Diese Klassen basieren auf Datenbank-spezifische Funktionen, die die Entity Framework-Dienstanbieter angeboten werden. Nicht alle Anbieter werden räumliche Daten unterstützt, und diejenigen, die möglicherweise zusätzlicher Voraussetzungen, z. B. die Installation der räumliche Typ Assemblys. Weitere Informationen über anbieterunterstützung für räumliche Typen finden Sie weiter unten.  

Weitere Informationen zur Verwendung von räumlichen Typen in einer Anwendung finden Sie in beiden exemplarischen Vorgehensweisen, eine für Code First, die andere für Database First oder Model First:  

- [Räumliche Datentypen im Code zuerst](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Typen von räumlichen Daten im EF Designer](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>EF-Versionen, die räumliche Typen unterstützen.  

Unterstützung für räumliche Datentypen wurde in EF5 eingeführt. Jedoch werden räumliche Typen in EF5 nur unterstützt, wenn die Anwendung ausgerichtet ist und auf .NET 4.5 ausgeführt wird.  

Beginnend mit EF6, die räumliche Typen für Anwendungen, die auf .NET 4 und .NET 4.5 unterstützt werden.  

## <a name="ef-providers-that-support-spatial-types"></a>EF-Anbieter, die räumliche Typen zu unterstützen  

### <a name="ef5"></a>EF5  

Die Entity Framework-Anbieter für EF5, die wir beachten, dass Unterstützung für räumliche Typen sind:  

- Microsoft SQL Server-Anbieter  
    - Dieser Anbieter wird als Teil des EF5 ausgeliefert.  
    - Dieser Anbieter abhängig ist, auf einige zusätzlichen Low-Level-Bibliotheken, die installiert werden müssen, finden Sie weiter unten Weitere Informationen.  
- [Devart DotConnect für Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Dies ist eine Drittanbieter-Anbieter von Devart.  

Wenn Sie einen Anbieter EF5 kennen, die Sie räumliche Typen unterstützt erhalten, wenden Sie sich an, und wir werden darüber freuen, dass es zu dieser Liste hinzufügen.  

### <a name="ef6"></a>EF6  

Die Entity Framework-Anbieter für EF6, die wir beachten, dass Unterstützung für räumliche Typen sind:  

- Microsoft SQL Server-Anbieter  
    - Dieser Anbieter wird als Teil von EF6 ausgeliefert.  
    - Dieser Anbieter abhängig ist, auf einige zusätzlichen Low-Level-Bibliotheken, die installiert werden müssen, finden Sie weiter unten Weitere Informationen.  
- [Devart DotConnect für Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Dies ist eine Drittanbieter-Anbieter von Devart.  

Wenn Sie einen Anbieter für EF 6 kennen, die Sie räumliche Typen unterstützt erhalten, wenden Sie sich an, und wir werden darüber freuen, dass es zu dieser Liste hinzufügen.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Voraussetzungen für räumliche Typen, die mit Microsoft SQL Server  

Unterstützung von räumlichen Daten SQL Server hängt die Low-Level, SQL Server-spezifische Typen "sqlgeography" und "sqlgeometry" aus. Diese Typen befinden sich in "Microsoft.SqlServer.Types.dll"-Assembly, und diese Assembly ist nicht als Bestandteil von EF oder als Teil von .NET Framework geliefert.  

Bei der Installation von Visual Studio wird häufig auch eine Version von SQL Server installiert, und dies schließt die Installation von "Microsoft.SqlServer.Types.dll".  

Wenn SQL Server nicht auf dem Computer installiert ist, in dem Sie räumliche Typen verwenden möchten, oder wenn räumliche Typen aus der SQL Server-Installation ausgeschlossen wurden, müssen Sie sie manuell installieren. Die Typen können installiert werden, mithilfe von `SQLSysClrTypes.msi`, diese ist Teil von Microsoft SQL Server Feature Pack. Räumliche Typen sind SQL Server-Version-spezifisch ist, daher wir empfehlen [suchen Sie nach "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) im Microsoft Download Center, wählen Sie dann und Laden Sie auf die Version von SQL Server, die Sie verwenden die Option, die entspricht.
