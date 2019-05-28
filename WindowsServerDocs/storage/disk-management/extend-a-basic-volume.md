---
title: Extender un volumen básico
description: En este artículo se describe cómo agregar espacio en unidades principales y lógicas para extender un volumen básico
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c20e2da3e629743ab4d4d4cf1da16a6e69093ecf
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192583"
---
# <a name="extend-a-basic-volume"></a>Extender un volumen básico

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes agregar más espacio a las particiones existentes principales y unidades lógicas ampliándolas con espacio sin asignar adyacente en el mismo disco. Para ampliar un volumen básico, debe ser un volumen sin procesar (no formateado con un sistema de archivos) o formateado con el sistema de archivos NTFS. Puedes ampliar una unidad lógica en el espacio libre contiguo en la partición extendida que lo contiene. Si extiendes a una unidad lógica más allá del espacio libre disponible en la partición extendida, la partición extendida aumenta para incluir la unidad lógica.

En lo que respecta a unidades lógicas y volúmenes de arranque o de sistema, puedes extender el volumen solo en un espacio contiguo y solo si el disco puede actualizarse a un disco dinámico. En lo que respecta a otros volúmenes, puedes extender el volumen en un espacio no contiguo, pero se te pedirá que conviertas el disco en dinámico.

## <a name="extending-a-basic-volume"></a>Extender un volumen básico

-   [Mediante la interfaz de Windows](#to-extend-a-basic-volume-using-the-windows-interface)
-   [Línea de comandos](#to-extend-a-basic-volume-using-a-command-line)

#### <a name="to-extend-a-basic-volume-using-the-windows-interface"></a>Para extender un volumen básico mediante la interfaz de Windows

1. En el Administrador de discos, haz clic con el botón derecho en el volumen básico que quieres extender.

2. Haz clic en **Extender volumen**.

3. Sigue las instrucciones en pantalla.

#### <a name="to-extend-a-basic-volume-using-a-command-line"></a>Para extender un volumen básico mediante la línea de comandos

1. Abra un símbolo del sistema y escriba `diskpart`.

2. En el símbolo del sistema **DISKPART**, escribe `list volume`. Anota el volumen básico que quieres extender.

3. En el símbolo del sistema **DISKPART**, escribe `select volume <volumenumber>`. Esta acción selecciona el volumen básico *volumenumber* que quieres extender en un espacio contiguo y vacío del mismo disco.

4. En el símbolo del sistema **DISKPART**, escribe `extend [size=<size>]`. Esto extiende el volumen seleccionado por *tamaño* en megabytes (MB).

| Valor | Descripción |
| --- | --- |
| <p>**volumen de la lista**</p> | <p>Muestra una lista de volúmenes básicos y dinámicos en todos los discos.</p> |
| <p>**Seleccione el volumen**</p> | <p>Selecciona el volumen especificado, donde <em>volumenumber</em> es el número de volumen y el que recibe el foco. Si no se especifica ningún volumen, el comando **select** muestra el volumen actual con el foco. Puedes especificar el volumen por número, letra de unidad o ruta de acceso de punto de montaje. En un disco básico, si seleccionas un volumen, este también recibe el foco de partición correspondiente.</p> |
| <p>**extend**</p> | <p><ul><li>Extiende el volumen con el foco en el espacio contiguo sin asignar. En cuanto los volúmenes básicos, el espacio sin asignar debe estar en el mismo disco que la partición con el foco, así como seguirla (o estar en un desplazamiento de sector superior). Un volumen simple o distribuido dinámico puede extenderse a cualquier espacio vacío de un disco dinámico. Al usar este comando, puedes extender un volumen existente en un espacio recientemente creado.</p></li ><p><li>Si la partición se ha formateado previamente con el sistema de archivos NTFS, el sistema de archivos se extiende automáticamente para ocupar la partición de mayor tamaño. No se produce ninguna pérdida de datos. Si la partición se ha formateado anteriormente con cualquier formato de sistema de archivos que no sea NTFS, se producirá un error en el comando sin cambios en la partición.</p></li></ul>|
| <p>**size=** <em>size</em></p> | <p>La cantidad de espacio, en megabytes (MB), para agregar a la partición actual. Si no se especifica un tamaño, el disco se extiende para ocupar todo el espacio contiguo sin asignar.</p> |

## <a name="additional-considerations"></a>Consideraciones adicionales

-   Si el disco no incluye particiones de arranque o de sistema, puedes extender el volumen en otros discos sin arranque o sin sistema, pero el disco se convertirá en un disco dinámico (si se puede actualizar).

## <a name="see-also"></a>Vea también

-   [Notación de sintaxis de línea de comandos](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)
