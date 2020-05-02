---
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
title: Fsutil fsinfo
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b7af3859cd16b89587a86e3436d5c832620c4e22
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725494"
---
# <a name="fsutil-fsinfo"></a>Fsutil fsinfo
> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2 y Windows 7

Muestra todas las unidades, consulta el tipo de unidad, consulta la información del volumen, consulta información de volumen específica de NTFS o consulta estadísticas del sistema de archivos.



## <a name="syntax"></a>Sintaxis

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <VolumePath>
fsutil fsinfo [ntfsinfo] <RootPath>
fsutil fsinfo [statistics] <VolumePath>
fsutil fsinfo [volumeinfo] <RootPath>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|unidades|Muestra todas las unidades del equipo.|
|Drivetype|Consulta una unidad y muestra su tipo, por ejemplo, la unidad de CD-ROM.|
|ntfsinfo|Muestra información de volumen específica de NTFS para el volumen especificado, como el número de sectores, clústeres totales, clústeres libres y el inicio y el final de la zona MFT.|
|sectorinfo|Muestra información sobre el tamaño y la alineación del sector del hardware.|
|estadísticas|Muestra las estadísticas del sistema de archivos para el volumen especificado, como metadatos, archivo de registro y lecturas y escrituras de MFT.|
|volumeinfo|Muestra información para el volumen especificado, como el sistema de archivos, y si el volumen admite nombres de archivo que distinguen mayúsculas de minúsculas, Unicode en nombres de archivo, cuotas de disco o es un volumen de DirectAccess (DAX).|
|< "VolumePath" >|Especifica la letra de unidad (seguida de dos puntos).|
|< "RootPathname" >|Especifica la letra de unidad (seguida de dos puntos) de la unidad raíz.|

## <a name="examples"></a><a name="BKMK_examples"></a>Ejemplos
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

Para consultar la información de volumen específica de NTFS en la unidad F, escriba:

```
fsutil fsinfo ntfsinfo f:
```

El resultado es similar al que se muestra a continuación:

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors :            0x00000000010ea04f
Total Clusters :            0x000000000021d409
.
.
.
Mft Zone End   :            0x0000000000004700       
```

Para consultar el hardware subyacente del sistema de archivos para obtener información del sector, escriba:

```
fsinfo sectorinfo d:
```

El resultado es similar al que se muestra a continuación:

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

Para consultar las estadísticas del sistema de archivos de la unidad E, escriba:

```
fsinfo statistics e:
```

El resultado es similar al que se muestra a continuación:

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

## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[fsutil](Fsutil.md)


