---
title: convert
description: Temas de comandos de Windows para convertir, que convierte los volúmenes de tabla de asignación de archivos (FAT) y FAT32 en el sistema de archivos NTFS, lo que deja intactos los archivos y directorios existentes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 96e437c0-1aa3-46ab-9078-a7b8cdaf3792
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fb2981d6cd5a54737700b64b28f7a8a52de72b1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847178"
---
# <a name="convert"></a>convert

Convierte los volúmenes de tabla de asignación de archivos (FAT) y FAT32 en el sistema de archivos NTFS, lo que deja intactos los archivos y directorios existentes. Los volúmenes convertidos en el sistema de archivos NTFS no se pueden volver a convertir a FAT o FAT32.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
convert [<Volume>] /fs:ntfs [/v] [/cvtarea:<FileName>] [/nosecurity] [/x]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<> de volumen|Especifica la letra de unidad (seguida de dos puntos), el punto de montaje o el nombre del volumen que se va a convertir en NTFS.|
|/FS: NTFS|Obligatorio. Convierte el volumen en NTFS.|
|/v|Ejecuta la **conversión** en modo detallado, que muestra todos los mensajes durante el proceso de conversión.|
|/Cvtarea:\<nombre de archivo >|Especifica que la tabla de archivos maestros (MFT) y otros archivos de metadatos NTFS se escriben en un archivo de marcador de posición contiguo y existente. Este archivo debe estar en el directorio raíz del sistema de archivos que se va a convertir. El uso del parámetro **/cvtarea** puede dar lugar a un sistema de archivos menos fragmentado después de la conversión. Para obtener los mejores resultados, el tamaño de este archivo debe ser 1 KB multiplicado por el número de archivos y directorios del sistema de archivos, aunque la utilidad de **conversión** acepta archivos de cualquier tamaño.</br>Importante: debe crear el archivo de marcador de posición con el comando **fsutil File CreateNew** antes de ejecutar **Convert**. La **conversión** no crea este archivo automáticamente. **Convert** sobrescribe este archivo con los metadatos de NTFS. Después de la conversión, se libera cualquier espacio no utilizado en este archivo.|
|/nosecurity|Especifica que la configuración de seguridad de los archivos y directorios convertidos permite el acceso de todos los usuarios.|
|/x|Desmonta el volumen, si es necesario, antes de que se convierta. Todos los controladores abiertos en el volumen ya no serán válidos.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Si **Convert** no puede bloquear la unidad (por ejemplo, la unidad es el volumen del sistema o la unidad actual), se le ofrece la opción de convertir la unidad la próxima vez que reinicie el equipo. Si no puede reiniciar el equipo inmediatamente para completar la conversión, planee una hora para reiniciar el equipo y deje tiempo adicional para que se complete el proceso de conversión.
-   Para volúmenes convertidos de FAT o FAT32 a NTFS:

    Debido al uso existente del disco, la MFT se crea en una ubicación diferente a la de un volumen originalmente formateado con NTFS, por lo que el rendimiento del volumen podría no ser tan bueno como en los volúmenes con formato original con NTFS. Para obtener un rendimiento óptimo, considere la posibilidad de volver a crear estos volúmenes y darles formato con el sistema de archivos NTFS.

    La conversión de volumen de FAT o FAT32 a NTFS deja los archivos intactos, pero es posible que el volumen no tenga algunas ventajas de rendimiento en comparación con los volúmenes formateados inicialmente con NTFS. Por ejemplo, el MFT se puede fragmentar en los volúmenes convertidos. Además, en los volúmenes de arranque convertidos, la **conversión** aplica la misma seguridad predeterminada que se aplica durante la instalación de Windows.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para convertir el volumen de la unidad E a NTFS y Mostrar todos los mensajes durante el proceso de conversión, escriba:
```
convert e: /fs:ntfs /v
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)