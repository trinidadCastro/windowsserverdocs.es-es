---
title: mover
description: Artículo de referencia del comando move, que mueve uno o más archivos de un directorio a otro.
ms.topic: reference
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: cd68c7bcfa1f33eeb977a46a1687bbbab678420f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640569"
---
# <a name="move"></a>mover

Mueve uno o más archivos de un directorio a otro.

> [!IMPORTANT]
> Si se mueven archivos cifrados a un volumen que no sea compatible con los resultados de Sistema de cifrado de archivos (EFS), se producirá un error. Primero debe descifrar los archivos o moverlos a un volumen que admita EFS.

## <a name="syntax"></a>Sintaxis

```
move [{/y|-y}] [<source>] [<target>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /y | Detiene la solicitud de confirmación de que desea sobrescribir un archivo de destino existente. Este parámetro puede estar preestablecido en la variable de entorno COPYCMD. Puede invalidar este valor predeterminado mediante el parámetro **-y** . El valor predeterminado es preguntar antes de sobrescribir archivos, a menos que el comando se ejecute desde un script de batch. |
| -y | Inicia la solicitud de confirmación de que desea sobrescribir un archivo de destino existente. |
| `<source>` | Especifica la ruta de acceso y el nombre de los archivos que se van a trasladar. Para quitar o cambiar el nombre de un directorio, el *origen* debe ser la ruta de acceso y el nombre del directorio actual. |
| `<target>` | Especifica la ruta de acceso y el nombre al que se van a trasladar los archivos. Para quitar o cambiar el nombre de un directorio, el *destino* debe ser la ruta de acceso y el nombre del directorio deseado. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para migrar todos los archivos con la extensión. xls del directorio *\data* al directorio *\ Second_Q \reports* , escriba:

```
move \data\*.xls \second_q\reports\
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
