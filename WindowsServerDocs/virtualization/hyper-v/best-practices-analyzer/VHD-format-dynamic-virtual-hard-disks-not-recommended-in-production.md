---
title: 'Formato de disco duro virtual: discos duros virtuales dinámicos no se recomiendan para las máquinas virtuales que ejecutan cargas de trabajo de servidor en un entorno de producción'
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 324a60a0-1d15-4ef2-9f17-23cbd2eb42ce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6acd27fa0efa27ba74c28e290c789edca599f66f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849886"
---
# <a name="vhd-format-dynamic-virtual-hard-disks-are-not-recommended-for-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Formato de disco duro virtual: discos duros virtuales dinámicos no se recomiendan para las máquinas virtuales que ejecutan cargas de trabajo de servidor en un entorno de producción

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
*Una o más máquinas virtuales usar discos duros virtuales de expansión dinámica en el formato de VHD.*  
  
## <a name="impact"></a>**Impact**  
*Formato de disco duro virtual: discos duros virtuales dinámicos podría experimentar problemas de coherencia si se produce un error de alimentación. Problemas de coherencia pueden ocurrir si el disco físico realiza una actualización incompleta o incorrecta de un sector en un archivo .vhd que se modifica cuando se produce un error de alimentación. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Apague la máquina virtual y convertir el formato de disco duro virtual dinámico disco duro en un disco duro virtual de formato VHDX o a un disco duro virtual fijo. (El formato VHDX tiene mecanismos de confiabilidad que ayudan a impedir daños debidos a errores de alimentación del sistema en el disco). Sin embargo, convertir el disco duro virtual si es probable que se adjuntará a una versión anterior de Windows en algún momento. Versiones anteriores a Windows Server 2012 no admiten el formato VHDX de Windows.*  
  


