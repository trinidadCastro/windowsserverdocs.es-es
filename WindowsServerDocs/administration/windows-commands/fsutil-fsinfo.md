---
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
title: Fsutil fsinfo
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 434dfde2286538367fb96d168b06983cb4357067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873046"
---
# <a name="fsutil-fsinfo"></a>Fsutil fsinfo
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Enumera todas las unidades, consulta el tipo de unidad, información del volumen, consulta la información de volumen específica de NTFS o las estadísticas del sistema de archivos de consulta.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <VolumePath>
fsutil fsinfo [ntfsinfo] <RootPath>
fsutil fsinfo [statistics] <VolumePath>
fsutil fsinfo [volumeinfo] <RootPath>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|unidades de disco|Enumera todas las unidades en el equipo.|
|DriveType|Una unidad de consulta y muestra su tipo, por ejemplo la unidad de CD-ROM.|
|ntfsinfo|Muestra información de volumen específico de NTFS para el volumen especificado, como el número de sectores, total de clústeres, clústeres libres y el inicio y final de la zona MFT.|
|sectorinfo|Muestra información sobre el tamaño de sector de hardware y la alineación.|
|Estadísticas|Muestra estadísticas del sistema para el volumen especificado, como metadatos, el archivo de registro y MFT lecturas y escrituras de archivo.|
|volumeinfo|Muestra información para el volumen especificado, como el sistema de archivos y si el volumen admite nombres de archivo distingue mayúsculas de minúsculas, unicode en nombres de archivo, las cuotas de disco, o es un volumen de DirectAccess (DAX).|
|<"VolumePath">|Especifica la letra de unidad (seguida de dos puntos).|
|<"RootPathname">|Especifica la letra de unidad (seguida de dos puntos) de la unidad raíz.|

## <a name="BKMK_examples"></a>Ejemplos
Para enumerar todas las unidades en el equipo, escriba:

```
fsutil fsinfo drives
```

Salida es similar a la muestra siguiente:

```
Drives: A:\ C:\ D:\ E:\       
```

Para consultar el tipo de unidad de la unidad C, escriba:

```
fsutil fsinfo drivetype c:
```

Los resultados posibles de la consulta incluyen:

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

Para consultar la información de volumen para volumen E, escriba:

```
fsinfo volumeinfo e:\
```

Salida es similar a la muestra siguiente:

```
Volume Name :Volume
Serial Number : 0xd0b634d9
Max Component Length : 255
File System Name : NTFS
.
.
.
Supports Named Streams
Is DAX Volume
```

En la unidad de consulta F para la información de volumen NTFS específica, escriba:

```
fsutil fsinfo ntfsinfo f:
```

Salida es similar a la muestra siguiente:

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors :            0x00000000010ea04f
Total Clusters :            0x000000000021d409
.
.
.
Mft Zone End   :            0x0000000000004700       
```

Para consultar el archivo hardware del sistema subyacente para la información del sector, escriba:

```
fsinfo sectorinfo d:
```

Salida es similar a la muestra siguiente:

```
D:\>fsutil fsinfo sectorinfo d:
LogicalBytesPerSector :                                 4096
PhysicalBytesPerSectorForAtomicity :                    4096
.
.
.
Trim Not Supported
DAX capable
```

Para consultar las estadísticas de la unidad E del sistema de archivo, escriba:

```
fsinfo statistics e:
```

Salida es similar a la muestra siguiente:

```
File System Type :     NTFS
Version :              1
UserFileReads :        75021
UserFileReadBytes :    1305244512
.
.
.
LogFileWriteBytes :    180936704       
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)
[Fsutil](Fsutil.md)


