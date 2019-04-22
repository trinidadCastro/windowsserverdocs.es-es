---
title: Formato
ms.prod: windows-server-threshold
manager: dongill
ms.author: JGerend
ms.technology: storage
ms.topic: article
ms.assetid: 51ec7423-9a01-4219-868a-25d69cdcc832
author: JasonGerend
ms.date: 10/16/2017
ms.openlocfilehash: 10c0dd97ddd7384673573c1354105314af5d5e04
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818986"
---
# <a name="format"></a>Formato
> Se aplica a: Windows 10, Windows Server 2016

Da formato a un disco para que acepte los archivos de Windows.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
format <Volume> [/fs:{FAT|FAT32|NTFS}] [/v:<Label>] [/q] [/a:<UnitSize>] [/c] [/x] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/f:<Size>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/t:<Tracks> /n:<Sectors>] [/p:<Passes>]
format <Volume> [/v:<Label>] [/q] [/p:<Passes>]
format <Volume> [/q]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Volume>|Especifica el punto de montaje, el nombre de volumen o la letra de unidad (seguido de dos puntos) de la unidad que desea dar formato. Si no se especifica cualquiera de las siguientes opciones de línea de comandos, **formato** usa el tipo de volumen para determinar el formato predeterminado para el disco.|
|/fs:{FAT|FAT32|NTFS|UDF}|Especifica el tipo del sistema de archivos: FAT, FAT32, NTFS o UDF. Los disquetes puede usar solo el sistema de archivos FAT.|
|/ v:\<etiqueta >|Especifica la etiqueta del volumen. Si se omite el **/v** de línea de comandos de opción o usarla sin especificar una etiqueta de volumen, **formato** le pide la etiqueta de volumen una vez completado el formato. Use la sintaxis **/v:** para que no se solicite una etiqueta de volumen. Si utiliza un único comando **format** para dar formato a más de un disco, todos los discos recibirán la misma etiqueta de volumen.|
|/a:\<UnitSize>|Especifica el tamaño de unidad de asignación para utilizar en volúmenes FAT, FAT32 o NTFS. Si no se especifica *UnitSize*, se elige según el tamaño del volumen. Se recomienda la configuración predeterminada para uso general. La siguiente lista muestra los valores válidos para *UnitSize* de NTFS, FAT y FAT32:</br>512</br>1024</br>2048</br>4096</br>8192</br>16 K</br>32 K</br>64 K</br>FAT y FAT32 también admiten 128 K y 256 K para un tamaño de sector mayor de 512 bytes.|
|/q|Realiza un formato rápido. Elimina la tabla de archivos y el directorio raíz de un volumen formateado anteriormente, pero no realiza un examen sector por sector defectuosos. Debe usar el **/q** los volúmenes que estén en buenas condiciones de opción de línea de comandos para dar formato solo previamente formateados. Tenga en cuenta que **/q** reemplaza a **/p**.|
|/ f:\<tamaño >|Especifica el tamaño del disquete que se va a dar formato. Cuando sea posible, use esta opción de línea de comandos en lugar de la **/t** y **/n** opciones de línea de comandos. Windows acepta los siguientes valores para el tamaño:</br>-1440 o 1440 k o 1440 kb</br>-1,44 o 1,44 m o 1,44 mb</br>-Disco de 3,5 pulgadas, 1,44 MB, caras, cuádruple densidad|
|/ t:\<pistas >|Especifica el número de pistas en el disco. Cuando sea posible, use el **/f** de línea de comandos en su lugar la opción. Si utiliza la opción **/t**, también debe utilizar la opción **/n**. Juntas, estas opciones proporcionan un método alternativo de especificar el tamaño del disco que se está dando formato. Esta opción no es válida con la opción **/f**.|
|/ n:\<sectores >|Especifica el número de sectores por pista. Cuando sea posible, use el **/f** la opción de línea de comandos en lugar de **/n**. Si utiliza **/n**, también debe usar **/t**. Estas dos opciones juntas proporcionan un método alternativo de especificar el tamaño del disco que se está dando formato. Esta opción no es válida con la opción **/f**.|
|/ p:\<pasadas >|Incluye ceros en cada sector del volumen para el número de pasadas especificado. Esta opción no es válida con la opción **/q**.|
|/c|Solo NTFS. De forma predeterminada, los archivos creados en el nuevo volumen se comprimirán.|
|/x|Hace que el volumen se desmonte, si es necesario, antes de darle formato. Todos los controladores abiertos en el volumen ya no serán válidos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Credenciales administrativas

    Debe ser miembro del grupo Administradores para formatear un disco duro.
-   Uso de **formato**

    El **formato** comando crea un sistema de archivos y directorios de raíz nuevo para el disco. También puede comprobar si hay sectores defectuosos en el disco y pueden eliminar todos los datos en el disco. Para poder usar un disco nuevo, primero debe usar este comando para formatear el disco.
-   Agregar una etiqueta de volumen

    Después de formatear un disquete, **formato** muestra el mensaje siguiente:

    `Volume label (11 characters, ENTER for none)?`

    Para agregar una etiqueta de volumen, escriba hasta 11 caracteres (incluidos espacios). Si no desea agregar una etiqueta de volumen en el disco, presione ENTRAR.
-   Formatear un disco duro

> [!NOTE]
> Debe ser miembro del grupo Administradores para formatear un disco duro.

Cuando se usa el **formato** comando para dar formato a un disco duro, un mensaje de advertencia similar a la muestra siguiente:
```
WARNING, ALL DATA ON NON-REMOVABLE DISK 
DRIVE x: WILL BE LOST! 
Proceed with Format (Y/N)? _ 
```
Para formatear el disco duro, presione S; Si no desea formatear el disco, presione N.
-   Tamaño de unidad

    Sistemas de archivos FAT restringen el número de clústeres a no más de 65526. Sistemas de archivos FAT32 restringen el número de clústeres a entre 65.527 y 4.177.917.

    No se admite la compresión NTFS para tamaños de unidad de asignación superiores a 4096.

> [!NOTE]
> **Formato** detendrá inmediatamente el procesamiento si determina que no pueden cumplirse los requisitos anteriores con el tamaño de clúster especificado.
-   **Formato** mensajes

    Cuando se complete la formato **formato** muestra mensajes que muestran el espacio en disco total, los espacios de marcado como defectuoso y el espacio disponible para los archivos.
-   Formato rápido

    Puede acelerar el proceso de formato mediante el **/q** opción de línea de comandos. Utilice esta opción solo si no hay sectores defectuosos en el disco duro.
-   Uso de **format** con una unidad reasignada o una unidad de red

    No debe utilizar el comando **format** en una unidad que se ha preparado con el comando **subst**. No se puede dar formato a discos en una red.
-   Códigos de salida de **format**

    En la tabla siguiente se enumeran los códigos de salida y una breve descripción de su significado.  
    |Código de salida|Descripción|
    |---------|-----------|
    |0|La operación de formato se realizó correctamente.|
    |1|Se proporcionaron parámetros incorrectos.|
    |4|Se produjo un error irrecuperable (que es cualquier error distinto de 0, 1 ó 5).|
    |5|El usuario presionó N en respuesta a la pregunta "¿continuar con el formato (S/N)?" para detener el proceso.|

    Puede comprobar estos códigos de salida mediante la variable de entorno ERRORLEVEL con el comando por lotes **if**.
-   Uso de **format** en la Consola de recuperación

    El comando **format**, con diferentes parámetros, está disponible en la Consola de recuperación.

## <a name="BKMK_examples"></a>Ejemplos

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

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](https://technet.microsoft.com/library/cc771080.aspx)