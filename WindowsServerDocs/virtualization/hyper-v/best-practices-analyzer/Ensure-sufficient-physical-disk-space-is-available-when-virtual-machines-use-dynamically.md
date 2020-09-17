---
title: Asegúrese de que hay suficiente espacio en disco físico disponible cuando las máquinas virtuales usan discos duros virtuales de expansión dinámica
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
ms.date: 8/16/2016
ms.openlocfilehash: 1e67b51aa3c41c9642a8972d25e0501b26e91cd5
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746870"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>Asegúrese de que hay suficiente espacio en disco físico disponible cuando las máquinas virtuales usan discos duros virtuales de expansión dinámica

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
*Una o varias máquinas virtuales están usando discos duros virtuales de expansión dinámica.*

## <a name="impact"></a>Impacto
*Los discos duros virtuales de expansión dinámica requieren espacio disponible en el volumen de hospedaje para que se pueda asignar espacio cuando se produzcan escrituras en los discos duros virtuales. Si se agota el espacio disponible, cualquier máquina virtual que se base en el almacenamiento físico podría verse afectada. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Supervise el espacio disponible en disco para asegurarse de que hay suficiente espacio disponible para la expansión. Considere la posibilidad de apagar la máquina virtual y usar el Asistente para editar discos en el administrador de Hyper-V para convertir cada disco duro virtual de expansión dinámica para esta máquina virtual en un disco duro virtual de tamaño fijo.*



