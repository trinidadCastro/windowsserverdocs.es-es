---
title: Cleanmgr
description: Obtenga información sobre cómo usar las opciones de línea de comandos para configurar la herramienta de limpieza de disco (Cleanmgr.exe) para limpiar automáticamente determinados archivos.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: edcf878723653c3b51c8b5b29e9cd93106159446
ms.sourcegitcommit: 078304c4b92bb57eb85ba29634afc92cc028c644
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67301576"
---
# <a name="cleanmgr"></a>Cleanmgr

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semianual)

Borra los archivos innecesarios del disco duro de su equipo. Puede usar las opciones de línea de comandos para especificar que Cleanmgr limpia archivos Temp, archivos Internet, los archivos descargados y archivos de la Papelera de reciclaje. A continuación, puede programar la tarea se ejecute en un momento determinado mediante el uso de la herramienta tareas programadas.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

## <a name="parameters"></a>Parámetros

|      Parámetro      |    Descripción     |
| ------------------- | ------------------ |
|  /d \<driveletter>          | Especifica la unidad que desea liberar espacio en disco para limpiar.<br>**Nota:** No se usa la opción/d. con /sagerun:n. |
| /sageset:n | Muestra el cuadro de diálogo Configuración de limpieza de disco y también crea una clave del registro para almacenar la configuración que seleccione. El `n` valor, que se almacena en el registro, le permite especificar las tareas para ejecutar el Liberador de espacio en disco. El `n` valor puede ser cualquier valor entero comprendido entre 0 y 65535. Para que todas las opciones disponibles cuando se usa la opción /sageset, especificar la unidad donde está instalado Windows.  |
|  /sagerun:n  |  Ejecuta las tareas especificadas que se asignan al valor n si utiliza la opción \sageset. Se enumeran todas las unidades en el equipo y el perfil seleccionado se ejecuta en cada unidad.           |
| /TuneUp:n    | Ejecute /sageset y /sagerun para el mismo `n` . |
| / LOWDISK     | Ejecutar con la configuración predeterminada. |
| / VERYLOWDISK | Ejecutar con la configuración predeterminada, ningún usuario le pide. |
| /?           | Mostrar la Ayuda. |

## <a name="options"></a>Opciones

Las opciones para los archivos que puede especificar para liberar espacio en disco mediante el uso de /sageset y /sagerun se incluyen:

- **Archivos temporales de instalación** -son archivos creados por un programa de instalación ya no se está ejecutando.

- **Archivos de programa descargados** : archivos de programa descargados son controles ActiveX y programas de Java que se descargan automáticamente de Internet cuando se ven determinadas páginas. Estos archivos se almacenan temporalmente en la carpeta archivos de programa descargados en el disco duro. Esta opción incluye un botón de ver los archivos para que puedan ver los archivos antes de liberar espacio en disco los elimine. El botón abre la carpeta archivos de programa C:\Winnt\Downloaded.

- **Archivos temporales de Internet** -carpeta de los archivos temporales de Internet contiene páginas Web que se almacenan en el disco duro para una visualización rápida. Liberar espacio en disco quita estas páginas, pero deja intacta la configuración personalizada de las páginas Web. Esta opción también incluye un botón de ver los archivos, que se abre la carpeta C:\Documents and Settings\Username\Local local\Archivos temporales Internet Internet\Content. IE5. 

- **Los archivos antiguos de Chkdsk** -Chkdsk cuando comprueba si hay errores en un disco, puede guardar fragmentos de archivos perdidos como archivos en la carpeta raíz en el disco. Estos archivos son innecesarios.

- **Papelera de reciclaje** -la Papelera de reciclaje contiene archivos que se eliminaron desde el equipo. Estos archivos no se quitan permanentemente hasta vacía la Papelera de reciclaje. Esta opción incluye un botón de ver los archivos que se abre la Papelera de reciclaje. **Nota:** Papelera de reciclaje pueden aparecer en más de una unidad, por ejemplo, no solo en % SystemRoot %.

- **Los archivos temporales** -programas a veces, almacenan información temporal en una carpeta Temp. Antes de cerrarse un programa, normalmente elimina esta información. Puede eliminar de forma segura los archivos temporales que no se han modificado en la última semana.

- **Archivos sin conexión temporales** -temporales archivos sin conexión son copias locales de los archivos de red usados recientemente. Estos archivos se almacenan en caché automáticamente para que pueda usar después de desconectarse de la red. Botón Ver archivos abre la carpeta de archivos sin conexión.

- **Los archivos sin conexión** -archivos sin conexión son copias locales de los archivos de red que específicamente desea tener disponibles sin conexión para que pueda usar después de desconectarse de la red. Botón Ver archivos abre la carpeta de archivos sin conexión.

- **Comprimir archivos antiguos** -Windows pueden comprimir los archivos que no ha usado recientemente. Compresión de archivos ahorra espacio en disco, pero todavía puede usar los archivos. Se elimina ningún archivo. Dado que los archivos se comprimen con distintas velocidades, la cantidad de espacio en disco que obtendrán es aproximada. Un botón de opciones permite especificar el número de días que se esperará antes de liberar espacio en disco se comprime un archivo no usado.

- **Archivos de catálogo para el indizador de contenido** -Index Server acelera y mejora las búsquedas de archivos mediante el mantenimiento de un índice de los archivos que están en el disco. Estos archivos de catálogo permanecen en una operación de indización anterior y se pueden eliminar de forma segura. **Nota:** Archivo de catálogo puede aparecer en más de una unidad, por ejemplo, no solo en % SystemRoot %.

**Tenga en cuenta** si especifica limpiar la unidad que contiene la instalación de Windows, todas estas opciones están disponibles en la ficha Liberador de espacio en disco. Si especifica cualquier otra unidad, solo la Papelera de reciclaje y los archivos de catálogo para las opciones de índice de contenido están disponibles en la pestaña de liberar espacio en disco. 

## <a name="examples"></a>Ejemplos

Para ejecutar la aplicación Liberador de espacio en disco para que pueda usar su cuadro de diálogo para especificar las opciones para su uso posterior, guardar la configuración en el conjunto **1**, escriba lo siguiente:

```
cleanmgr /sageset:1
```

Para ejecutar el Liberador de espacio en disco e incluyen las opciones que especificó con el comando /sageset:1 cleanmgr, escriba:

```
cleanmgr /sagerun:1
```

Para ejecutar ```cleanmgr /sageset:1``` y ```cleanmgr /sagerun:1``` juntos, escriba:

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>Referencias adicionales

[Libere espacio en disco en Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)
