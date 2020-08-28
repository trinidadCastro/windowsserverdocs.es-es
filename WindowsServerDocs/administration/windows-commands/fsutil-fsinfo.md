---
title: Fsutil fsinfo
description: Artículo de referencia del comando fsutil fsinfo, en el que se enumeran todas las unidades, se consulta el tipo de unidad, se consulta información de volumen, se consulta información de volumen específica de NTFS o se consultan las estadísticas del sistema de archivos.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: e921b8572b7d1d87a1baf40cfdbc955adce3ec01
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037373"
---
# <a name="fsutil-fsinfo"></a>fsutil fsinfo

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Muestra todas las unidades, consulta el tipo de unidad, consulta la información del volumen, consulta información de volumen específica de NTFS o consulta estadísticas del sistema de archivos.

## <a name="syntax"></a>Sintaxis

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <volumepath>
fsutil fsinfo [ntfsinfo] <rootpath>
fsutil fsinfo [statistics] <volumepath>
fsutil fsinfo [volumeinfo] <rootpath>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| unidades | Muestra todas las unidades del equipo. |
| Drivetype | Consulta una unidad y muestra su tipo, por ejemplo, la unidad de CD-ROM. |
| ntfsinfo | Muestra información de volumen específica de NTFS para el volumen especificado, como el número de sectores, clústeres totales, clústeres libres y el inicio y el final de la zona MFT. |
| sectorinfo | Muestra información sobre el tamaño y la alineación del sector del hardware. |
| estadísticas | Muestra las estadísticas del sistema de archivos para el volumen especificado, como metadatos, archivo de registro y lecturas y escrituras de MFT. |
| volumeinfo | Muestra información para el volumen especificado, como el sistema de archivos, y si el volumen admite nombres de archivo que distinguen mayúsculas de minúsculas, Unicode en nombres de archivo, cuotas de disco o es un volumen de DirectAccess (DAX). |
| `<volumepath>:` | Especifica la letra de unidad (seguida de dos puntos). |
| `<rootpath>:` | Especifica la letra de unidad (seguida de dos puntos) de la unidad raíz. |

### <a name="examples"></a>Ejemplos

Para enumerar todas las unidades del equipo, escriba:

```
fsutil fsinfo drives
```

El resultado es similar al que se muestra a continuación:

```
Drives: A:\ C:\ D:\ E:\
```

Para consultar el tipo de unidad de la unidad C, escriba:

```
fsutil fsinfo drivetype c:
```

Entre los posibles resultados de la consulta se incluyen:

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

Para consultar la información del volumen E, escriba:

```
fsinfo volumeinfo e:\
```

El resultado es similar al que se muestra a continuación:

```
Volume Name : Volume
Serial Number : 0xd0b634d9
Max Component Length : 255
File System Name : NTFS
Supports Named Streams
Is DAX Volume
```

Para consultar la información de volumen específica de NTFS en la unidad F, escriba:

```
fsutil fsinfo ntfsinfo f:
```

El resultado es similar al que se muestra a continuación:

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors : 0x00000000010ea04f
Total Clusters : 0x000000000021d409
Mft Zone End : 0x0000000000004700
```

Para consultar el hardware subyacente del sistema de archivos para obtener información del sector, escriba:

```
fsinfo sectorinfo d:
```

El resultado es similar al que se muestra a continuación:

```
D:\>fsutil fsinfo sectorinfo d:
LogicalBytesPerSector : 4096
PhysicalBytesPerSectorForAtomicity : 4096
Trim Not Supported
DAX capable
```

Para consultar las estadísticas del sistema de archivos de la unidad E, escriba:

```
fsinfo statistics e:
```

El resultado es similar al que se muestra a continuación:

```
File System Type : NTFS
Version : 1
UserFileReads : 75021
UserFileReadBytes : 1305244512
LogFileWriteBytes : 180936704
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)
