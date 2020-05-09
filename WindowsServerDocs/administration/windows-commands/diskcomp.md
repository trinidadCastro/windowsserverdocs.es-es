---
title: diskcomp
description: Tema de referencia para el comando diskcomp, que compara el contenido de dos disquetes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4f56f534-a356-4daa-8b4f-38e089341e42
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fcb810f4cd18d51f8151b27a6f447c86130624fd
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992520"
---
# <a name="diskcomp"></a>diskcomp

Compara el contenido de dos disquetes. Si se utiliza sin parámetros, **Diskcomp** usa la unidad actual para comparar ambos discos.

## <a name="syntax"></a>Sintaxis

```
diskcomp [<drive1>: [<drive2>:]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<drive1>` | Especifica la unidad que contiene uno de los disquetes. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- El comando **Diskcomp** solo funciona con discos de disquete. No se puede usar **Diskcomp** con un disco duro. Si especifica una unidad de disco duro para *unidad1* o *unidad2*, **Diskcomp** muestra el siguiente mensaje de error:

  ```
  Invalid drive specification
  Specified drive does not exist
  or is nonremovable
  ```

- Si todas las pistas de los dos discos que se comparan son las mismas (omite el número de volumen de un disco), **Diskcomp** muestra el mensaje siguiente:

  ```
  Compare OK
  ```

  Si las pistas no son iguales, **Diskcomp** muestra un mensaje similar al siguiente:

  ```
  Compare error on
  side 1, track 2
  ```

  Cuando **Diskcomp** completa la comparación, muestra el siguiente mensaje:

  ```
  Compare another diskette (Y/N)?
  ```

  Si presiona **Y**, **Diskcomp** le pedirá que inserte el disco para la siguiente comparación. Si presiona **N**, **Diskcomp** detiene la comparación.

- Si omite el parámetro *unidad2* , **Diskcomp** usa la unidad actual para *unidad2*. Si omite ambos parámetros de unidad, **Diskcomp** usa la unidad actual para ambos. Si la unidad actual es la misma que la *unidad1*, **Diskcomp** le pedirá que intercambie los discos según sea necesario.

- Si especifica la misma unidad de disquete para *unidad1* y *unidad2*, **Diskcomp** las compara con una unidad y le pide que inserte los discos según sea necesario. Es posible que tenga que cambiar más de una vez los discos, en función de la capacidad de los discos y la cantidad de memoria disponible.

- **Diskcomp** no puede comparar un disco de una sola cara con un disco de doble cara ni un disco de alta densidad con un disco de doble densidad. Si el disco en *unidad1* no es del mismo tipo que el disco en *unidad2*, **Diskcomp** muestra el siguiente mensaje:

  ```
  Drive types or diskette types not compatible
  ```

- **Diskcomp** no funciona en una unidad de red o en una unidad creada por el comando **subst** . Si intenta usar **Diskcomp** con una unidad de cualquiera de estos tipos, **Diskcomp** muestra el siguiente mensaje de error:

  ```
  Invalid drive specification
  ```

- Si usa **Diskcomp** con un disco que ha realizado mediante **copiar**, **Diskcomp** podría mostrar un mensaje similar al siguiente:

  ```
  Compare error on
  side 0, track 0
  ```

  Este tipo de error puede producirse incluso si los archivos de los discos son idénticos. Aunque **Copy** duplica la información, no la coloca necesariamente en la misma ubicación en el disco de destino.

- códigos de salida de **Diskcomp** :

  | Código de salida | Descripción |
  | --------- | ----------- |
  | 0 | Los discos son los mismos |
  | 1 | Se encontraron diferencias |
  | 3 | Error de hardware |
  | 4 | Error de inicialización |

  Para procesar códigos de salida devueltos por **Diskcomp**, puede usar la variable de entorno *ERRORLEVEL* en la línea de comandos **If** en un programa por lotes.

## <a name="examples"></a>Ejemplos

Si el equipo tiene solo una unidad de disquete (por ejemplo, la unidad A) y desea comparar dos discos, escriba:

```
diskcomp a: a:
```

**Diskcomp** le pide que inserte cada disco, según sea necesario.

Ilustra cómo procesar un código de salida de **Diskcomp** en un programa por lotes que utiliza la variable de entorno *ERRORLEVEL* en la línea de comandos **If** :

```
rem Checkout.bat compares the disks in drive A and B
echo off
diskcomp a: b:
if errorlevel 4 goto ini_error
if errorlevel 3 goto hard_error
if errorlevel 1 goto no_compare
if errorlevel 0 goto compare_ok
:ini_error
echo ERROR: Insufficient memory or command invalid
goto exit
:hard_error
echo ERROR: An irrecoverable error occurred
goto exit
:break
echo You just pressed CTRL+C to stop the comparison
goto exit
:no_compare
echo Disks are not the same
goto exit
:compare_ok
echo The comparison was successful; the disks are the same
goto exit
:exit
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
