---
title: Evite el uso de discos duros virtuales con un tamaño de sector menor que el tamaño de sector del almacenamiento físico que almacena el archivo de disco duro virtual
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b6ec2e0995180ecf9ae5986447fdd460a1c7a337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846266"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>Evite el uso de discos duros virtuales con un tamaño de sector menor que el tamaño de sector del almacenamiento físico que almacena el archivo de disco duro virtual

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Operating** <br />**Sistema**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>**Problema**  
*Uno o varios discos duros virtuales tienen un tamaño de sector físico que es menor que el tamaño de sector físico del almacenamiento en el que se encuentra el archivo de disco duro virtual.*  
  
## <a name="impact"></a>**Impact**  
*Pueden producirse problemas de rendimiento en la máquina virtual o aplicación que utilice el disco duro virtual. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Realice una de las siguientes acciones:*  
  
-   *Realizar una migración de almacenamiento para mover el disco duro virtual a un nuevo sistema físico*  
  
-   *Usar Windows PowerShell o WMI para habilitar un disco duro virtual de formato VHDX informar de un tamaño de sector específico*  
  
-   *Usar una configuración del registro para habilitar un disco duro virtual de formato de VHD informar de un tamaño de sector físico de 4k*  
  


