---
title: replace
description: Obtenga información sobre cómo usar el comando replace para reemplazar los archivos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 293534a2287fe0219643dacc88926018c37dbdcc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441770"
---
# <a name="replace"></a>replace



Reemplaza los archivos. Si se usa con el **/a** opción, **reemplazar** agrega nuevos archivos en un directorio en lugar de reemplazar los archivos existentes.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/a] [/p] [/r] [/w] 
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/p] [/r] [/s] [/w] [/u] 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive1>:][\<Path1>]\<FileName>|Especifica la ubicación y el nombre del archivo de origen o un conjunto de archivos. *Nombre de archivo* es necesario y puede incluir caracteres comodín ( **&#42;** y **?** ).|
|[\<Drive2>:][\<Path2>]|Especifica la ubicación del archivo de destino. No se puede especificar un nombre de archivo para reemplazar los archivos. Si no especifica una unidad o ruta de acceso, **reemplazar** usa la unidad actual y el directorio como el destino.|
|/a|Agrega nuevos archivos al directorio de destino en lugar de reemplazar los archivos existentes. No se puede usar esta opción de línea de comandos con el **/s** o **/u** opción de línea de comandos.|
|/p|Solicita confirmación antes de reemplazar un archivo de destino o agregar un archivo de origen.|
|/r|Reemplaza los archivos de solo lectura y sin proteger. Si intenta reemplazar un archivo de solo lectura, pero no se especifica **/r**, un error y se detendrá la operación de reemplazo.|
|/w|Espera que se va a insertar un disco antes de que comience la búsqueda de archivos de código fuente. Si no especifica **/w**, **reemplazar** comienza a reemplazar o agregar archivos inmediatamente después de presionar ENTRAR.|
|/s|Busca en todos los subdirectorios del directorio de destino y reemplaza los archivos coincidentes. No puede usar **/s** con el **/a** opción de línea de comandos. El **reemplazar** comando no busca en subdirectorios que se especifican en *Path1*.|
|/u|Reemplaza solo esos archivos en el directorio de destino que son anteriores a los del directorio de origen. No puede usar **/u** con el **/a** opción de línea de comandos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

- Como **reemplazar** agrega o reemplaza archivos, el archivo se muestran los nombres en la pantalla. Después de **reemplazar** haya finalizado, se muestra una línea de resumen en uno de los siguientes formatos:  
  ```
  nnn files added
  nnn files replaced
  no file added
  no file replaced
  ```  
- Si usas los disquetes y necesita cambiar discos durante la **reemplazar** operación, puede especificar el **/w** opción de línea de comandos para que **reemplazar** esperará para que pueda cambiar los discos.
- No puede usar **reemplazar** para actualizar los archivos ocultos o archivos del sistema.
- En la tabla siguiente se muestra los códigos de salida y una breve descripción de su significado:  
  |Código de salida|Descripción|
  |---------|-----------|
  |0|El **reemplazar** comando reemplaza o agrega los archivos correctamente.|
  |1|El **reemplazar** comando encontró una versión incorrecta de MS-DOS.|
  |2|El **reemplazar** comando no encontró los archivos de origen.|
  |3|El **reemplazar** comando no encontró la ruta de acceso de origen o destino.|
  |5|El usuario no tiene acceso a los archivos que se va a reemplazar.|
  |8|No hay memoria de sistema insuficiente para llevar a cabo el comando.|
  |11|El usuario ha utilizado una sintaxis incorrecta en la línea de comandos.|

> [!NOTE]
> Puede usar el parámetro ERRORLEVEL en el **si** línea de comandos en un programa por lotes para procesar los códigos de salida devueltos por **reemplazar**.

## <a name="BKMK_examples"></a>Ejemplos

Para actualizar todas las versiones de un archivo denominado teléfono.CLI (que aparecen en varios directorios en la unidad C), con la versión más reciente del archivo teléfono.CLI desde un disquete en la unidad, escriba:

`replace a:\phones.cli c:\ /s`

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)