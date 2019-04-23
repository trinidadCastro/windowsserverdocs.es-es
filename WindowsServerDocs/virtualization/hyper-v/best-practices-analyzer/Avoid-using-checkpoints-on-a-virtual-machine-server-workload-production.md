---
title: Evite el uso de puntos de control en una máquina virtual que se ejecuta una carga de trabajo de servidor en un entorno de producción
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 166ef839a40452cc4156144e10e9c666e7ce3472
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856156"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>Evite el uso de puntos de control en una máquina virtual que se ejecuta una carga de trabajo de servidor en un entorno de producción

>Se aplica a: Windows Server 2016


  
*Para obtener más información sobre análisis y los procedimientos recomendados, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  

En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.

> [!NOTE]  
> En Windows Server 2012 R2, las instantáneas de máquina virtual se cambió el nombre a los puntos de control de máquina virtual en el Administrador de Hyper-V para que coincida con la terminología usada en la administración de System Center Virtual Machine. Para obtener más información, consulte [Checkpoints and Snapshots Overview](https://technet.microsoft.com/library/dn818483.aspx).  
  
## <a name="issue"></a>Problema  
  
*Se ha encontrado una máquina virtual con uno o varios puntos de control.*  
  
## <a name="impact"></a>Impacto  
  
*Espacio disponible puede agotarse en el disco físico que almacena los archivos de puntos de control. Si esto ocurre, puede realizarse ninguna operación de disco adicional en el almacenamiento físico. Cualquier máquina virtual que se basa en el almacenamiento físico podría verse afectada.*  
  
Si el espacio físico de disco expira, puede detenerse automáticamente cualquier máquina virtual en ejecución que tiene puntos de control o discos duros virtuales almacenados en el disco. Administrador de Hyper-V muestra el estado de estas máquinas virtuales como "pausado-crítico".  
  
## <a name="resolution"></a>Resolución  
  
*Si la máquina virtual se ejecuta una carga de trabajo de servidor en un entorno de producción, desconecte la máquina virtual y, a continuación, utilice el Administrador de Hyper-V para aplicar o eliminar los puntos de control. Para eliminar los puntos de control, debe apagar la máquina virtual para completar el proceso.*  
  
> [!NOTE]  
> Los puntos de control de producción ahora están disponibles como una alternativa a los puntos de control estándares. Para obtener más información, consulte [elegir entre los puntos de control estándares o de producción](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  


