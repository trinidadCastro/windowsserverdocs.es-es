---
title: etiqueta
description: Artículo de referencia para el comando etiqueta, que crea, cambia o elimina la etiqueta de volumen (es decir, el nombre) de un disco.
ms.topic: reference
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 70e26a89d679c499dbe0eaa7fcd04aa4b9994e98
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636598"
---
# <a name="label"></a>etiqueta

Crea, cambia o elimina la etiqueta de volumen (es decir, el nombre) de un disco. Si se usa sin parámetros, el comando **Label** cambia la etiqueta de volumen actual o elimina la etiqueta existente.

## <a name="syntax"></a>Sintaxis

```
label [/mp] [<volume>] [<label>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /mp | Especifica que el volumen se debe tratar como un punto de montaje o un nombre de volumen. |
| `<volume>` | Especifica una letra de unidad (seguida de un signo de dos puntos), un punto de montaje o un nombre de volumen. Si se especifica un nombre de volumen, el parámetro **/MP** no es necesario. |
| `<label>` | Especifica la etiqueta del volumen. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Observaciones

- Windows muestra la etiqueta de volumen y el número de serie (si tiene uno) como parte de la lista de directorios.

- Una etiqueta de volumen NTFS puede tener una longitud de hasta 32 caracteres, incluidos espacios. Las etiquetas de volumen NTFS conservan y muestran el caso que se usó cuando se creó la etiqueta.

## <a name="examples"></a>Ejemplos

Para etiquetar un disco en la unidad A que contiene información de ventas de julio, escriba:

```
label a:sales-july
```

Para ver y eliminar la etiqueta actual de la unidad C, siga estos pasos:

1. En el símbolo del sistema, escriba:

   ```
   label
   ```

   Debe mostrarse una salida similar a la siguiente:

   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```

2. Presione ENTRAR. Se debe mostrar el siguiente mensaje:

   ```
   Delete current volume label (Y/N)?
   ```

3. Presione **Y** para eliminar la etiqueta actual o **N** si desea conservar la etiqueta existente.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)