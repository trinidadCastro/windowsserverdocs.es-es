---
title: rd
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 298e6b291a6aa08701b6d54a11470b0cc4bea486
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836718"
---
# <a name="rd"></a>rd



Elimina un directorio. Este comando es el mismo que el comando **rmdir** .

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

### <a name="parameters"></a>Parámetros

|     Parámetro     |                                                                 Descripción                                                                  |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [\<> de unidad:]<Path> |                      Especifica la ubicación y el nombre del directorio que desea eliminar. La *ruta de acceso* es obligatoria.                       |
|        /s         |                     Elimina un árbol de directorios (el directorio especificado y todos sus subdirectorios, incluidos todos los archivos).                      |
|        /q         | Especifica el modo silencioso. No solicita confirmación al eliminar un árbol de directorios. (Tenga en cuenta que **/q** solo funciona si se especifica **/s** ). |
|        /?         |                                                     Muestra la Ayuda en el símbolo del sistema.                                                     |

## <a name="remarks"></a>Comentarios

-   No se puede eliminar un directorio que contenga archivos, incluidos los archivos ocultos o del sistema. Si intenta hacerlo, aparece el siguiente mensaje:

    `The directory is not empty`

    Use el comando **dir/a** para mostrar todos los archivos (incluidos los archivos ocultos y del sistema). A continuación, use el comando **attrib** con **-h** para quitar atributos de archivo ocultos, **-s** para quitar atributos de archivo de sistema o **-h-s** para quitar los atributos de archivo ocultos y del sistema. Una vez quitados los atributos de archivo y ocultos, puede eliminar los archivos.
-   Si inserta una barra diagonal inversa (\) al principio de la *ruta de acceso*, la *ruta de acceso* se iniciará en el directorio raíz (independientemente del directorio actual).
-   No se puede usar **Rd** para eliminar el directorio actual. Si intenta eliminar el directorio actual, aparece el siguiente mensaje de error:

    `The process cannot access the file because it is being used by another process.`

    Si recibe este mensaje de error, debe cambiar a un directorio diferente (no un subdirectorio del directorio actual) y, a continuación, usar **Rd** (especifique la *ruta de acceso* si es necesario).
-   El comando **Rd** , con diferentes parámetros, está disponible en la consola de recuperación.

## <a name="examples"></a><a name=BKMK_examples></a>Example

No se puede eliminar el directorio en el que está trabajando actualmente. Debe cambiar a un directorio que no esté dentro del directorio actual. Por ejemplo, para cambiar al directorio principal, escriba:
```
cd ..
```
Ahora puede quitar el directorio deseado de forma segura.

Use la opción **/s** para quitar un árbol de directorios. Por ejemplo, para quitar un directorio denominado test (y todos sus subdirectorios y archivos) del directorio actual, escriba:
```
rd /s test
```
Para ejecutar el ejemplo anterior en modo silencioso, escriba:
```
rd /s /q test
```

> [!CAUTION]
> Al ejecutar **Rd/s** en modo silencioso, todo el árbol de directorios se elimina sin confirmación. Asegúrese de que se mueven o se realiza una copia de seguridad de los archivos importantes antes de usar la opción de línea de comandos **/q** .

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)