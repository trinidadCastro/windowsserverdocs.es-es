---
title: Usar todas las funciones virtuales de red cuando estén disponibles
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3ad120ffa689f1f7dcae832432e216ebda57e62f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877796"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>Usar todas las funciones virtuales de red cuando estén disponibles

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*No se están utilizando algunas capacidades de aceleración de hardware*  
  
## <a name="impact"></a>Impacto  
*Esta configuración puede provocar general de la CPU sea superior al necesario. Rendimiento de red podría no ser óptima en las máquinas virtuales siguientes:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Considere la posibilidad de configurar el adaptador de red virtual de SR-IOV si el hardware físico es compatible con SR-IOV y si esta configuración no entra en conflicto con las características de red necesarias para la máquina virtual.*  
  


