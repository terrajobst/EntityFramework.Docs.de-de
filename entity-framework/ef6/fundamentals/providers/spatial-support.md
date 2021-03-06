---
title: Anbieter Unterstützung für räumliche Typen EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413894"
---
# <a name="provider-support-for-spatial-types"></a>Anbieter Unterstützung für räumliche Typen
Entity Framework unterstützt das Arbeiten mit räumlichen Daten durch die dbgeography-Klasse oder die dbgeometry-Klasse. Diese Klassen basieren auf datenbankspezifischen Funktionen, die der Entity Framework Anbieter bietet. Nicht alle Anbieter unterstützen räumliche Daten, und solche, die möglicherweise zusätzliche Voraussetzungen aufweisen, wie z. b. die Installation von Assemblys für räumliche Weitere Informationen zur Anbieter Unterstützung für räumliche Typen finden Sie weiter unten.  

Weitere Informationen zur Verwendung räumlicher Typen in einer Anwendung finden Sie in zwei exemplarischen Vorgehensweisen, einer für Code First, der andere für Database First oder Model First:  

- [Räumliche Datentypen in Code First](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Räumliche Datentypen im EF-Designer](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>EF-Releases, die räumliche Typen unterstützen  

Unterstützung für räumliche Typen wurde in EF5 eingeführt. In EF5 werden räumliche Typen jedoch nur unterstützt, wenn die Anwendung auf .NET 4,5 abzielt und ausgeführt wird.  

Beginnend mit EF6 räumliche Typen werden für Anwendungen unterstützt, die auf .NET 4 und .NET 4,5 abzielen.  

## <a name="ef-providers-that-support-spatial-types"></a>EF-Anbieter, die räumliche Typen unterstützen  

### <a name="ef5"></a>EF5  

Die Entity Framework Anbieter für EF5, die wir uns bewusst sind und räumliche Typen unterstützen, sind:  

- Microsoft SQL Server Anbieter  
    - Dieser Anbieter wird als Teil von EF5 ausgeliefert.  
    - Dieser Anbieter hängt von einigen zusätzlichen, auf Low-Level-Bibliotheken ab, die möglicherweise installiert werden müssen – weitere Informationen finden Sie weiter unten.  
- [Devart dotConnect für Oracle](https://www.devart.com/dotconnect/oracle/)  
    - Dabei handelt es sich um einen Drittanbieter von Devart.  

Wenn Sie von einem EF5-Anbieter wissen, der räumliche Typen unterstützt, wenden Sie sich bitte an den Kontakt, und wir freuen uns, ihn dieser Liste hinzuzufügen.  

### <a name="ef6"></a>EF6  

Die Entity Framework Anbieter für EF6, die wir uns bewusst sind und räumliche Typen unterstützen, sind:  

- Microsoft SQL Server Anbieter  
    - Dieser Anbieter wird als Teil von EF6 ausgeliefert.  
    - Dieser Anbieter hängt von einigen zusätzlichen, auf Low-Level-Bibliotheken ab, die möglicherweise installiert werden müssen – weitere Informationen finden Sie weiter unten.  
- [Devart dotConnect für Oracle](https://www.devart.com/dotconnect/oracle/)  
    - Dabei handelt es sich um einen Drittanbieter von Devart.  

Wenn Sie von einem EF6-Anbieter wissen, der räumliche Typen unterstützt, wenden Sie sich bitte an den Kontakt, und wir freuen uns, ihn dieser Liste hinzuzufügen.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Voraussetzungen für räumliche Typen mit Microsoft SQL Server  

SQL Server räumliche Unterstützung hängt von den SQL Server spezifischen Typen "SQLGeography" und "SQLGeometry" auf niedriger Ebene ab. Diese Typen befinden sich in der Microsoft. SqlServer. types. dll-Assembly, und diese Assembly wird nicht als Teil von EF oder als Teil der .NET Framework ausgeliefert.  

Wenn Visual Studio installiert ist, wird häufig auch eine Version von SQL Server installiert. dazu gehört auch die Installation von Microsoft. SqlServer. types. dll.  

Wenn SQL Server nicht auf dem Computer installiert ist, auf dem Sie räumliche Typen verwenden möchten, oder wenn räumliche Typen aus der SQL Server Installation ausgeschlossen wurden, müssen Sie diese manuell installieren. Die Typen können mit `SQLSysClrTypes.msi`installiert werden, der Teil von Microsoft SQL Server Feature Pack ist. Räumliche Typen sind SQL Server Versions spezifisch. Daher wird empfohlen, im Microsoft Download Center [nach "SQL Server Feature Pack" zu suchen](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) und dann die Option auszuwählen, die der Version von SQL Server entspricht, die Sie verwenden möchten.
