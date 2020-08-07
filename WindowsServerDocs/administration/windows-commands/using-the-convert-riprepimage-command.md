---
title: Convert-RiprepImage
description: Artículo de referencia sobre Convert-RiprepImage, que convierte una imagen existente de preparación de la instalación remota (RIPrep) en el formato de imagen de Windows (. wim).
ms.topic: article
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e4a10c4013594c4b6ea19463cf41836f251127a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896580"
---
# <a name="convert-riprepimage"></a>Convert-RiprepImage

Convierte una imagen existente de preparación de la instalación remota (RIPrep) en el formato de imagen de Windows (. wim).

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /Convert-RIPrepImage /FilePath:<File path and name>
     /DestinationImage
         /FilePath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
         [/InPlace]
         [/Overwrite:{Yes | No | Append}]
```

### <a name="parameters"></a>Parámetros

|            Parámetro            |                                                                                                                                                                                                                                                                                                               Descripción                                                                                                                                                                                                                                                                                                                |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| FilePath\<File path and name> |                                                                                                                                                                                                       Especifica la ruta de acceso completa y el nombre de archivo del archivo. sif correspondiente a la imagen RIPrep. Este archivo se denomina normalmente Riprep. sif y se encuentra en la subcarpeta \Templates de la carpeta que contiene la imagen RIPrep.                                                                                                                                                                                                       |
|        /DestinationImage        | Especifica los valores para la imagen de destino, con las siguientes opciones.</br>-/FilePath: \<File path and name> -establece la ruta de acceso completa del archivo para el nuevo archivo. Por ejemplo: **C:\Temp\convert.Wim**</br>-[/Name: \<Name> ]: establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre para mostrar, se usará el nombre para mostrar de la imagen de origen.</br>-[/Description: \<Description> ]-establece la descripción de la imagen.</br>-[/InPlace]: especifica que la conversión debe realizarse en la imagen original RIPrep y no en una copia de la imagen original, que es el comportamiento predeterminado.</br>-[/Overwrite: {Yes |

## <a name="examples"></a>Ejemplos

Para convertir la imagen RIPrep. sif especificada en RIPREP. Wim, escriba:
```
WDSUTIL /Convert-RiPrepImage /FilePath:R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif /DestinationImage
/FilePath:C:\Temp\RIPREP.wim
```
Para convertir la imagen RIPrep. sif especificada en RIPREP. Wim con el nombre y la descripción especificados y sobrescribirla con el nuevo archivo si ya existe un archivo, escriba:
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif
/DestinationImage /FilePath:\\Server\Share\RIPREP.wim
/Name:WindowsXP image /Description:Converted RIPREP image of WindowsXP
/Overwrite:Append
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)