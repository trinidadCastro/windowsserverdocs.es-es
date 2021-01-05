---
title: Evitar la habilitación de la calidad de servicio de almacenamiento cuando se usa un disco duro virtual de diferenciación cuando los discos duros virtuales principales y secundarios están en volúmenes diferentes
description: Obtenga información acerca de qué hacer cuando un disco duro virtual de diferenciación con los discos duros virtuales principales y secundarios en diferentes volúmenes tiene habilitada la calidad de servicio de almacenamiento.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
ms.date: 8/16/2016
ms.openlocfilehash: b02cb69d62e9cac74f42f68b1d4be136e1d7cd29
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834680"
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



