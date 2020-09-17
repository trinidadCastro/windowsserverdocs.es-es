---
title: Evitar pausar una máquina virtual
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
ms.date: 8/16/2016
ms.openlocfilehash: 956ada130cea2d489545c8206ae8283041e6868f
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747040"
---
# <a name="avoid-pausing-a-virtual-machine"></a>Evitar pausar una máquina virtual

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

*Este servidor tiene una o más máquinas virtuales en un estado de pausa.*

## <a name="impact"></a>Impacto

*En función de la cantidad de memoria disponible, es posible que no pueda ejecutar máquinas virtuales adicionales.*

Las máquinas virtuales en pausa no liberan la memoria asignada, lo que significa que la memoria no está disponible para iniciar otras máquinas virtuales.

## <a name="resolution"></a>Solución

*Si esto es intencionado, no es necesario realizar ninguna otra acción. De lo contrario, considere la posibilidad de reanudar estas máquinas virtuales o apagarlas.*

#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>Usar el administrador de Hyper-V para reanudar la máquina virtual

1.  Abra el administrador de Hyper-V. (En el menú **herramientas** de administrador del servidor, haga clic en **Administrador de Hyper-V**).

2.  En la lista de **virtual machines** , busque las máquinas virtuales con el estado en **pausa**.

    > [!IMPORTANT]
    > Un estado de **pausa: crítico** se produce cuando hay muy poco espacio libre en el almacenamiento físico de esa máquina virtual. Antes de intentar reanudar una máquina virtual en este estado, libere espacio disponible en el almacenamiento físico.

3.  Haga clic con el botón secundario en cada nombre de máquina virtual y haga clic en **reanudar**. Esto devuelve la máquina virtual a un estado de ejecución. Después, si desea apagar la máquina virtual, vuelva a hacer clic con el botón secundario y elija **apagar**.

#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>Usar Windows PowerShell para reanudar la máquina virtual

Puede hacerlo en un comando mediante el filtrado y la canalización después de obtener todas las máquinas virtuales del host. Escriba:

```
get-vm | where state -eq 'paused' | resume-vm
```



