---
title: rmdir
description: Artículo de referencia del comando rmdir, que elimina un directorio.
ms.topic: reference
ms.assetid: e4d7162b-7132-413c-a416-6eb3dc032a26
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 524ca632c11c7d861ed431c4bc86dbdb2b35abc8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640286"
---
# <a name="rmdir"></a>rmdir

Elimina un directorio.

El comando **rmdir** también se puede ejecutar desde la consola de recuperación de Windows, con parámetros diferentes. Para obtener más información, vea [entorno de recuperación de Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

> [!NOTE]
> Este comando es el mismo que el [comando Rd](rd.md).

## <a name="syntax"></a>Sintaxis

```
rmdir [<drive>:]<path> [/s [/q]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[<drive>:]<path>` | Especifica la ubicación y el nombre del directorio que desea eliminar. La *ruta de acceso* es obligatoria. Si incluye una barra diagonal inversa ( \) al principio de la ruta de *acceso*especificada), la *ruta de acceso* se inicia en el directorio raíz (independientemente del directorio actual). |
| /s | Elimina un árbol de directorios (el directorio especificado y todos sus subdirectorios, incluidos todos los archivos). |
| /q | Especifica el modo silencioso. No solicita confirmación al eliminar un árbol de directorios. El parámetro **/q** solo funciona si se especifica **/s** .<p>**PRECAUCIÓN:** Cuando se ejecuta en modo silencioso, se elimina el árbol de directorios completo sin confirmación. Asegúrese de que se mueven o se realizan copias de seguridad de los archivos importantes antes de usar la opción de línea de comandos **/q** . |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- No se puede eliminar un directorio que contenga archivos, incluidos los archivos ocultos o del sistema. Si intenta hacerlo, aparece el siguiente mensaje:

    `The directory is not empty`

    Use el comando **dir/a** para mostrar todos los archivos (incluidos los archivos ocultos y del sistema). A continuación, use el comando **attrib** con **-h** para quitar atributos de archivo ocultos, **-s** para quitar atributos de archivo de sistema o **-h-s** para quitar los atributos de archivo ocultos y del sistema. Una vez quitados los atributos de archivo y ocultos, puede eliminar los archivos.

- No se puede usar el comando **rmdir** para eliminar el directorio actual. Si intenta eliminar el directorio actual, aparece el siguiente mensaje de error:

    `The process can't access the file because it is being used by another process.`

    Si recibe este mensaje de error, debe cambiar a un directorio diferente (no un subdirectorio del directorio actual) e intentarlo de nuevo.

## <a name="examples"></a>Ejemplos

Para cambiar al directorio principal de modo que pueda quitar de forma segura el directorio deseado, escriba:

```
cd ..
```

Para quitar un directorio denominado *Test* (y todos sus subdirectorios y archivos) del directorio actual, escriba:

```
rmdir /s test
```

Para ejecutar el ejemplo anterior en modo silencioso, escriba:

```
rmdir /s /q test
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
