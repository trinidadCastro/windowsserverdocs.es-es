---
ms.assetid: 693ab895-9d0c-47c1-9f52-df5cd287842a
title: Fsutil objectId
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: e39b36a6c3126429bc47d5b89d104612cab5db96
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844288"
---
# <a name="fsutil-objectid"></a>Fsutil objectId
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Administra los identificadores de objeto (OID), que son objetos internos utilizados por el servicio de cliente de seguimiento de vínculos distribuidos (DLT) y el servicio de replicación de archivos (FRS) para realizar el seguimiento de otros objetos, como archivos, directorios y vínculos. Los identificadores de objeto son invisibles para la mayoría de los programas y nunca deben modificarse.

> [!CAUTION]
> No elimine, establezca ni modifique ningún identificador de objeto. La eliminación o la configuración de un identificador de objeto puede dar lugar a la pérdida de datos de partes de un archivo, hasta incluir volúmenes completos de datos. Además, podría producirse un comportamiento adverso en el servicio de cliente de seguimiento de vínculos distribuidos (DLT) y en el servicio de replicación de archivos (FRS).

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil objectid [create] <FileName>
fsutil objectid [delete] <FileName>
fsutil objectid [query] <FileName>
fsutil objectid [set] <ObjectID> <BirthVolumeID> <BirthObjectID> <DomainID> <FileName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|crear|Crea un identificador de objeto si el archivo especificado todavía no tiene uno. Si el archivo ya tiene un identificador de objeto, este subcomando es equivalente al subcomando **query** .|
|eliminar|Elimina un identificador de objeto.|
|consulta|Consulta un identificador de objeto.|
|set|Establece un identificador de objeto.|
|\<ObjectID >|Establece un identificador hexadecimal de 16 bytes específico del archivo que se garantiza que es único dentro de un volumen. El servicio de cliente de seguimiento de vínculos distribuidos (DLT) y el servicio de replicación de archivos (FRS) utilizan el identificador de objetos para identificar los archivos.|
|\<BirthVolumeID >|Indica el volumen en el que se encontraba el archivo cuando se obtuvo por primera vez un identificador de objeto. Este valor es un identificador hexadecimal de 16 bytes que usa el servicio cliente DLT.|
|\<BirthObjectID >|Indica el identificador de objeto original del archivo ( *objectId* puede cambiar cuando se mueve un archivo). Este valor es un identificador hexadecimal de 16 bytes que usa el servicio cliente DLT.|
|\<DomainID >|identificador de dominio hexadecimal de 16 bytes. Este valor no se utiliza actualmente y debe establecerse en ceros.|
|\<nombre de archivo >|Especifica la ruta de acceso completa al archivo, incluido el nombre de archivo y la extensión, por ejemplo C:\documents\filename.txt.|

## <a name="remarks"></a>Comentarios

-   Cualquier archivo que tenga un identificador de objeto también tiene un identificador de volumen de nacimiento, un identificador de objeto de nacimiento y un identificador de dominio. Cuando se mueve un archivo, el identificador del objeto puede cambiar, pero los identificadores del volumen de nacimiento y del objeto de nacimiento siguen siendo los mismos. Este comportamiento permite que el sistema operativo Windows encuentre siempre un archivo, independientemente de dónde se haya pasado.

## <a name="examples"></a><a name="BKMK_examples"></a>Example
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

[Fsutil](Fsutil.md)


