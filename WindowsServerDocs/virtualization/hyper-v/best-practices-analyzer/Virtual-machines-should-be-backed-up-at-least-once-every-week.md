---
title: Las máquinas virtuales se deben copiar al menos una vez cada semana
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e079e3cb225ec9c712233bbf3efc85bb6f09b218
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826756"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>Las máquinas virtuales se deben copiar al menos una vez cada semana

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*Una o más máquinas virtuales no se copia en la semana pasada.*  
  
## <a name="impact"></a>Impacto  
*Pérdida de datos importantes puede producirse si la máquina virtual se encuentra un problema y no existe una copia de seguridad reciente. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Programar una copia de seguridad de las máquinas virtuales para ejecutar al menos una vez por semana. Puede omitir esta regla si esta máquina virtual es una réplica y se está realizando una copia de seguridad de su máquina virtual principal, o si se trata de la máquina virtual principal y se está realizando una copia de seguridad de su réplica.*  
  


