---
title: ren
description: Artículo de referencia para el comando Ren, que cambia el nombre de un archivo o directorio.
ms.topic: reference
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 751fa94d760d5fe1f49ceedb3ddeeac2656e4487
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641023"
---
# <a name="ren"></a>ren

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el nombre de los archivos o directorios.

> [!NOTE]
> Este comando es el mismo que el [comando Rename](rename.md).

## <a name="syntax"></a>Sintaxis

```
ren [<drive>:][<path>]<filename1> <filename2>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[<drive>:][<path>]<filename1>` | Especifica la ubicación y el nombre del archivo o conjunto de archivos a los que desea cambiar el nombre. *NombreDeArchivo1* puede incluir caracteres comodín (**&#42;** y **?**). |
| `<filename2>` | Especifica el nuevo nombre del archivo. Puede usar caracteres comodín para especificar nuevos nombres para varios archivos. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- No se puede especificar una nueva unidad o ruta de acceso al cambiar el nombre de los archivos. Tampoco puede usar este comando para cambiar el nombre de los archivos entre las unidades o para trasladarlos a un directorio diferente.

- Los caracteres representados por caracteres comodín en *Nombredearchivo2* serán idénticos a los caracteres correspondientes de *nombreDeArchivo1*.

- *Nombredearchivo2* debe ser un nombre de archivo único. Si *Nombredearchivo2* coincide con un nombre de archivo existente, aparece el siguiente mensaje: `Duplicate file name or file not found` .

### <a name="examples"></a>Ejemplos

Para cambiar todas las extensiones de nombre de archivo. txt del directorio actual a extensiones. doc, escriba:

```
ren *.txt *.doc
```

Para cambiar el nombre de un directorio de *Chap10* a *part10*, escriba:

```
ren chap10 part10
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Rename (comando)](rename.md)
