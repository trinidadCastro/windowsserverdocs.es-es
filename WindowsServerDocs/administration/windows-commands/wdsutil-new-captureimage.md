---
title: New-Imagendecaptura
description: Artículo de referencia sobre New-Imagendecaptura, que crea una nueva imagen de captura a partir de una imagen de arranque existente.
ms.topic: reference
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ab582e65ad3f1c9920071260541fefd0125bf428
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730758"
---
# <a name="new-captureimage"></a>New-Imagendecaptura

Crea una nueva imagen de captura a partir de una imagen de arranque existente. Las imágenes de captura son imágenes de arranque que inician la utilidad de captura de servicios de implementación de Windows en lugar de iniciar el programa de instalación. Cuando se arranca un equipo de referencia (que se ha preparado con Sysprep) en una imagen de captura, un asistente crea una imagen de instalación del equipo de referencia y la guarda como un archivo de imagen de Windows (. wim). También puede Agregar la imagen a un medio (por ejemplo, un CD, un DVD o una unidad USB) y, a continuación, arrancar un equipo desde ese medio. Tras crear la imagen de instalación, podrá agregar la imagen al servidor para la implementación del arranque PXE. Para obtener más información, consulte crear imágenes ( [https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311) ).

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

### <a name="parameters"></a>Parámetros

|        Parámetro         |                                                                                                                                                                                                                         Descripción                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<Server name>] |                                                                                                                                       Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.                                                                                                                                        |
|   /Image:\<Image name>   |                                                                                                                                                                                                         Especifica el nombre de la imagen de arranque de origen.                                                                                                                                                                                                         |
|   /Architecture: {x86    |                                                                                                                                                                                                                             64                                                                                                                                                                                                                             |
| [/Filename: \<Filename> ] |                                                                                                                                                                            Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.                                                                                                                                                                            |
|    /DestinationImage     | Especifica la configuración de la imagen de destino. La configuración se especifica mediante las siguientes opciones:</br>-/FilePath: \<File path and name> establece la ruta de acceso completa del archivo para la nueva imagen de captura.</br>-[/Name: \<Name> ]: establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre para mostrar, se usará el nombre para mostrar de la imagen de origen.</br>-[/Description: \<Description> ]-establece la descripción de la imagen.</br>-[/Overwrite: {Yes |

## <a name="examples"></a>Ejemplos

Para crear una imagen de captura y asignarle el nombre WinPECapture. Wim, escriba:
```
WDSUTIL /New-CaptureImage /Image:WinPE boot image /Architecture:x86 /DestinationImage /FilePath:C:\Temp\WinPECapture.wim
```
Para crear una imagen de captura y aplicar la configuración especificada, escriba:
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:WinPE boot image /Architecture:x64 /Filename:boot.wim
/DestinationImage /FilePath:\\Server\Share\WinPECapture.wim /Name:New WinPE image /Description:WinPE image with capture utility /Overwrite:No /UnattendFilePath:\\Server\Share\WDSCapture.inf
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)