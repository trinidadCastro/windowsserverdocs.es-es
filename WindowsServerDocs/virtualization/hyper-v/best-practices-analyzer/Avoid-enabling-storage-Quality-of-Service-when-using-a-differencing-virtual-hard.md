---
title: Evitar la habilitación de la calidad de servicio de almacenamiento cuando se usa un disco duro virtual de diferenciación cuando los discos duros virtuales principales y secundarios están en volúmenes diferentes
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 716a32de2f9327e5eca38c470fa1b7c44150e9cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366440"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Evitar la habilitación de la calidad de servicio de almacenamiento cuando se usa un disco duro virtual de diferenciación cuando los discos duros virtuales principales y secundarios están en volúmenes diferentes

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
*Un disco duro virtual de diferenciación con los discos duros virtuales principales y secundarios en diferentes volúmenes tiene habilitada la calidad de servicio de almacenamiento.*  
  
## <a name="impact"></a>**Impacto**  
la configuración de *This puede producir un comportamiento inesperado de la calidad del servicio para el disco duro virtual de diferenciación, así como otros discos duros virtuales de los volúmenes primarios y secundarios. Esto afecta a los siguientes discos duros virtuales:*  
  
\<list de los discos duros virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Deshabilite la calidad de servicio de almacenamiento en los discos duros virtuales a los que se hace referencia o realice una migración de almacenamiento para trasladar el disco duro virtual primario y secundario al mismo volumen.*  
  


