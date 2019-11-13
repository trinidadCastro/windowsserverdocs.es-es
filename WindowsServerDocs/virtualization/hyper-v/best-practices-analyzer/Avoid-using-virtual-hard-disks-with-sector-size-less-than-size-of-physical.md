---
title: Evite el uso de discos duros virtuales con un tamaño de sector menor que el tamaño de sector del almacenamiento físico que almacena el archivo de disco duro virtual.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 944fdce68a2f0b8e9c122f5f9134f0e07de18bbd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366406"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>Evite el uso de discos duros virtuales con un tamaño de sector menor que el tamaño de sector del almacenamiento físico que almacena el archivo de disco duro virtual.

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Operativo** <br />**Integrado**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*Uno o más discos duros virtuales tienen un tamaño de sector físico menor que el tamaño de sector físico del almacenamiento en el que se encuentra el archivo de disco duro virtual.*  
  
## <a name="impact"></a>**Impacto**  
*Pueden producirse problemas de rendimiento en la máquina virtual o la aplicación que usa el disco duro virtual. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Realice una de las acciones siguientes:*  
  
-   *Realizar una migración de almacenamiento para trasladar el disco duro virtual a un nuevo sistema físico*  
  
-   *Usar Windows PowerShell o WMI para habilitar un disco duro virtual con formato VHDX para informar de un tamaño de sector específico*  
  
-   *Usar una configuración del registro para permitir que un disco duro virtual con formato VHD informe de un tamaño de sector físico de 4k*  
  


