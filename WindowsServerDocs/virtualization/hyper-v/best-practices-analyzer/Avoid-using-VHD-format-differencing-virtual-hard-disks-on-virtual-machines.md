---
title: Evite el uso de discos duros virtuales de diferenciación con formato VHD en máquinas virtuales que ejecutan cargas de trabajo de servidor en un entorno de producción.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a11959266db4c9f3da73123c41a211198f27b9a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857728"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Evite el uso de discos duros virtuales de diferenciación con formato VHD en máquinas virtuales que ejecutan cargas de trabajo de servidor en un entorno de producción.

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*Una o varias máquinas virtuales usan discos duros virtuales de diferenciación con formato VHD.*  
  
## <a name="impact"></a>**Impacto**  
*Los discos duros virtuales de diferenciación con formato VHD podrían experimentar problemas de coherencia si se produce un error de alimentación. Pueden producirse problemas de coherencia si el disco físico realiza una actualización incompleta o incorrecta en un sector de un archivo. vhd que se está modificando cuando se produce un error de alimentación. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Apague la máquina virtual y convierta la cadena de discos duros virtuales de diferenciación con formato VHD al formato VHDX o mezcle la cadena en un disco duro virtual fijo. (El formato VHDX tiene mecanismos de confiabilidad que ayudan a proteger el disco de daños debidos a errores de alimentación). Sin embargo, no convierta el disco duro virtual si es probable que se adjunte a una versión anterior de Windows en algún momento. Las versiones de Windows anteriores a Windows Server 2012 no admiten el formato VHDX.*  
  


