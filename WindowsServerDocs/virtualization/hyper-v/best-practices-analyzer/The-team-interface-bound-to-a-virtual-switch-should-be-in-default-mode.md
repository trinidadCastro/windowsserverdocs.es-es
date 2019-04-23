---
title: La interfaz de equipo enlazada a un conmutador virtual debe ser de forma predeterminada en modo
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 22e5ad0eed6e6ea07a83150762b76163442f2c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872166"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>La interfaz de equipo enlazada a un conmutador virtual debe ser de forma predeterminada en modo

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
*Algunos conmutadores virtuales están enlazados a una interfaz de equipo, pero la interfaz de equipo no pasa el tráfico en todas las VLAN para los conmutadores virtuales.*  
  
## <a name="impact"></a>**Impact**  
*Los conmutadores virtuales siguientes no pueden tener acceso a todas las VLAN: \n{0}*  
  
## <a name="resolution"></a>**Resolución**  
*Use el Administrador de servidor o el cmdlet Set-NetLbfoTeamNic de Windows PowerShell para restablecer la interfaz de equipo en el modo predeterminado.*  
  


