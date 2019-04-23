---
title: rd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9190a77369c0a4631db87ab5a5c112b13b37e6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840046"
---
# <a name="rd"></a>rd



Elimina un directorio. Este comando es el mismo que el **rmdir** comando.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive>:]<Path>|Especifica la ubicación y el nombre del directorio que desea eliminar. *Ruta de acceso* es necesario.|
|/s|Elimina un árbol de directorio (el directorio especificado y todos sus subdirectorios, incluidos todos los archivos).|
|/q|Especifica el modo silencioso. No solicita confirmación al eliminar un árbol de directorios. (Tenga en cuenta que **/q** funciona únicamente si **/s** se especifica.)|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   No se puede eliminar un directorio que contiene los archivos, incluidos los ocultos o archivos del sistema. Si intenta hacerlo, aparece el mensaje siguiente:

    `The directory is not empty`

    Use la **dir /a** comando para enumerar todos los archivos (incluidos ocultos y los archivos del sistema). A continuación, utilice el **attrib** comando **-h** para quitar atributos de archivo oculto, **-s** para quitar atributos de archivo del sistema, o **-h -s** para quitar ambos ocultos y los atributos de archivo del sistema. Después de ocultar y se han quitado los atributos de archivo, puede eliminar los archivos.
-   Si inserta una barra diagonal inversa (\) al principio de *ruta*, *ruta* se iniciará en el directorio raíz (independientemente del directorio actual).
-   No puede usar **rd** para eliminar el directorio actual. Si intenta eliminar el directorio actual, aparece el mensaje de error siguiente:

    `The process cannot access the file because it is being used by another process.`

    Si recibe este mensaje de error, debe cambiar a un directorio diferente (que no sea un subdirectorio del directorio actual) y, a continuación, usar **rd** (especificar *ruta* si es necesario).
-   El **rd** comando, con diferentes parámetros, está disponible en la consola de recuperación.

## <a name="BKMK_examples"></a>Ejemplos

No se puede eliminar el directorio que está trabajando actualmente en. Debe cambiar a un directorio que no está dentro del directorio actual. Por ejemplo, para cambiar al directorio principal, escriba:
```
cd ..
```
Ahora puede quitar el directorio que desee.

Use la **/s** opción para quitar un árbol de directorios. Por ejemplo quitar un directorio denominado Test (y todos sus subdirectorios y archivos) desde el directorio actual, escriba:
```
rd /s test
```
Para ejecutar el ejemplo anterior en modo silencioso, escriba:
```
rd /s /q test
```

> [!CAUTION]
> Al ejecutar **rd /s** en modo silencioso, el árbol de directorios completo se eliminará sin confirmación. Asegurarse de que los archivos importantes se mueve o copia de seguridad antes de usar el **/q** opción de línea de comandos.

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)