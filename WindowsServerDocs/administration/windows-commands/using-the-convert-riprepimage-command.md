---
title: Usar el comando Convert-RiprepImage
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0b5e75a4148359db5088e7ff60d29b8d157309d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363647"
---
# <a name="using-the-convert-riprepimage-command"></a>Usar el comando Convert-RiprepImage



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

## <a name="parameters"></a>Parámetros

|            Parámetro            |                                                                                                                                                                                                                                                                                                               Descripción                                                                                                                                                                                                                                                                                                                |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /FilePath: ruta de acceso y nombre de la @no__t 0File > |                                                                                                                                                                                                       Especifica la ruta de acceso completa y el nombre de archivo del archivo. sif correspondiente a la imagen RIPrep. Este archivo se denomina normalmente Riprep. sif y se encuentra en la subcarpeta \Templates de la carpeta que contiene la imagen RIPrep.                                                                                                                                                                                                       |
|        /DestinationImage        | Especifica los valores para la imagen de destino, con las siguientes opciones.</br>-/FilePath: \<File ruta de acceso y nombre >: establece la ruta de acceso completa del archivo para el nuevo archivo. Por ejemplo: **C:\Temp\convert.wim**</br>-[/Name: \<Name >]: establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre para mostrar, se usará el nombre para mostrar de la imagen de origen.</br>-[/Description: \<Description >]: establece la descripción de la imagen.</br>-[/InPlace]: especifica que la conversión debe realizarse en la imagen original RIPrep y no en una copia de la imagen original, que es el comportamiento predeterminado.</br>-[/Overwrite: {Yes |

## <a name="BKMK_examples"></a>Example

Para convertir la imagen RIPrep. sif especificada en RIPREP. Wim, escriba:
```
WDSUTIL /Convert-RiPrepImage /FilePath:"R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif" /DestinationImage
/FilePath:"C:\Temp\RIPREP.wim"
```
Para convertir la imagen RIPrep. sif especificada en RIPREP. Wim con el nombre y la descripción especificados y sobrescribirla con el nuevo archivo si ya existe un archivo, escriba:
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:"\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif"
/DestinationImage /FilePath:"\\Server\Share\RIPREP.wim"
/Name:"WindowsXP image" /Description:"Converted RIPREP image of WindowsXP"
/Overwrite:Append
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)