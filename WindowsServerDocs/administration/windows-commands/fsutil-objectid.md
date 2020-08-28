---
title: fsutil objectid
description: Artículo de referencia para el comando fsutil objectId, que administra los identificadores de objeto para realizar el seguimiento de otros objetos, como archivos, directorios y vínculos.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 27b7048ebb659c29bd6aa7d41c0be9b26deca547
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037343"
---
# <a name="fsutil-objectid"></a>fsutil objectid

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Administra identificadores de objeto (OID), que son objetos internos utilizados por el servicio de cliente de seguimiento de vínculos distribuidos (DLT) y el servicio de replicación de archivos (FRS), para realizar el seguimiento de otros objetos, como archivos, directorios y vínculos. Los identificadores de objeto son invisibles para la mayoría de los programas y nunca deben modificarse.

> [!WARNING]
> No elimine, establezca ni modifique ningún identificador de objeto. La eliminación o la configuración de un identificador de objeto puede dar lugar a la pérdida de datos de partes de un archivo, hasta incluir volúmenes completos de datos. Además, podría producirse un comportamiento adverso en el servicio de cliente de seguimiento de vínculos distribuidos (DLT) y en el servicio de replicación de archivos (FRS).

## <a name="syntax"></a>Sintaxis

```
fsutil objectid [create] <filename>
fsutil objectid [delete] <filename>
fsutil objectid [query] <filename>
fsutil objectid [set] <objectID> <birthvolumeID> <birthobjectID> <domainID> <filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| create | Crea un identificador de objeto si el archivo especificado todavía no tiene uno. Si el archivo ya tiene un identificador de objeto, este subcomando es equivalente al subcomando **query** . |
| delete | Elimina un identificador de objeto. |
| Query | Consulta un identificador de objeto. |
| set | Establece un identificador de objeto. |
| `<objectID>` | Establece un identificador hexadecimal de 16 bytes específico del archivo que se garantiza que es único dentro de un volumen. El servicio de cliente de seguimiento de vínculos distribuidos (DLT) y el servicio de replicación de archivos (FRS) utilizan el identificador de objetos para identificar los archivos. |
| `<birthvolumeID>` | Indica el volumen en el que se encontraba el archivo cuando se obtuvo por primera vez un identificador de objeto. Este valor es un identificador hexadecimal de 16 bytes que usa el servicio cliente DLT. |
| `<birthobjectID>` | Indica el identificador de objeto original del archivo ( *objectId* puede cambiar cuando se mueve un archivo). Este valor es un identificador hexadecimal de 16 bytes que usa el servicio cliente DLT. |
| `<domainID>` | identificador de dominio hexadecimal de 16 bytes. Este valor no se utiliza actualmente y debe establecerse en ceros. |
| `<filename>` | Especifica la ruta de acceso completa al archivo, incluido el nombre de archivo y la extensión, por ejemplo *C:\documents\filename.txt*. |

#### <a name="remarks"></a>Observaciones

- Cualquier archivo que tenga un identificador de objeto también tiene un identificador de volumen de nacimiento, un identificador de objeto de nacimiento y un identificador de dominio. Cuando se mueve un archivo, el identificador del objeto puede cambiar, pero los identificadores del volumen de nacimiento y del objeto de nacimiento siguen siendo los mismos. Este comportamiento permite que el sistema operativo Windows encuentre siempre un archivo, independientemente de dónde se haya pasado.

### <a name="examples"></a>Ejemplos

Para crear un identificador de objeto, escriba:

`fsutil objectid create c:\temp\sample.txt`

Para eliminar un identificador de objeto, escriba:

`fsutil objectid delete c:\temp\sample.txt`

Para consultar un identificador de objeto, escriba:

`fsutil objectid query c:\temp\sample.txt`

Para establecer un identificador de objeto, escriba:

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)
