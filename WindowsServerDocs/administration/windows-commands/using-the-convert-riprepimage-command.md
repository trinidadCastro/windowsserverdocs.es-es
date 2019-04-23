---
title: Con el comando convert-RiprepImage
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cf5fffdedbc25ad97e9e96a84d3ff1bbdaf87b2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835056"
---
# <a name="using-the-convert-riprepimage-command"></a>Con el comando convert-RiprepImage



Convierte una imagen de preparación de instalación remota (RIPrep) existente al formato de imagen de Windows (.wim).

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

|Parámetro|Descripción|
|---------|-----------|
|/ FilePath:\<ruta de acceso y nombre del archivo >|Especifica la ruta de acceso y el nombre completo del archivo que se corresponde con la imagen RIPrep. Este archivo suele llamarse Riprep.sif y se encuentra en la subcarpeta \Templates de la carpeta que contiene la imagen RIPrep.|
|/DestinationImage|Especifica la configuración de la imagen de destino, mediante las siguientes opciones.</br>-/ FilePath:\<ruta de acceso y nombre del archivo >-establece la ruta de acceso completa para el nuevo archivo. Por ejemplo: **C:\Temp\convert.wim**</br>-[/ Name:\<nombre >]-establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre para mostrar, se usará el nombre para mostrar de la imagen de origen.</br>-[/ Descripción: \<Descripción >]-establece la descripción de la imagen.</br>-[/InPlace]: Especifica que la conversión debe tener lugar en la imagen RIPrep original y no en una copia de la imagen original, que es el comportamiento predeterminado.</br>-   [/Overwrite:{Yes | No | Append}]: determina si el archivo especificado en el **/DestinationImage** opción se debe sobrescribir si ya existe un archivo existente con ese nombre en/filepath. **Sí** sobrescribe el archivo existente. **No** (valor predeterminado) provoca un error si ya existe otro archivo con el mismo nombre. **Anexar** adjunta la imagen generada como una nueva imagen en el archivo .wim existente.|

## <a name="BKMK_examples"></a>Ejemplos

Para convertir la imagen RIPrep.sif especificada en RIPREP.wim, escriba:
```
WDSUTIL /Convert-RiPrepImage /FilePath:"R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif" /DestinationImage
/FilePath:"C:\Temp\RIPREP.wim"
```
Para convertir la imagen RIPrep.sif especificada en RIPREP.wim con el nombre especificado y la descripción y sobrescribirlo con el nuevo archivo si ya existe un archivo, escriba:
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:"\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif"
/DestinationImage /FilePath:"\\Server\Share\RIPREP.wim"
/Name:"WindowsXP image" /Description:"Converted RIPREP image of WindowsXP"
/Overwrite:Append
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)