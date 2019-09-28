---
title: El número de máquinas virtuales en ejecución configuradas para SR-IOV no debe superar el número de funciones virtuales disponibles para las máquinas virtuales
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0887035c84ebc4b7d93163533387f2f8ab20fb87
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364614"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>El número de máquinas virtuales en ejecución configuradas para SR-IOV no debe superar el número de funciones virtuales disponibles para las máquinas virtuales

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*No hay suficientes funciones virtuales disponibles para el número de máquinas virtuales en ejecución configuradas para la virtualización de e/s de raíz única (SR-IOV).*  
  
## <a name="impact"></a>Impacto  
*Es posible que el rendimiento de red no sea óptimo en las siguientes máquinas virtuales:*  
   
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Considere la posibilidad de deshabilitar SR-IOV en una o varias máquinas virtuales que no requieran una función virtual de SR-IOV.*  
  


