---
title: Evitar incoherencias de alineación entre los bloques virtuales y los sectores del disco físico en discos duros virtuales dinámicos o en discos de diferenciación
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f77d80db1e2454eb460043cacef632e979fc16de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366495"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>Evitar incoherencias de alineación entre los bloques virtuales y los sectores del disco físico en discos duros virtuales dinámicos o en discos de diferenciación

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Se detectaron incoherencias de alineación en uno o varios discos duros virtuales.*  
  
### <a name="impact"></a>Impacto  
*Si los discos duros virtuales se almacenan en un disco físico que tiene un tamaño de sector de 4K, es posible que la máquina virtual o las aplicaciones que usan el disco duro virtual experimenten problemas de rendimiento. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use el Asistente para crear discos duros virtuales para crear un nuevo disco duro virtual con formato VHD o VHDX y especifique el disco duro virtual existente como disco de origen. El nuevo disco duro virtual se creará con la alineación entre los bloques virtuales y el disco físico.*  
  


