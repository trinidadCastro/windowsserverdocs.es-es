---
title: rename
description: Artículo de referencia para el comando Rename, que cambia el nombre de un archivo o un directorio.
ms.topic: article
ms.assetid: 7f2ea658-0fa9-4015-8031-22c2b0089231
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a2ab634be010f470314658b25daac92c00d4706c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883797"
---
# <a name="rename"></a>rename

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el nombre de los archivos o directorios.

> [!NOTE]
> Este comando es el mismo que el [comando Ren](ren.md).

## <a name="syntax"></a>Sintaxis

```
rename [<drive>:][<path>]<filename1> <filename2>
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
rename *.txt *.doc
```

Para cambiar el nombre de un directorio de *Chap10* a *part10*, escriba:

```
rename chap10 part10
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Ren (comando)](ren.md)
