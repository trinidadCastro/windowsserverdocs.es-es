---
title: convert
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 96e437c0-1aa3-46ab-9078-a7b8cdaf3792
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9fcb7b2190c3359b78145a2ef79a28e30639a89c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873166"
---
# <a name="convert"></a>convert



Convierte archivos (FAT) de la tabla de asignación y los volúmenes al sistema de archivos NTFS, FAT32 dejan intactos los directorios y archivos existentes. No se puede convertir los volúmenes que se convierte en el sistema de archivos NTFS a FAT o FAT32.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
convert [<Volume>] /fs:ntfs [/v] [/cvtarea:<FileName>] [/nosecurity] [/x]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Volume>|Especifica la letra de unidad (seguida de dos puntos), punto de montaje o nombre de volumen para convertir a NTFS.|
|/fs:ntfs|Obligatorio. Convierte el volumen a NTFS.|
|/v|Ejecuciones **convertir** en modo detallado, que muestra todos los mensajes durante el proceso de conversión.|
|/cvtarea:\<FileName>|Especifica que la tabla maestra de archivos (MFT) y otros archivos de metadatos NTFS se escriben en un archivo de marcador de posición existente y contiguos. Este archivo debe estar en el directorio raíz del sistema de archivos que se va a convertir. El uso de la **/Cvtarea** parámetro puede producir en un sistema de archivos menos fragmentado tras la conversión. Para obtener mejores resultados, el tamaño de este archivo debe ser de 1 KB multiplicado por el número de archivos y directorios del sistema de archivos, aunque la **convertir** utilidad acepta archivos de cualquier tamaño.</br>Importante: Debe crear el archivo de marcador de posición con el **fsutil archivo createnew** comando antes de ejecutar **convertir**. **Convertir** no crea este archivo por usted. **Convertir** sobrescribe este archivo con metadatos de NTFS. Después de la conversión, se libera espacio no utilizado en este archivo.|
|/NoSecurity|Especifica que la configuración de seguridad en los directorios y archivos convertidos permite el acceso a todos los usuarios.|
|/x|Desmonta el volumen, si es necesario, antes de que se convierta. Todos los controladores abiertos en el volumen ya no serán válidos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Si **convertir** no se puede bloquear la unidad (por ejemplo, la unidad es el volumen del sistema o la unidad actual), tiene la opción para convertir la unidad en la próxima vez que reinicie el equipo. Si no se puede reiniciar el equipo inmediatamente para completar la conversión, planee un tiempo para reiniciar el equipo y tardarán más tiempo para completar el proceso de conversión.
-   Para volúmenes convierten de FAT o FAT32 a NTFS:

    Debido al uso de disco existente, se crea la MFT en una ubicación diferente que en un volumen que se le dio formateado con NTFS, por lo que volumen rendimiento podría no ser tan buenas como en los volúmenes que se le dio formateados con NTFS. Para obtener un rendimiento óptimo, considere la posibilidad de volver a crear estos volúmenes y darles formato con el sistema de archivos NTFS.

    Conversión de volumen de FAT o FAT32 a NTFS deja intactos los archivos, pero el volumen puede faltar algunas ventajas de rendimiento en comparación con los volúmenes formateados inicialmente con NTFS. Por ejemplo, la MFT podría fragmentarse en los volúmenes convertidos. Además, en los volúmenes de arranque convertido, **convertir** se aplica la misma seguridad predeterminada que se aplica durante la instalación de Windows.

## <a name="BKMK_examples"></a>Ejemplos

Para convertir el volumen en la unidad E: a NTFS y mostrar todos los mensajes durante el proceso de conversión, escriba:
```
convert e: /fs:ntfs /v
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)