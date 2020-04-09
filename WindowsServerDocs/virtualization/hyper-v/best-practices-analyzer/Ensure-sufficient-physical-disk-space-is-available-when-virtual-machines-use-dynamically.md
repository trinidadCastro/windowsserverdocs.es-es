---
title: Asegúrese de que hay suficiente espacio en disco físico disponible cuando las máquinas virtuales usan discos duros virtuales de expansión dinámica
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 53302d9e8fc4f960f0a1744d4ebd274b936eac5f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861948"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>Asegúrese de que hay suficiente espacio en disco físico disponible cuando las máquinas virtuales usan discos duros virtuales de expansión dinámica

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Una o varias máquinas virtuales están usando discos duros virtuales de expansión dinámica.*  
  
## <a name="impact"></a>Impacto  
*Los discos duros virtuales de expansión dinámica requieren espacio disponible en el volumen de hospedaje para que se pueda asignar espacio cuando se produzcan escrituras en los discos duros virtuales. Si se agota el espacio disponible, cualquier máquina virtual que se base en el almacenamiento físico podría verse afectada. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Supervise el espacio disponible en disco para asegurarse de que hay suficiente espacio disponible para la expansión. Considere la posibilidad de apagar la máquina virtual y usar el Asistente para editar discos en el administrador de Hyper-V para convertir cada disco duro virtual de expansión dinámica para esta máquina virtual en un disco duro virtual de tamaño fijo.*  
  


