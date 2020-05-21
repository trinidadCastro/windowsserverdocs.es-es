---
title: extend
description: Tema de referencia del comando Extend, que extiende el volumen o la partición con el foco y su sistema de archivos en un espacio libre (sin asignar) en un disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd10e2ad4d6d647f37e1ad113f1516104e66315f
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437200"
---
# <a name="extend"></a>extend

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Extiende el volumen o la partición con el foco y su sistema de archivos a un espacio libre (sin asignar) en un disco.

## <a name="syntax"></a>Sintaxis

```
extend [size=<n>] [disk=<n>] [noerr]
extend filesystem [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| tamaño =`<n>` | Especifica la cantidad de espacio en megabytes (MB) que se va a agregar al volumen o partición actual. Si no se proporciona ningún tamaño, se usa todo el espacio libre contiguo que está disponible en el disco. |
| disco =`<n>` | Especifica el disco en el que se extiende el volumen o la partición. Si no se especifica ningún disco, el volumen o la partición se amplía en el disco actual. |
| Systems | Extiende el sistema de archivos del volumen que tiene el foco. Para su uso solo en discos en los que el sistema de archivos no se extendió con el volumen. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

#### <a name="remarks"></a>Observaciones

- En los discos básicos, el espacio disponible debe estar en el mismo disco que el volumen o la partición que tiene el foco. También debe seguir inmediatamente el volumen o la partición que tiene el foco (es decir, debe comenzar en el siguiente desplazamiento del sector).

- En los discos dinámicos con volúmenes simples o distribuidos, se puede extender un volumen a cualquier espacio disponible en cualquier disco dinámico. Con este comando, puede convertir un volumen dinámico simple en un volumen dinámico distribuido. Los volúmenes reflejados, RAID-5 y seccionados no se pueden extender.

- Si se formateó la partición anteriormente con el sistema de archivos NTFS, el sistema de archivos se extiende automáticamente para rellenar la partición más grande y no se producirá ninguna pérdida de datos.

- Si se formateó la partición anteriormente con un sistema de archivos distinto de NTFS, el comando produce un error sin cambiar la partición.

- Si no se formateó la partición anteriormente con un sistema de archivos, se seguirá extendiendo la partición.

- La partición debe tener un volumen asociado para poder extenderla.

## <a name="examples"></a>Ejemplos

Para extender el volumen o la partición con el foco en 500 megabytes, en el disco 3, escriba:

```
extend size=500 disk=3
```

Para extender el sistema de archivos de un volumen una vez extendido, escriba:

```
extend filesystem
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
