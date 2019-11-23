---
title: Usar el comando New-Imagendecaptura
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb48cb76ef99eac51b862a5e1a3d1999a1cfc89d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363047"
---
# <a name="using-the-new-captureimage-command"></a>Usar el comando New-Imagendecaptura



Crea una nueva imagen de captura a partir de una imagen de arranque existente. Las imágenes de captura son imágenes de arranque que inician la utilidad de captura de servicios de implementación de Windows en lugar de iniciar el programa de instalación. Cuando se arranca un equipo de referencia (que se ha preparado con Sysprep) en una imagen de captura, un asistente crea una imagen de instalación del equipo de referencia y la guarda como un archivo de imagen de Windows (. wim). También puede Agregar la imagen a un medio (por ejemplo, un CD, un DVD o una unidad USB) y, a continuación, arrancar un equipo desde ese medio. Después de crear la imagen de instalación, puede Agregar la imagen al servidor para la implementación de arranque de PXE. Para obtener más información, consulte crear imágenes ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /New-CaptureImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
        /FilePath:<File path and name>
        [/Name:<Name>]
        [/Description:<Description>]
        [/Overwrite:{Yes | No | Append}]
        [/UnattendFilePath:<File path>]
```

## <a name="parameters"></a>Parámetros

|        Parámetro         |                                                                                                                                                                                                                         Descripción                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<nombre de servidor >] |                                                                                                                                       Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.                                                                                                                                        |
|   /Image:\<nombre de imagen >   |                                                                                                                                                                                                         Especifica el nombre de la imagen de arranque de origen.                                                                                                                                                                                                         |
|   /Architecture: {x86    |                                                                                                                                                                                                                             64                                                                                                                                                                                                                             |
| [/Filename: \<nombre de archivo >] |                                                                                                                                                                            Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.                                                                                                                                                                            |
|    /DestinationImage     | Especifica la configuración de la imagen de destino. La configuración se especifica mediante las siguientes opciones:</br>-/FilePath: \<ruta de acceso y nombre del archivo > establece la ruta de acceso completa del archivo para la nueva imagen de captura.</br>-[/Name: \<nombre >]: establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre para mostrar, se usará el nombre para mostrar de la imagen de origen.</br>-[/Description: \<Descripción >]: establece la descripción de la imagen.</br>-[/Overwrite: {Yes |

## <a name="BKMK_examples"></a>Example

Para crear una imagen de captura y asignarle el nombre WinPECapture. Wim, escriba:
```
WDSUTIL /New-CaptureImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPECapture.wim"
```
Para crear una imagen de captura y aplicar la configuración especificada, escriba:
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim 
/DestinationImage /FilePath:"\\Server\Share\WinPECapture.wim" /Name:"New WinPE image" /Description:"WinPE image with capture utility" /Overwrite:No /UnattendFilePath:"\\Server\Share\WDSCapture.inf"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)