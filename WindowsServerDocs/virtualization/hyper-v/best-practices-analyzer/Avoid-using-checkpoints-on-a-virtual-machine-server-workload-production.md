---
title: Evitar el uso de puntos de control en una máquina virtual que ejecuta una carga de trabajo de servidor en un entorno de producción
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
ms.date: 8/16/2016
ms.openlocfilehash: da0315ab06d4678d3cedc51092be7160d301ac4d
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747010"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>Evitar el uso de puntos de control en una máquina virtual que ejecuta una carga de trabajo de servidor en un entorno de producción

>Se aplica a: Windows Server 2016



*Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Operations|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

> [!NOTE]
> En Windows Server 2012 R2, se ha cambiado el nombre de las instantáneas de máquina virtual a los puntos de control de la máquina virtual en el administrador de Hyper-V para que coincida con la terminología que se usa en la administración de máquinas virtuales de System Center. Para obtener más información, consulte [Introducción a los puntos de comprobación e instantáneas](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn818483(v=ws.11)).

## <a name="issue"></a>Problema

*Se ha encontrado una máquina virtual con uno o varios puntos de control.*

## <a name="impact"></a>Impacto

*Se puede agotar el espacio disponible en el disco físico en el que se almacenan los archivos de puntos de control. Si esto ocurre, no se pueden realizar más operaciones de disco en el almacenamiento físico. Cualquier máquina virtual que se base en el almacenamiento físico podría verse afectada.*

Si se agota el espacio en disco físico, cualquier máquina virtual en ejecución que tenga puntos de control o discos duros virtuales almacenados en ese disco puede pausarse automáticamente. El administrador de Hyper-V muestra el estado de estas máquinas virtuales como crítico en pausa.

## <a name="resolution"></a>Solución

*Si la máquina virtual ejecuta una carga de trabajo de servidor en un entorno de producción, ponga la máquina virtual sin conexión y, a continuación, use el administrador de Hyper-V para aplicar o eliminar los puntos de control. Para eliminar puntos de control, debe apagar la máquina virtual para completar el proceso.*

> [!NOTE]
> Los puntos de control de producción ahora están disponibles como alternativa a los puntos de control estándar. Para obtener más información, consulte [elegir entre los puntos de control estándar o de producción](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).