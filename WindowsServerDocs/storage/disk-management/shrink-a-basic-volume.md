---
title: "Reducir un volumen básico"
description: "En este artículo se describe cómo reducir un volumen básico"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e54632b78fd67a65b51147323565130881d8d81b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="shrink-a-basic-volume"></a>Reducir un volumen básico

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Es posible disminuir el espacio que usan las particiones principales y las unidades lógicas reduciéndolas en espacios adyacentes y contiguos del mismo disco. Por ejemplo, si necesitas una partición más pero no dispones de discos adicionales, puedes reducir la partición existente de la parte final del volumen para crear un nuevo espacio sin asignar que puede usarse para una nueva partición. La operación de reducción puede bloquearse por la presencia de determinados tipos de archivos. Para obtener más información, consulta [Consideraciones adicionales](#addcon) 

Al reducir una partición, todos los archivos normales se reubican automáticamente en el disco para crear el nuevo espacio sin asignar. No es necesario volver a formatear el disco para reducir la partición.

> [!CAUTION]
> Si la partición es una partición sin procesar (es decir, sin un sistema de archivos) que contiene datos (como un archivo de base de datos), la reducción de la partición puede destruir los datos.

## <a name="shrinking-a-basic-volume"></a>Reducir un volumen básico

-   [Usar la interfaz de Windows](#BKMK_WINUI)
-   [Usar una línea de comandos](#BKMK_CMD)

> [!NOTE]
> Debes ser miembro del grupo **Operadores de copia de seguridad** o **Administradores**, como mínimo, para completar estos pasos.

<a id="BKMK_WINUI"></a>
#### <a name="to-shrink-a-basic-volume-using-the-windows-interface"></a>Para reducir un volumen básico mediante la interfaz de Windows

1.  En el Administrador de discos, haz clic con el botón derecho en el volumen básico que quieres reducir.

2.  Haz clic en **Reducir volumen**.

3.  Sigue las instrucciones en pantalla.

<br />

> [!NOTE]
> Solo se pueden reducir los volúmenes básicos que no tienen ningún sistema de archivos o que usan el sistema de archivos NTFS.

<a id="BKMK_CMD"></a>
#### <a name="to-shrink-a-basic-volume-using-a-command-line"></a>Para reducir un volumen básico mediante una línea de comandos

1.  Abre un símbolo del sistema y escribe `diskpart`.

2.  En el símbolo del sistema **DISKPART**, escribe `list volume`. Ten en cuenta el número del volumen sencillo que quieres reducir.

3.  En el símbolo del sistema **DISKPART**, escribe `select volume <volumenumber>`. Selecciona el volumen sencillo *volumenumber* que quieres reducir.

4.  En el símbolo del sistema **DISKPART**, escribe `shrink [desired=<desiredsize>] [minimum=<minimumsize>]`. Reduce el volumen seleccionado a *desiredsize* en megabytes (MB), si es posible, o a *minimumsize* si *desiredsize* es demasiado grande.

<br />

| Valor | Descripción|
|---|---|
| <p>**list volume**</p> | <p>Muestra una lista de volúmenes básicos y dinámicos en todos los discos.</p>|
| <p>**select volume**</p> | <p>Selecciona el volumen especificado, donde <em>volumenumber</em> es el número de volumen y el que recibe el foco. Si no se especifica ningún volumen, el comando **select** muestra el volumen actual con el foco. Puedes especificar el volumen por número, letra de unidad o ruta de acceso de punto de montaje. En un disco básico, si seleccionas un volumen, este también recibe el foco de partición correspondiente.</p> |
| <p>**shrink**</p> | <p>Reduce el volumen con el foco para crear un espacio sin asignar. No se produce ninguna pérdida de datos. Si la partición incluye archivos que no pueden moverse (por ejemplo, el archivo de página o el área de almacenamiento de instantáneas), el volumen se reducirá hasta el punto donde se encuentran los archivos que no pueden moverse. |
| <p>**desired=** <em>desiredsize</em></p> | <p>La cantidad de espacio, en megabytes, para recuperarse en la partición actual.</p> |
| <p>**minimum=** <em>minimumsize</em></p> | <p>La cantidad mínima de espacio, en megabytes, para recuperarse en la partición actual. Si no se especifica un tamaño mínimo o deseado, el comando reclamará la cantidad máxima de espacio posible.</p> 

<a id="addcon"></a>

## <a name="additional-considerations"></a>Consideraciones adicionales

-   Al reducir una partición, no pueden reubicarse automáticamente determinados archivos (por ejemplo, el archivo de paginación o el área de almacenamiento de instantáneas), y no se puede reducir el espacio asignado más allá del punto donde se encuentran los archivos que no pueden moverse. Si se produce un error en la operación de reducción, comprueba el registro de la aplicación para el evento 259, que identificará el archivo que no puede moverse. Si sabes que los clústeres asociados con el archivo impiden la operación de reducción, también puedes usar el comando **fsutil** en un símbolo del sistema (escribe **fsutil volume querycluster /?** para el uso). Cuando se proporciona el parámetro **querycluster**, el resultado del comando identificará el archivo que no se puede mover que impide que la operación de reducción se realice correctamente.
En algunos casos, se puede cambiar la ubicación del archivo temporalmente. Por ejemplo, si necesitas reducir aún más la partición, puedes usar el Panel de control para mover el archivo de paginación o instantáneas almacenadas en otro disco, eliminar las instantáneas almacenadas, reducir el volumen y mover el archivo de paginación en el disco. Si el número de clústeres defectuosos detectados por la reasignación dinámica de clústeres defectuosos, no se puede reducir la partición. Si esto ocurre, debes considerar trasladar los datos y reemplazar el disco.

-  No uses una copia de nivel de bloque para transferir los datos. También se copiarán la tabla de sectores defectuosos y el nuevo disco tratará los mismos sectores como defectuosos aunque sean normales.

-   Puedes reducir particiones principales y unidades lógicas en particiones sin procesar (las que no disponen de un sistema de archivos) o particiones con el sistema de archivos NTFS.

## <a name="see-also"></a>Consulta también

-   [Administrar volúmenes básicos](manage-basic-volumes.md)