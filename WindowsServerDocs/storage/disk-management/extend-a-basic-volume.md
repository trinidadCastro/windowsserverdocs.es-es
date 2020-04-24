---
title: Extensión de un volumen básico
description: Puedes agregar espacio a un volumen existente en Windows y extenderlo al espacio vacío de la unidad, pero solo si el espacio vacío no tiene un volumen (está sin asignar) y va inmediatamente después del volumen que quieres extender, sin otros volúmenes en medio. En este artículo se describe cómo hacerlo.
ms.date: 12/19/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c72b242437c4c308da77a25e06f3d76e4c65f480
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "75351905"
---
# <a name="extend-a-basic-volume"></a>Extensión de un volumen básico

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Puedes usar Administración de discos para agregar espacio a un volumen existente y extenderlo al espacio vacío de la unidad, pero solo si el espacio vacío no tiene un volumen (está sin asignar) y va inmediatamente después del volumen que quieres extender, sin otros volúmenes entre ambos, como se muestra en la imagen siguiente. El volumen que se va a extender también debe estar formateado con los sistemas de archivos NTFS o ReFS.

:::image type="content" source="media/extend-volume-space-highlighted.png" alt-text="Administración de discos mostrando el espacio disponible en el que se puede extender un volumen.":::

## <a name="to-extend-a-volume-by-using-disk-management"></a>Para extender un volumen mediante Administración de discos

Aquí se muestra cómo extender un volumen a un espacio vacío inmediatamente después del volumen en la unidad:

1. Abre Administración de discos con permisos de administrador.

   Una manera fácil de hacerlo es escribir **Administración de equipos** en el cuadro de búsqueda de la barra de tareas, seleccionar y mantener pulsado (o hacer clic con el botón derecho) **Administración de equipos** y, después, seleccionar **Ejecutar como administrador** > **Sí**. Cuando se abra Administración de equipos, ve a **Almacenamiento** > **Administración de discos**.
2. Selecciona y mantén pulsado (o haz clic con el botón derecho) el volumen que quieres extender y, a continuación, selecciona **Extender volumen**.

   Si la opción **Extender volumen** está atenuada, comprueba lo siguiente:
    - Administración de discos o Administración de equipos se abrieron con permisos de administrador
    - Hay espacio sin asignar directamente después (a la derecha) del volumen, tal como se muestra en el gráfico anterior. Si hay otro volumen entre el espacio sin asignar y el volumen que quieres extender, puedes eliminar el volumen del medio y todos los archivos que contiene (asegúrate de mover o de realizar una copia de seguridad de todos los archivos importantes en primer lugar), usar una aplicación de creación de particiones de disco que no sea de Microsoft y que pueda mover volúmenes sin destruir datos, o bien omitir la extensión del volumen y, en su lugar, crear un volumen independiente en el espacio sin asignar.
    - El volumen está formateado con el sistema de archivos NTFS o ReFS. No se pueden extender otros sistemas de archivos, por lo que tendrás que mover o realizar una copia de seguridad de los archivos del volumen y, a continuación, formatear el volumen con el sistema de archivos NTFS o ReFS.
    - Si el disco es mayor de 2 TB, asegúrate de que usa el esquema de particiones GPT. Para usar más de 2 TB en un disco, se debe inicializar con el esquema de particiones GPT. Para la conversión a GPT, consulta [Conversión de un disco MBR en un disco GPT](change-an-mbr-disk-into-a-gpt-disk.md).
    - Si sigues sin poder extender el volumen, intenta buscar el sitio [Archivos, carpetas y almacenamiento de la comunidad Microsoft](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) y, si no encuentras la respuesta, publica una pregunta en el sitio y Microsoft u otros miembros de la comunidad intentarán ayudarte. Otra opción es ponerte en [contacto con el soporte técnico de Microsoft](https://support.microsoft.com/contactus/).

3. Selecciona **Siguiente** y, en la página **Seleccionar discos** del asistente (que se muestra aquí), especifica cuánto quieres extender el volumen. Por lo general, usarás el valor predeterminado, que utiliza todo el espacio libre disponible, pero puedes usar un valor menor si quieres crear volúmenes adicionales en el espacio libre.

   :::image type="content" source="media/extend-volume-wizard.png" alt-text="Asistente para la extensión del volumen mostrando un volumen que se extiende para ocupar todo el espacio disponible":::

4. Selecciona **Siguiente** y, a continuación, **Finalizar** para extender el volumen.

## <a name="to-extend-a-volume-by-using-powershell"></a>Para extender un volumen mediante PowerShell

1. Selecciona y mantén pulsado (o haz clic con el botón derecho) el botón Inicio y, a continuación, selecciona Windows PowerShell (Administrador).
2. Escribe el siguiente comando para cambiar el tamaño del volumen al máximo y especifica la letra de unidad del volumen que quieres extender en la variable *$drive _letter*:

   ```PowerShell
   # Variable specifying the drive you want to extend
   $drive_letter = "C"

   # Script to get the partition sizes and then resize the volume
   $size = (Get-PartitionSupportedSize -DriveLetter $drive_letter)
   Resize-Partition -DriveLetter $drive_letter -Size $size.SizeMax
   ```

## <a name="see-slso"></a>Consulta también

- [Resize-Partition](https://docs.microsoft.com/powershell/module/storage/resize-partition)
- [Extensión de DiskPart](https://docs.microsoft.com/windows-server/administration/windows-commands/extend)
