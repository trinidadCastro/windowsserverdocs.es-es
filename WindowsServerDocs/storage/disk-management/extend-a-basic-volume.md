---
title: Extensión de un volumen básico
description: En este artículo se describe cómo agregar espacio a unidades principales y lógicas para extender un volumen básico
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a98bd3553c3223716d70ed4329bd7e265e697b73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402100"
---
# <a name="extend-a-basic-volume"></a>Extensión de un volumen básico

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Para agregar más espacio a las particiones principales y unidades lógicas existentes amplíalas al espacio sin asignar adyacente que hay en el mismo disco. Para ampliar un volumen básico, debe ser un volumen sin procesar (sin dar formato con un sistema de archivos) o formateado con el sistema de archivos NTFS. Puedes ampliar una unidad lógica en el espacio disponible contiguo de la partición extendida que lo contiene. Si extiendes una unidad lógica más allá del espacio disponible en la partición extendida, esta crece para incluir la unidad lógica.

En lo que respecta a unidades lógicas y volúmenes de arranque o del sistema, puedes extender el volumen solo al espacio contiguo y solo si el disco puede actualizarse a un disco dinámico. En el caso de otros volúmenes, puedes extender el volumen a un espacio no contiguo, pero se te solicitará que conviertas el disco en dinámico.

## <a name="extending-a-basic-volume"></a>Extensión de un volumen básico

#### <a name="to-extend-a-basic-volume-using-the-windows-interface"></a>Para extender un volumen básico mediante la interfaz de Windows

1. En el Administrador de discos, haz clic con el botón derecho en el volumen básico que quieres extender.

2. Haz clic en **Extender volumen**.

3. Sigue las instrucciones en pantalla.

#### <a name="to-extend-a-basic-volume-using-a-command-line"></a>Para extender un volumen básico desde la línea de comandos

1. Abra un símbolo del sistema y escriba `diskpart`.

2. En el símbolo del sistema **DISKPART**, escribe `list volume`. Anota el volumen básico que quieres extender.

3. En el símbolo del sistema **DISKPART**, escribe `select volume <volumenumber>`. Esta acción selecciona el volumen básico *volumenumber* que quieres extender a un espacio contiguo y vacío del mismo disco.

4. En el símbolo del sistema **DISKPART**, escribe `extend [size=<size>]`. Así se extiende el volumen seleccionado por *tamaño* en megabytes (MB).

| Valor | Descripción |
| --- | --- |
| **list volume** | Muestra una lista de volúmenes básicos y dinámicos en todos los discos. |
| **select volume** | Selecciona el volumen especificado, donde <em>volumenumber</em> es el número de volumen y el que recibe el foco. Si no se especifica ningún volumen, el comando **select** muestra el volumen actual con el foco. Puedes especificar el volumen por número, letra de unidad o ruta de acceso de punto de montaje. En un disco básico, si seleccionas un volumen, este también recibe el foco de partición correspondiente. |
| **extend** | <ul><li>Extiende el volumen con el foco al espacio contiguo sin asignar. En el caso de los volúmenes básicos, el espacio sin asignar debe estar en el mismo disco que la partición con el foco, es más, debe ir detrás de ella (o estar en un desplazamiento de sector superior). Un volumen simple o distribuido dinámico puede extenderse a cualquier espacio vacío de un disco dinámico. Mediante este comando puedes extender un volumen existente a un espacio recién creado.</li ><li>Si la partición se ha formateado previamente con el sistema de archivos NTFS, el sistema de archivos se extiende automáticamente para ocupar la partición de mayor tamaño. No se produce ninguna pérdida de datos. Si la partición se ha formateado anteriormente con cualquier formato de sistema de archivos que no sea NTFS, se producirá un error en el comando y no de realizarán cambios en la partición.</li></ul> |
| **size=** <em>size</em> | La cantidad de espacio, en megabytes (MB), para agregar a la partición actual. Si no se especifica ningún tamaño, el disco se extiende para ocupar todo el espacio contiguo sin asignar. |

## <a name="additional-considerations"></a>Consideraciones adicionales

-   Si el disco no incluye particiones de arranque o del sistema, puedes extender el volumen a otros discos que no sean de arranque ni de sistema, pero el disco se convertirá en un disco dinámico (si se puede actualizar).

## <a name="see-also"></a>Consulta también

-   [Notación de sintaxis de línea de comandos](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)
