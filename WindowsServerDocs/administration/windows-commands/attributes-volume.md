---
title: attributes volume
description: Artículo de referencia para el comando de volumen Attributes, que muestra, establece o borra los atributos de un volumen.
ms.topic: reference
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dd651732dbf537da31ae5f5343c687868ffb2741
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633283"
---
# <a name="attributes-volume"></a>attributes volume

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra, establece o borra los atributos de un volumen.

## <a name="syntax"></a>Sintaxis

```
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ------- | -------- |
| set | Establece el atributo especificado del volumen que tiene el foco. |
| clear | Borra el atributo especificado del volumen que tiene el foco. |
| readonly | Especifica que el volumen es de sólo lectura. |
| hidden | Especifica que el volumen está oculto. |
| nodefaultdriveletter | Especifica que el volumen no recibe una letra de unidad de forma predeterminada. |
| shadowcopy | Especifica que el volumen es un volumen de instantánea. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

### <a name="remarks"></a>Observaciones

- En los discos básicos de registro de arranque maestro (MBR), los parámetros **Hidden**, **ReadOnly**y **nodefaultdriveletter** se aplican a todos los volúmenes del disco.

- En los discos básicos de tabla de particiones GUID (GPT) y en discos MBR y GPT dinámicos, los parámetros **Hidden**, **ReadOnly**y **nodefaultdriveletter** solo se aplican al volumen seleccionado.

- Se debe seleccionar un volumen para que el comando de **volumen de atributos** se ejecute correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.

## <a name="examples"></a>Ejemplos

Para mostrar los atributos actuales en el volumen seleccionado, escriba:

```
attributes volume
```

Para establecer el volumen seleccionado como oculto y de solo lectura, escriba:

```
attributes volume set hidden readonly
```

Para quitar los atributos oculto y de solo lectura del volumen seleccionado, escriba:

```
attributes volume clear hidden readonly
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Seleccionar volumen](select-volume.md)
