---
title: Las instantáneas de recuperación se deben quitar después de la conmutación por error
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4b8574956fb1b46ca0cf9678187fffcd68c2d261
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393525"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>Las instantáneas de recuperación se deben quitar después de la conmutación por error

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016| 
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*Una máquina virtual conmutada por error tiene una o varias instantáneas de recuperación.*  
  
## <a name="impact"></a>**Impacto**  
*Se puede agotar el espacio disponible en el disco físico que almacena los archivos de instantáneas. Si esto ocurre, no se pueden realizar más operaciones de disco en el almacenamiento físico. Cualquier máquina virtual que se base en el almacenamiento físico podría verse afectada. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Para cada máquina virtual conmutada por error, use el cmdlet complete-VMFailover en Windows PowerShell para quitar las instantáneas de recuperación e indicar la finalización de la conmutación por error.*  
  


