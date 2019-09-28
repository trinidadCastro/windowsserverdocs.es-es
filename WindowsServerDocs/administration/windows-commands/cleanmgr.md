---
title: cleanmgr
description: Obtenga información acerca de cómo usar las opciones de línea de comandos para configurar la herramienta liberador de espacio en disco (cleanmgr. exe) para limpiar automáticamente determinados archivos.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: 20bc60abc747e6bab0ef59f38d0a392f18d75abe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379374"
---
# <a name="cleanmgr"></a>cleanmgr

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semianual)

Borra archivos innecesarios del disco duro del equipo. Puede usar las opciones de línea de comandos para especificar que cleanmgr Limpie los archivos temporales, los archivos de Internet, los archivos descargados y los archivos de la papelera de reciclaje. A continuación, puede programar la tarea para que se ejecute a una hora concreta mediante la herramienta tareas programadas.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#examples).

## <a name="syntax"></a>Sintaxis

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

## <a name="parameters"></a>Parámetros

|      Parámetro      |    Descripción     |
| ------------------- | ------------------ |
|  /d \<driveletter >          | Especifica la unidad que desea que limpie el liberador de espacio en disco.<br>**Nota:** La opción/d no se emplea con/sagerun: n. |
| /sageset: n | Muestra el cuadro de diálogo Configuración de limpieza de disco y también crea una clave del registro para almacenar la configuración seleccionada. El valor `n`, que se almacena en el registro, le permite especificar las tareas para que se ejecute el liberador de espacio en disco. El valor de `n` puede ser cualquier valor entero comprendido entre 0 y 65535. Para que todas las opciones estén disponibles cuando se usa la opción/sageset, especifique la unidad donde está instalado Windows.  |
|  /sagerun: n  |  Ejecuta las tareas especificadas que se asignan al valor n si se utiliza la opción \sageset. Se enumeran todas las unidades del equipo y se ejecuta el perfil seleccionado en cada unidad.           |
| /TUNEUP: n    | Ejecute/sageset y/sagerun para el mismo `n`. |
| /LOWDISK     | Ejecute con la configuración predeterminada. |
| /VERYLOWDISK | Ejecute con la configuración predeterminada, sin mensajes de usuario. |
| /?           | Mostrar ayuda. |

## <a name="options"></a>Opciones

Las opciones de los archivos que puede especificar para el liberador de espacio en disco mediante/sageset y/sagerun incluyen:

- **Archivos de instalación temporales** : son archivos creados por un programa de instalación que ya no se está ejecutando.

- **Archivos de programa descargados** : los archivos de programa descargados son controles ActiveX y programas Java que se descargan automáticamente desde Internet cuando se ven determinadas páginas. Estos archivos se almacenan temporalmente en la carpeta archivos de programa descargados del disco duro. Esta opción incluye un botón ver archivos para que pueda ver los archivos antes de que el liberador de espacio en disco los quite. El botón abre la carpeta archivos de programa C:\Winnt\Downloaded.

- **Archivos temporales de Internet** : la carpeta archivos temporales de Internet contiene páginas web que se almacenan en el disco duro para una visualización rápida. El liberador de espacio en disco quita estas páginas pero deja intacta la configuración personalizada para las páginas Web. Esta opción también incluye un botón ver archivos, que abre la carpeta C:\Documents and Settings\Username\Local Settings\Temporary Internet Files\Content.IE5. 

- **Archivos CHKDSK antiguos** : cuando CHKDSK comprueba si hay errores en un disco, CHKDSK podría guardar fragmentos de archivos perdidos como archivos en la carpeta raíz del disco. Estos archivos no son necesarios.

- **Papelera de reciclaje** : la papelera de reciclaje contiene archivos que se han eliminado del equipo. Estos archivos no se quitan permanentemente hasta que se vacía la papelera de reciclaje. Esta opción incluye un botón ver archivos que abre la papelera de reciclaje. **Nota:** Una papelera de reciclaje puede aparecer en más de una unidad, por ejemplo, no solo en% SystemRoot%.

- **Archivos temporales: los** programas a veces almacenan información temporal en una carpeta temporal. Antes de salir de un programa, el programa normalmente elimina esta información. Puede eliminar de forma segura los archivos temporales que no se han modificado en la última semana.

- **Archivos sin conexión temporales** : los archivos sin conexión temporales son copias locales de archivos de red usados recientemente. Estos archivos se almacenan en caché automáticamente para que pueda usarlos después de desconectarse de la red. Un botón ver archivos abre el Archivos sin conexión carpeta.

- **Archivos sin conexión** los archivos sin conexión son copias locales de archivos de red que desea que estén disponibles de forma específica para poder usarlos después de desconectarse de la red. Un botón ver archivos abre el Archivos sin conexión carpeta.

- **Comprimir archivos antiguos** : Windows puede comprimir archivos que no ha usado recientemente. La compresión de archivos ahorra espacio en disco, pero puede seguir usando los archivos. No se elimina ningún archivo. Dado que los archivos se comprimen con diferentes tasas, la cantidad de espacio en disco que se va a obtener es aproximada. Un botón opciones permite especificar el número de días de espera antes de que el liberador de espacio en disco comprima un archivo no utilizado.

- **Archivos de catálogo del indexador de contenido** : el servicio de indización acelera y mejora las búsquedas de archivos manteniendo un índice de los archivos que se encuentran en el disco. Estos archivos de catálogo permanecen en una operación de indexación anterior y se pueden eliminar de forma segura. **Nota:** El archivo de catálogo puede aparecer en más de una unidad, por ejemplo, no solo en% SystemRoot%.

**Nota:** Si especifica la limpieza de la unidad que contiene la instalación de Windows, todas estas opciones están disponibles en la ficha liberador de espacio en disco. Si especifica cualquier otra unidad, en la ficha liberador de espacio en disco solo estarán disponibles la papelera de reciclaje y los archivos de catálogo para las opciones del índice de contenido. 

## <a name="examples"></a>Ejemplos

Para ejecutar la aplicación liberador de espacio en disco para que pueda usar su cuadro de diálogo para especificar opciones para su uso posterior, guardando la configuración en el conjunto **1**, escriba lo siguiente:

```
cleanmgr /sageset:1
```

Para ejecutar el liberador de espacio en disco e incluir las opciones que especificó con el comando cleanmgr/sageset: 1, escriba:

```
cleanmgr /sagerun:1
```

Para ejecutar ```cleanmgr /sageset:1``` y ```cleanmgr /sagerun:1``` juntos, escriba:

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>Referencias adicionales

[Liberar espacio de la unidad en Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)
