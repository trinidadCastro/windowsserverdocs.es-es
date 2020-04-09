---
title: Usar todas las funciones virtuales para redes cuando estén disponibles
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0fab06ae21a4632df73b7a4d8b17b12665ffed98
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854218"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>Usar todas las funciones virtuales para redes cuando estén disponibles

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
*No se están usando algunas funciones de aceleración de hardware*  
  
## <a name="impact"></a>Impacto  
*Esta configuración puede hacer que el uso total de la CPU sea más alto de lo necesario. Es posible que el rendimiento de red no sea óptimo en las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Considere la posibilidad de configurar el adaptador de red virtual para SR-IOV si el hardware físico es compatible con SR-IOV y si esta configuración no entra en conflicto con las características de red que necesita la máquina virtual.*  
  


