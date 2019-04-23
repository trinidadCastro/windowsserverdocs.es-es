---
title: El número de la ejecución de máquinas virtuales configuradas para SR-IOV no debe superar el número de funciones virtuales disponibles para las máquinas virtuales
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e63df9283927437f9cfc62c052d83b07fe599b34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829756"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>El número de la ejecución de máquinas virtuales configuradas para SR-IOV no debe superar el número de funciones virtuales disponibles para las máquinas virtuales

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
*No hay funciones virtuales suficiente disponible para el número de máquinas virtuales configuradas para la virtualización de E/S de raíz única (SR-IOV) en ejecución.*  
  
## <a name="impact"></a>Impacto  
*Rendimiento de red podría no ser óptima en las máquinas virtuales siguientes:*  
   
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Considere la posibilidad de deshabilitar SR-IOV en uno o más máquinas virtuales que no requieren una función virtual SR-IOV.*  
  


