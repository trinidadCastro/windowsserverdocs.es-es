---
title: El número de máquinas virtuales en ejecución configuradas para SR-IOV no debe superar el número de funciones virtuales disponibles para las máquinas virtuales
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8bd4af5e-9e7d-4710-8950-39435a8bb373
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 3c58e980471284c950f41551b02b92bf74aca7cc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854618"
---
# <a name="the-number-of-running-virtual-machines-configured-for-sr-iov-should-not-exceed-the-number-of-virtual-functions-available-to-the-virtual-machines"></a>El número de máquinas virtuales en ejecución configuradas para SR-IOV no debe superar el número de funciones virtuales disponibles para las máquinas virtuales

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
*No hay suficientes funciones virtuales disponibles para el número de máquinas virtuales en ejecución configuradas para la virtualización de e/s de raíz única (SR-IOV).*  
  
## <a name="impact"></a>Impacto  
*Es posible que el rendimiento de red no sea óptimo en las siguientes máquinas virtuales:*  
   
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Considere la posibilidad de deshabilitar SR-IOV en una o varias máquinas virtuales que no requieran una función virtual de SR-IOV.*  
  


