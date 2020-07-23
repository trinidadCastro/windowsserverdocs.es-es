---
title: format
description: Artículo de referencia del comando format, que da formato a un disco para aceptar archivos de Windows.
ms.prod: windows-server
manager: dongill
ms.author: jgerend
ms.technology: storage
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: jasongerend
ms.date: 10/16/2017
ms.openlocfilehash: 73f83a07cb1537af66d59977099b251b6dd12f47
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958227"
---
# <a name="format"></a>Formato

> Se aplica a: Windows 10, Windows Server 2016

Formatea un disco para aceptar archivos de Windows. Debe ser miembro del grupo administradores para dar formato a una unidad de disco duro.

> [!NOTE]
> También puede usar el comando **Format** , con parámetros diferentes, de la consola de recuperación. Para obtener más información acerca de la consola de recuperación, consulte [entorno de recuperación de Windows (Windows re)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxis

```
format <volume> [/fs:{FAT|FAT32|NTFS}] [/v:<label>] [/q] [/a:<unitsize>] [/c] [/x] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/f:<size>] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/t:<tracks> /n:<sectors>] [/p:<passes>]
format <volume> [/v:<label>] [/q] [/p:<passes>]
format <volume> [/q]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<volume>` | Especifica el punto de montaje, el nombre del volumen o la letra de unidad (seguido de un signo de dos puntos) de la unidad a la que desea dar formato. Si no especifica ninguna de las siguientes opciones de línea de comandos, **Format** usa el tipo de volumen para determinar el formato predeterminado del disco. |
| /FS: {FAT | FAT32 | NTFS | Especifica el tipo de sistema de archivos (FAT, FAT32, NTFS). |
| /v:`<label>` | Especifica la etiqueta del volumen. Si omite la opción de línea de comandos **/v** o la utiliza sin especificar una etiqueta de volumen, **Format** le pedirá la etiqueta del volumen una vez completado el formato. Use la sintaxis **/v:** para que no se solicite una etiqueta de volumen. Si utiliza un único comando **format** para dar formato a más de un disco, todos los discos recibirán la misma etiqueta de volumen. |
| /a`<unitsize>` | Especifica el tamaño de la unidad de asignación que se va a utilizar en volúmenes FAT, FAT32 o NTFS. Si no especifica *unitsin*, se elige en función del tamaño del volumen. Se recomienda la configuración predeterminada para uso general. En la siguiente lista se presentan los valores válidos para las *unidades*NTFS, FAT y FAT32:<ul><li>512</li><li>1024</li><li>2048</li><li>4096</li><li>8192</li><li>16 K</li><li>32 K</li><li>64 K</li></ul>FAT y FAT32 también admiten 128 K y 256 K para un tamaño de sector mayor de 512 bytes. |
| /q | Realiza un formato rápido. Elimina la tabla de archivos y el directorio raíz de un volumen con formato anterior, pero no realiza un examen sector por sector para las áreas defectuosas. Debe usar la opción de línea de comandos **/q** para dar formato solo a los volúmenes con el formato anterior que sepa que están en buenas condiciones. Tenga en cuenta que **/q** reemplaza a **/p**. |
| /f`<size>` | Especifica el tamaño del disquete que se va a dar formato. Cuando sea posible, use esta opción de línea de comandos en lugar de las opciones de línea de comandos **/t** y **/n** . Windows acepta los siguientes valores para el tamaño:<ul><li>1440 o 1440k o 1440kb</li><li>1,44 o 1,44 m o 1,44 MB</li><li>1,44 MB, doble cara, cuádruple densidad, disco de 3,5 pulgadas</li></ul> |
| /t:`<tracks>` | Especifica el número de pistas en el disco. Cuando sea posible, use la opción de línea de comandos **/f** en su lugar. Si utiliza la opción **/t**, también debe utilizar la opción **/n**. Juntas, estas opciones proporcionan un método alternativo de especificar el tamaño del disco que se está dando formato. Esta opción no es válida con la opción **/f**. |
| /n`<sectors>` | Especifica el número de sectores por pista. Cuando sea posible, use la opción de línea de comandos **/f** en lugar de **/n**. Si utiliza **/n**, también debe usar **/t**. Estas dos opciones juntas proporcionan un método alternativo de especificar el tamaño del disco que se está dando formato. Esta opción no es válida con la opción **/f**. |
| /p`<passes>` | Incluye ceros en cada sector del volumen para el número de pasadas especificado. Esta opción no es válida con la opción **/q**. |
| /C | Solo NTFS. De forma predeterminada, los archivos creados en el nuevo volumen se comprimirán. |
| /x | Hace que el volumen se desmonte, si es necesario, antes de darle formato. Todos los controladores abiertos en el volumen ya no serán válidos. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- El comando **Format** crea un nuevo directorio raíz y el sistema de archivos para el disco. También puede comprobar si hay áreas defectuosas en el disco y puede eliminar todos los datos del disco. Para poder usar un disco nuevo, primero debe usar este comando para formatear el disco.

- Después de formatear un disquete, **Format** muestra el siguiente mensaje:

    `Volume label (11 characters, ENTER for none)?`

    Para agregar una etiqueta de volumen, escriba hasta 11 caracteres (incluidos espacios). Si no desea agregar una etiqueta de volumen al disco, presione Entrar.

- Cuando se usa el comando **Format** para formatear un disco duro, aparece un mensaje de advertencia similar al siguiente:

  ```
  WARNING, ALL DATA ON NON-REMOVABLE DISK
  DRIVE x: WILL BE LOST!
  Proceed with Format (Y/N)? _
  ```

  Para formatear el disco duro, presione **Y**; Si no desea formatear el disco, presione **N**.

- Los sistemas de archivos FAT restringen el número de clústeres a un máximo de 65526. Los sistemas de archivos FAT32 restringen el número de clústeres entre 65527 y 4177917.

- No se admite la compresión NTFS para tamaños de unidad de asignación superiores a 4096.

  > [!NOTE]
  > El **formato** detendrá inmediatamente el procesamiento si determina que no se pueden cumplir los requisitos anteriores con el tamaño de clúster especificado.

- Cuando se completa el formato, el **formato** muestra mensajes que muestran el espacio total en disco, los espacios marcados como defectuosos y el espacio disponible para los archivos.

- Puede acelerar el proceso de formato mediante la opción de línea de comandos **/q** . Utilice esta opción solo si no hay sectores defectuosos en el disco duro.

- No debe usar el comando **Format** en una unidad que se haya preparado con el comando **subst** . No se puede dar formato a los discos a través de una red.

- En la tabla siguiente se enumeran los códigos de salida y una breve descripción de su significado.

  | Código de salida | Descripción |
  | --------- | ----------- |
  | 0 | La operación de formato se realizó correctamente. |
  | 1 | Se proporcionaron parámetros incorrectos. |
  | 4 | Se produjo un error irrecuperable (que es cualquier error distinto de 0, 1 o 5). |
  | 5 | El usuario presionó N en respuesta a la pregunta "¿desea continuar con el formato (Y/N)?" para detener el proceso. |

  Puede comprobar estos códigos de salida mediante la variable de entorno ERRORLEVEL con el comando por lotes **if**.

### <a name="examples"></a>Ejemplos

Para dar formato a un disquete nuevo en la unidad A con el tamaño predeterminado, escriba:

```
format a:
```

Para realizar una operación de formato rápido en un disquete previamente formateado en la unidad A, escriba:

```
format a: /q
```

Para formatear un disquete en la unidad A y asignarle los *datos*de la etiqueta de volumen, escriba:

```
format a: /v:DATA
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc771080(v=ws.11))
