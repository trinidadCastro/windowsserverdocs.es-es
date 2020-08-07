---
title: Asegúrese de que hay suficiente espacio en disco físico disponible cuando las máquinas virtuales usan discos duros virtuales de diferenciación
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 9915d01408ec5d51cc70bcdf4682ebca5b4e5c32
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950289"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>Asegúrese de que hay suficiente espacio en disco físico disponible cuando las máquinas virtuales usan discos duros virtuales de diferenciación

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Incidencia
*Una o varias máquinas virtuales están usando discos duros virtuales de diferenciación.*

## <a name="impact"></a>Impacto
*Los discos duros virtuales de diferenciación requieren espacio disponible en el volumen de hospedaje para que se pueda asignar espacio cuando se produzcan escrituras en los discos duros virtuales. Si se agota el espacio disponible, cualquier máquina virtual que se base en el almacenamiento físico podría verse afectada. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Resolución
*Supervise el espacio disponible en disco para asegurarse de que hay suficiente espacio disponible para la expansión del disco duro virtual. Considere la posibilidad de combinar discos duros virtuales de diferenciación en sus elementos primarios. En el administrador de Hyper-V, inspeccione el disco de diferenciación para determinar el disco duro virtual principal. Si fusiona mediante combinación un disco de diferenciación en un disco primario compartido por otros discos de diferenciación, la acción dañará la relación entre los demás discos de diferenciación y el disco principal, de forma que no se puedan usar. Después de comprobar que el disco duro virtual primario no está compartido, puede usar el Asistente para editar discos para fusionar mediante combinación el disco de diferenciación con el disco duro virtual principal.*



