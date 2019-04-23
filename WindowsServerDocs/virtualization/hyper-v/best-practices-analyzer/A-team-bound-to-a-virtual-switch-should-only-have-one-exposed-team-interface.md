---
title: Un equipo que se enlaza a un conmutador virtual solo debe tener una interfaz de equipo expuesto
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 108bbec1439959bb7ab4475b59c7231653952ea8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838466"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>Un equipo que se enlaza a un conmutador virtual solo debe tener una interfaz de equipo expuesto

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>**Problema**  
*Uno o más conmutadores virtuales están enlazados a un equipo que tiene varias interfaces del equipo.*  
  
## <a name="impact"></a>**Impact**  
*Los siguientes modificadores virtuales podrían no tener acceso a las VLAN y el ancho de banda usado por otras interfaces del equipo:*  
  
\<lista de conmutadores virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Use el cmdlet de Windows PowerShell Remove-NetLbfoTeamNic para quitar todas las interfaces del equipo desde el equipo que no sea de la interfaz de equipo predeterminada.*  
  


