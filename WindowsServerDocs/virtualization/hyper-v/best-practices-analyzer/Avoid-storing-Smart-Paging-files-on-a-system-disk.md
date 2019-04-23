---
title: Evite almacenar los archivos de paginación inteligente en un disco del sistema
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6abc84b406de7e7c33628ccee4e3af706efe5c70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886176"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Evite almacenar los archivos de paginación inteligente en un disco del sistema

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, la cursiva indica texto que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*La configuración de memoria para una o más máquinas virtuales puede requerir el uso de la paginación inteligente si se reinicia la máquina virtual y la ubicación especificada para el archivo de paginación inteligente es el disco del sistema del servidor que ejecuta Hyper-V.*  
  
## <a name="impact"></a>Impacto  
*Uso del disco del sistema para la paginación inteligente puede provocar que el servidor que ejecuta Hyper-V para experimentar problemas. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Volver a configurar las máquinas virtuales para almacenar los archivos de paginación inteligente en un disco ajeno al sistema.*  
  


