---
title: Evitar incoherencias de alineación entre los bloques virtuales y los sectores del disco físico en discos duros virtuales dinámicos o en discos de diferenciación
description: Obtenga información sobre qué hacer cuando se detecten incoherencias de alineación en uno o varios discos duros virtuales.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
ms.date: 8/16/2016
ms.openlocfilehash: a35906161a51e2884a15944b51416a2404cce38a
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834730"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>Evitar incoherencias de alineación entre los bloques virtuales y los sectores del disco físico en discos duros virtuales dinámicos o en discos de diferenciación

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*Se detectaron incoherencias de alineación en uno o varios discos duros virtuales.*

### <a name="impact"></a>Impacto
*Si los discos duros virtuales se almacenan en un disco físico que tiene un tamaño de sector de 4K, es posible que la máquina virtual o las aplicaciones que usan el disco duro virtual experimenten problemas de rendimiento. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Use el Asistente para crear discos duros virtuales para crear un nuevo disco duro virtual con formato VHD o VHDX y especifique el disco duro virtual existente como disco de origen. El nuevo disco duro virtual se creará con la alineación entre los bloques virtuales y el disco físico.*



