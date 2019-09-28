---
title: Evitar el uso de puntos de control en una máquina virtual que ejecuta una carga de trabajo de servidor en un entorno de producción
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f2486093e31143b7493665d3d1254f7034ad1415
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365221"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>Evitar el uso de puntos de control en una máquina virtual que ejecuta una carga de trabajo de servidor en un entorno de producción

>Se aplica a: Windows Server 2016


  
*Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

> [!NOTE]  
> En Windows Server 2012 R2, se ha cambiado el nombre de las instantáneas de máquina virtual a los puntos de control de la máquina virtual en el administrador de Hyper-V para que coincida con la terminología que se usa en la administración de máquinas virtuales de System Center. Para obtener más información, consulte [Introducción a los puntos de comprobación e instantáneas](https://technet.microsoft.com/library/dn818483.aspx).  
  
## <a name="issue"></a>Problema  
  
*Se ha encontrado una máquina virtual con uno o varios puntos de control.*  
  
## <a name="impact"></a>Impacto  
  
@no__t espacio 0Available puede quedarse en el disco físico que almacena los archivos de puntos de control. Si esto ocurre, no se pueden realizar más operaciones de disco en el almacenamiento físico. Cualquier máquina virtual que se base en el almacenamiento físico podría verse afectada. *  
  
Si se agota el espacio en disco físico, cualquier máquina virtual en ejecución que tenga puntos de control o discos duros virtuales almacenados en ese disco puede pausarse automáticamente. El administrador de Hyper-V muestra el estado de estas máquinas virtuales como "en pausa-crítico".  
  
## <a name="resolution"></a>Resolución  
  
*If la máquina virtual ejecuta una carga de trabajo de servidor en un entorno de producción, desconecta la máquina virtual y, a continuación, usa el administrador de Hyper-V para aplicar o eliminar los puntos de control. Para eliminar puntos de control, debe apagar la máquina virtual para completar el proceso.*  
  
> [!NOTE]  
> Los puntos de control de producción ahora están disponibles como alternativa a los puntos de control estándar. Para obtener más información, consulte [elegir entre los puntos de control estándar o de producción](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  


