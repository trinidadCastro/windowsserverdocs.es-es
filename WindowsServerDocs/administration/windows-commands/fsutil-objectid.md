---
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
title: fsutil objectid
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 2f5887f20e2c36ec7dcfcd6f4e920b5273c6c60c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813746"
---
# <a name="fsutil-objectid"></a>fsutil objectid
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Administra los identificadores de objeto (OID), que son objetos internos que usa el servicio cliente de seguimiento de vínculos distribuidos (DLT) y el servicio de replicación de archivos (FRS) para realizar un seguimiento de otros objetos como archivos, directorios y vínculos. Los identificadores de objeto son invisibles para la mayoría de los programas y nunca se deben modificar.

> [!CAUTION]
> No eliminar, establecer o modificar un identificador de objeto. Eliminar o establecer un identificador de objeto puede provocar la pérdida de datos de partes de un archivo, hasta e incluyendo volúmenes completos de datos. Además, se podría provocar un comportamiento adverso en el servicio de seguimiento de vínculos distribuidos (DLT) cliente y servicio de replicación de archivos (FRS).

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil objectid [create] <FileName>
fsutil objectid [delete] <FileName>
fsutil objectid [query] <FileName>
fsutil objectid [set] <ObjectID> <BirthVolumeID> <BirthObjectID> <DomainID> <FileName>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|crear|Crea un identificador de objeto si el archivo especificado no tiene ya una. Si el archivo ya tiene un identificador de objeto, este subcomando es equivalente a la **consulta** subcomando.|
|Eliminar|Elimina un identificador de objeto.|
|query|Consulta un identificador de objeto.|
|set|Establece un identificador de objeto.|
|\<ObjectID>|Establece un identificador hexadecimal de 16 bytes de archivo específico que se garantiza que sea único dentro de un volumen. El identificador de objeto se usa el servicio de cliente de seguimiento de vínculos distribuidos (DLT) y el servicio de replicación de archivos (FRS) para identificar los archivos.|
|\<BirthVolumeID>|Indica el volumen en el que se encontró el archivo cuando primero obtiene un identificador de objeto. Este valor es un identificador hexadecimal de 16 bytes que se usa el servicio cliente DLT.|
|\<BirthObjectID>|Indica el identificador de objeto original del archivo (el *ObjectID* pueden cambiar cuando se mueve un archivo). Este valor es un identificador hexadecimal de 16 bytes que se usa el servicio cliente DLT.|
|\<DomainID>|identificador de dominio hexadecimal de 16 bytes. Este valor no se utiliza actualmente y se debe establecer en todo ceros.|
|\<FileName>|Especifica la ruta de acceso completa al archivo incluido el nombre de archivo y la extensión, por ejemplo C:\documents\filename.txt.|

## <a name="remarks"></a>Comentarios

-   Cualquier archivo que tenga un identificador de objeto también tiene un identificador de volumen de nacimiento, un identificador de objeto de nacimiento y un identificador de dominio. Cuando mueve un archivo, puede cambiar el identificador de objeto, pero el volumen de nacimiento y los identificadores de objeto de nacimiento que siguen siendo los mismos. Este comportamiento permite que el sistema operativo de Windows siempre se encuentra un archivo, independientemente de donde se ha movido.

## <a name="BKMK_examples"></a>Ejemplos
Para crear un identificador de objeto, escriba:

`fsutil objectid create c:\temp\sample.txt`

Para eliminar un identificador de objeto, escriba:

`fsutil objectid delete c:\temp\sample.txt`

Para consultar un identificador de objeto, escriba:

`fsutil objectid query c:\temp\sample.txt`

Para establecer un identificador de objeto, escriba:

`fsutil objectid set 40dff02fc9b4d4118f120090273fa9fc f86ad6865fe8d21183910008c709d19e 40dff02fc9b4d4118f120090273fa9fc 00000000000000000000000000000000 c:\temp\sample.txt`

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


