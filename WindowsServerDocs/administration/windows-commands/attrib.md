---
title: attrib
description: Artículo de referencia para el comando attrib, que muestra, establece o quita atributos asignados a archivos o directorios.
ms.topic: reference
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1bf34ad853421d395a76e27313d92330922ba2d0
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633315"
---
# <a name="attrib"></a>attrib

Muestra, establece o quita atributos asignados a archivos o directorios. Si se usa sin parámetros, **attrib** muestra los atributos de todos los archivos del directorio actual.

## <a name="syntax"></a>Sintaxis

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<drive>:][<path>][<filename>] [/s [/d] [/l]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `{+|-}r` | Establece ( **+** ) o borra ( **-** ) el atributo de archivo de solo lectura. |
| `{+\|-}a` | Establece ( **+** ) o borra ( **-** ) el atributo de archivo de almacenamiento. Este conjunto de atributos marca los archivos que han cambiado desde la última vez que se realizó una copia de seguridad. Tenga en cuenta que el comando **xcopy** usa atributos de archivo. |
| `{+\|-}s` | Establece ( **+** ) o borra ( **-** ) el atributo de archivo del sistema. Si un archivo utiliza este conjunto de atributos, debe borrar el atributo antes de poder cambiar cualquier otro atributo del archivo. |
| `{+\|-}h` | Establece ( **+** ) o borra ( **-** ) el atributo de archivo oculto. Si un archivo utiliza este conjunto de atributos, debe borrar el atributo antes de poder cambiar cualquier otro atributo del archivo. |
| `{+\|-}i` | Establece ( **+** ) o borra ( **-** ) el atributo de archivo no indizado de contenido. |
| `[<drive>:][<path>][<filename>]` | Especifica la ubicación y el nombre del directorio, el archivo o el grupo de archivos para los que desea mostrar o cambiar atributos.<p>Puede **usar el** y **&#42;** caracteres comodín en el parámetro *filename* para mostrar o cambiar los atributos de un grupo de archivos. |
| /s | Aplica el **atributo attrib** y las opciones de línea de comandos a los archivos coincidentes en el directorio actual y en todos sus subdirectorios. |
| /d | Aplica el **atributo attrib** y las opciones de línea de comandos a los directorios. |
| /l | Aplica el **atributo attrib** y las opciones de línea de comandos al vínculo simbólico, en lugar del destino del vínculo simbólico. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para mostrar los atributos de un archivo denominado News86 que se encuentra en el directorio actual, escriba:

```
attrib news86
```

Para asignar el atributo de solo lectura al archivo denominado report.txt, escriba:

```
attrib +r report.txt
```

Para quitar el atributo de solo lectura de los archivos del directorio público y sus subdirectorios en un disco de la unidad b:, escriba:

```
attrib -r b:\public\*.* /s
```

Para establecer el atributo de archivo para todos los archivos de la unidad a:, y luego borrar el atributo de archivo para los archivos con la extensión. bak, escriba:

```
attrib +a a:*.* & attrib -a a:*.bak
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando xcopy](xcopy.md)
