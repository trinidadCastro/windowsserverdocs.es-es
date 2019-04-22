---
title: Evitar que se pause una máquina virtual
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4492ac385a289d075ebcd48b1c7c1c78c1af2f8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814356"
---
# <a name="avoid-pausing-a-virtual-machine"></a>Evitar que se pause una máquina virtual

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
  
*Este servidor tiene una o más máquinas virtuales en un estado de pausa.*  
  
## <a name="impact"></a>Impacto  
  
*Según la cantidad de memoria disponible, es posible que no pueda ejecutar máquinas virtuales adicionales.*  
  
Máquinas virtuales en pausa no lance su memoria asignada, lo que significa que la memoria no está disponible para iniciar otras máquinas virtuales.  
  
## <a name="resolution"></a>Resolución  
  
*Si esto es intencionado, se requiere ninguna acción adicional. De lo contrario, considere la posibilidad de reanudar estas máquinas virtuales o apagarlos.*  
  
#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>Use el Administrador de Hyper-V para reanudar la máquina virtual  
  
1.  Abre el Administrador Hyper-V. (Desde el **herramientas** menú del administrador del servidor, haga clic en **Administrador de Hyper-V**.)  
  
2.  Desde el **máquinas virtuales** lista, busque las máquinas virtuales con el estado de **en pausa**.  
  
    > [!IMPORTANT]  
    > Un estado de **en pausa crítica** se produce cuando hay poco espacio libre restante en el almacenamiento físico de esa máquina virtual. Antes de intentar reanudar una máquina virtual en este estado, libere espacio disponible en el almacenamiento físico.  
  
3.  Haga clic en cada nombre de máquina virtual, a continuación, haga clic en **reanudar**. Esto devuelve la máquina virtual a un estado de ejecución. Después de eso, si desea apagar la máquina virtual, haga clic en nuevo y elija **apagar**.  
  
#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>Usar Windows PowerShell para reanudar la máquina virtual  
  
Puede hacerlo en un único comando mediante el filtrado y la canalización después de obtener todas las máquinas virtuales del host. Tipo:  
  
```  
get-vm | where state -eq 'paused' | resume-vm  
```  
  


