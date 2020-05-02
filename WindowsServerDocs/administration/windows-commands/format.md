---
title: Formato
ms.prod: windows-server
manager: dongill
ms.author: jgerend
ms.technology: storage
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: jasongerend
ms.date: 10/16/2017
ms.openlocfilehash: ee2454cbbf817d3713e999ac2899da352a175272
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725562"
---
# <a name="format"></a>Formato
> Se aplica a: Windows 10, Windows Server 2016

Formatea un disco para aceptar archivos de Windows.



## <a name="syntax"></a>Sintaxis

```
format <Volume> [/fs:{FAT|FAT32|NTFS}] [/v:<Label>] [/q] [/a:<UnitSize>] [/c] [/x] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/f:<Size>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/t:<Tracks> /n:<Sectors>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/p:<Passes>]
format <Volume> [/q]
```

### <a name="parameters"></a>Parámetros

|   Parámetro    |                                                                                                                                                                                                                    Descripción                                                                                                                                                                                                                     |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   \<> de volumen    |                                                                                         Especifica el punto de montaje, el nombre del volumen o la letra de unidad (seguido de un signo de dos puntos) de la unidad a la que desea dar formato. Si no especifica ninguna de las siguientes opciones de línea de comandos, **Format** usa el tipo de volumen para determinar el formato predeterminado del disco.                                                                                         |
|    /FS: {FAT    |                                                                                                                                                                                                                       FAT32                                                                                                                                                                                                                        |
|  /v:\<etiqueta>   |                           Especifica la etiqueta del volumen. Si omite la opción de línea de comandos **/v** o la utiliza sin especificar una etiqueta de volumen, **Format** le pedirá la etiqueta del volumen una vez completado el formato. Use la sintaxis **/v:** para que no se solicite una etiqueta de volumen. Si utiliza un único comando **format** para dar formato a más de un disco, todos los discos recibirán la misma etiqueta de volumen.                            |
| /a:\<unitsin> | Especifica el tamaño de la unidad de asignación que se va a utilizar en volúmenes FAT, FAT32 o NTFS. Si no se especifica *UnitSize*, se elige según el tamaño del volumen. Se recomienda la configuración predeterminada para uso general. La siguiente lista muestra los valores válidos para *UnitSize* de NTFS, FAT y FAT32:</br>512</br>1024</br>2048</br>4096</br>8192</br>16 000</br>32 K</br>64 K</br>FAT y FAT32 también admiten 128 K y 256 K para un tamaño de sector mayor de 512 bytes. |
|       /q       |                                                       Realiza un formato rápido. Elimina la tabla de archivos y el directorio raíz de un volumen con formato anterior, pero no realiza un examen sector por sector para las áreas defectuosas. Debe usar la opción de línea de comandos **/q** para dar formato solo a los volúmenes con el formato anterior que sepa que están en buenas condiciones. Tenga en cuenta que **/q** reemplaza a **/p**.                                                       |
|   /f:\<tamaño>   |                                                         Especifica el tamaño del disquete que se va a dar formato. Cuando sea posible, use esta opción de línea de comandos en lugar de las opciones de línea de comandos **/t** y **/n** . Windows acepta los siguientes valores para el tamaño:</br>-1440 o 1440 k o 1440 kb</br>-1,44 o 1,44 m o 1,44 mb</br>-1,44-MB, doble cara, cuádruple densidad, disco de 3,5 pulgadas                                                         |
|  /t:\<realiza un seguimiento de>  |                                                    Especifica el número de pistas en el disco. Cuando sea posible, use la opción de línea de comandos **/f** en su lugar. Si utiliza la opción **/t**, también debe utilizar la opción **/n**. Juntas, estas opciones proporcionan un método alternativo de especificar el tamaño del disco que se está dando formato. Esta opción no es válida con la opción **/f**.                                                     |
| /n:\<sectores>  |                                                         Especifica el número de sectores por pista. Cuando sea posible, use la opción de línea de comandos **/f** en lugar de **/n**. Si utiliza **/n**, también debe usar **/t**. Estas dos opciones juntas proporcionan un método alternativo de especificar el tamaño del disco que se está dando formato. Esta opción no es válida con la opción **/f**.                                                         |
|  /p:\<pasa>  |                                                                                                                                                               Incluye ceros en cada sector del volumen para el número de pasadas especificado. Esta opción no es válida con la opción **/q**.                                                                                                                                                                |
|       /C       |                                                                                                                                                                                     Solo NTFS. De forma predeterminada, los archivos creados en el nuevo volumen se comprimirán.                                                                                                                                                                                      |
|       /x       |                                                                                                                                                            Hace que el volumen se desmonte, si es necesario, antes de darle formato. Todos los controladores abiertos en el volumen ya no serán válidos.                                                                                                                                                            |
|       /?       |                                                                                                                                                                                                        Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                        |

## <a name="remarks"></a>Observaciones

-   Credenciales administrativas

    Debe ser miembro del grupo administradores para dar formato a una unidad de disco duro.
-   Usar el **formato**

    El comando **Format** crea un nuevo directorio raíz y el sistema de archivos para el disco. También puede comprobar si hay áreas defectuosas en el disco y puede eliminar todos los datos del disco. Para poder usar un disco nuevo, primero debe usar este comando para formatear el disco.
-   Agregar una etiqueta de volumen

    Después de formatear un disquete, **Format** muestra el siguiente mensaje:

    `Volume label (11 characters, ENTER for none)?`

    Para agregar una etiqueta de volumen, escriba hasta 11 caracteres (incluidos espacios). Si no desea agregar una etiqueta de volumen al disco, presione Entrar.
-   Dar formato a un disco duro

> [!NOTE]
> Debe ser miembro del grupo administradores para dar formato a un disco duro.

Cuando se usa el comando **Format** para formatear un disco duro, aparece un mensaje de advertencia similar al siguiente:
```
WARNING, ALL DATA ON NON-REMOVABLE DISK 
DRIVE x: WILL BE LOST! 
Proceed with Format (Y/N)? _ 
```
Para formatear el disco duro, presione Y; Si no desea formatear el disco, presione N.
-   Tamaño de unidad

    Los sistemas de archivos FAT restringen el número de clústeres a un máximo de 65526. Los sistemas de archivos FAT32 restringen el número de clústeres entre 65527 y 4177917.

    No se admite la compresión NTFS para tamaños de unidad de asignación superiores a 4096.

> [!NOTE]
> El **formato** detendrá inmediatamente el procesamiento si determina que no se pueden cumplir los requisitos anteriores con el tamaño de clúster especificado.
> -   **Dar formato** a los mensajes

    When formatting is complete, **format** displays messages that show the total disk space, the spaces marked as defective, and the space available for your files.
- Formato rápido

  Puede acelerar el proceso de formato mediante la opción de línea de comandos **/q** . Utilice esta opción solo si no hay sectores defectuosos en el disco duro.
- Uso de **format** con una unidad reasignada o una unidad de red

  No debe utilizar el comando **format** en una unidad que se ha preparado con el comando **subst**. No se puede dar formato a discos en una red.
- Códigos de salida de **format**

  En la tabla siguiente se enumeran los códigos de salida y una breve descripción de su significado.  

  |Código de salida|Descripción|
  |---------|-----------|
  |0|La operación de formato se realizó correctamente.|
  |1|Se proporcionaron parámetros incorrectos.|
  |4|Se produjo un error irrecuperable (que es cualquier error distinto de 0, 1 o 5).|
  |5|El usuario presionó N en respuesta a la pregunta "¿desea continuar con el formato (Y/N)?" para detener el proceso.|

  Puede comprobar estos códigos de salida mediante la variable de entorno ERRORLEVEL con el comando por lotes **if**.
- Uso de **format** en la Consola de recuperación

  El comando **format**, con diferentes parámetros, está disponible en la Consola de recuperación.

## <a name="examples"></a><a name="BKMK_examples"></a>Ejemplos

Para dar formato a un disquete nuevo en la unidad A con el tamaño predeterminado, escriba:
```
format a:
```
Para realizar una operación de formato rápido en un disquete previamente formateado en la unidad A, escriba:
```
format a: /q
```
Para formatear un disquete en la unidad A y asignarle la etiqueta de volumen "DATA", escriba:
```
format a: /v:DATA
```

## <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](https://technet.microsoft.com/library/cc771080.aspx)