---
title: Se debe realizar una copia de seguridad de las máquinas virtuales al menos una vez cada semana
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ec11b067de2c9f8cbb3a17731caa0dc526bf54a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393238"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>Se debe realizar una copia de seguridad de las máquinas virtuales al menos una vez cada semana

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*No se ha realizado una copia de seguridad de una o más máquinas virtuales en la última semana.*  
  
## <a name="impact"></a>Impacto  
*Podría producirse una pérdida de datos significativa si la máquina virtual encuentra un problema y no existe una copia de seguridad reciente. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Programe una copia de seguridad de las máquinas virtuales para que se ejecute al menos una vez a la semana. Puede omitir esta regla si esta máquina virtual es una réplica y se está realizando una copia de seguridad de su máquina virtual principal, o si se trata de la máquina virtual principal y se está realizando una copia de seguridad de su réplica.*  
  


