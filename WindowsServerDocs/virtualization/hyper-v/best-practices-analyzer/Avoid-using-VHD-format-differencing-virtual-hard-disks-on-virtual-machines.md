---
title: Evite el uso de discos duros virtuales de diferenciación con formato VHD en máquinas virtuales que ejecutan cargas de trabajo de servidor en un entorno de producción.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 7b6bee685a72f8f9af2e16ffe7ac5cc1e1f22a4f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366433"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Evite el uso de discos duros virtuales de diferenciación con formato VHD en máquinas virtuales que ejecutan cargas de trabajo de servidor en un entorno de producción.

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*Una o varias máquinas virtuales usan discos duros virtuales de diferenciación con formato VHD.*  
  
## <a name="impact"></a>**Impacto**  
@no__t los discos duros virtuales de diferenciación con formato podrían experimentar problemas de coherencia si se produce un error de alimentación. Pueden producirse problemas de coherencia si el disco físico realiza una actualización incompleta o incorrecta en un sector de un archivo. vhd que se está modificando cuando se produce un error de alimentación. Esto afecta a las siguientes máquinas virtuales: *  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Shut en la máquina virtual y convierta la cadena de discos duros virtuales de diferenciación con formato VHD al formato VHDX o mezcle la cadena en un disco duro virtual fijo. (El formato VHDX tiene mecanismos de confiabilidad que ayudan a proteger el disco de daños debidos a errores de alimentación). Sin embargo, no convierta el disco duro virtual si es probable que se adjunte a una versión anterior de Windows en algún momento. Las versiones de Windows anteriores a Windows Server 2012 no admiten el formato VHDX.*  
  


