---
title: Evitar la habilitación de la calidad de servicio de almacenamiento cuando se usa un disco duro virtual de diferenciación cuando los discos duros virtuales principales y secundarios están en volúmenes diferentes
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: b208a7a10679804666ff41a02d4cddbb4d8c6762
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939187"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Evitar la habilitación de la calidad de servicio de almacenamiento cuando se usa un disco duro virtual de diferenciación cuando los discos duros virtuales principales y secundarios están en volúmenes diferentes

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*Un disco duro virtual de diferenciación con los discos duros virtuales principales y secundarios en diferentes volúmenes tiene habilitada la calidad de servicio de almacenamiento.*

## <a name="impact"></a>**Impacto**
*Esta configuración puede provocar un comportamiento inesperado de la calidad del servicio para el disco duro virtual de diferenciación, así como otros discos duros virtuales en los volúmenes primarios y secundarios. Esto afecta a los siguientes discos duros virtuales:*

\<list of virtual hard disks>

## <a name="resolution"></a>**Resolución**
*Deshabilite la calidad de servicio de almacenamiento en los discos duros virtuales a los que se hace referencia o realice una migración de almacenamiento para trasladar el disco duro virtual primario y secundario al mismo volumen.*



