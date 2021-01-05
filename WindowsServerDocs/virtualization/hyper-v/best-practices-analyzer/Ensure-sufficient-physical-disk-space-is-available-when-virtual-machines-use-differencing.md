---
title: Asegúrese de que hay suficiente espacio en disco físico disponible cuando las máquinas virtuales usan discos duros virtuales de diferenciación
description: Obtenga información acerca de qué hacer cuando una o varias máquinas virtuales usan discos duros virtuales de diferenciación.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
ms.date: 8/16/2016
ms.openlocfilehash: fe4cf45a9b93ce0a5277dcd6e108205a74ffca0b
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846102"
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

## <a name="issue"></a>Problema
*Una o varias máquinas virtuales están usando discos duros virtuales de diferenciación.*

## <a name="impact"></a>Impacto
*Los discos duros virtuales de diferenciación requieren espacio disponible en el volumen de hospedaje para que se pueda asignar espacio cuando se produzcan escrituras en los discos duros virtuales. Si se agota el espacio disponible, cualquier máquina virtual que se base en el almacenamiento físico podría verse afectada. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Supervise el espacio disponible en disco para asegurarse de que hay suficiente espacio disponible para la expansión del disco duro virtual. Considere la posibilidad de combinar discos duros virtuales de diferenciación en sus elementos primarios. En el administrador de Hyper-V, inspeccione el disco de diferenciación para determinar el disco duro virtual principal. Si fusiona mediante combinación un disco de diferenciación en un disco primario compartido por otros discos de diferenciación, la acción dañará la relación entre los demás discos de diferenciación y el disco principal, de forma que no se puedan usar. Después de comprobar que el disco duro virtual primario no está compartido, puede usar el Asistente para editar discos para fusionar mediante combinación el disco de diferenciación con el disco duro virtual principal.*



