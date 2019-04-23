---
title: Con el comando nuevo Imagendecaptura
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7d9f402acb9904624bdb4193a4306d57b104eda8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888616"
---
# <a name="using-the-new-captureimage-command"></a>Con el comando nuevo Imagendecaptura



Crea una nueva imagen de captura de una imagen de arranque existente. Las imágenes de captura son de utilidad en lugar de iniciar el programa de instalación de la captura de imágenes de arranque que inicie los servicios de implementación de Windows. Cuando se arranca un equipo de referencia (que se ha preparado con Sysprep) desde una imagen de captura, un asistente crea una imagen de instalación del equipo de referencia y lo guarda como un archivo de imagen de Windows (.wim). Puede también agregar la imagen en los medios (por ejemplo, una unidad de CD, DVD o USB) y, a continuación, se arranca un equipo desde ese medio. Después de crear la imagen de instalación, puede agregar la imagen en el servidor para la implementación de arranque PXE. Para obtener más información, vea Creación de imágenes ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

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

|Parámetro|Descripción|
|---------|-----------|
|[/ Server:\<nombre del servidor >]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|/ Image:\<nombre de la imagen >|Especifica el nombre de la imagen de arranque de origen.|
|/ Architecture: {x 86 | ia64 | x64}|Especifica la arquitectura de la imagen que se usará. Dado que puede tener el mismo nombre de imagen para las imágenes de arranque diferentes en distintas arquitecturas, especificar que esto garantiza que la imagen correcta se usa.|
|[/Filename: \<Nombre de archivo >]|Si la imagen no se identifica por nombre, debe usar esta opción para especificar el nombre de archivo.|
|/DestinationImage|Especifica la configuración de la imagen de destino. Especifique la configuración mediante las siguientes opciones:</br>-   /FilePath: \<Ruta de acceso y nombre del archivo > establece la ruta de acceso completa para la nueva imagen de captura.</br>-[/ Name: \<Nombre >]-establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre para mostrar, se usará el nombre para mostrar de la imagen de origen.</br>-[/ Descripción: \<Descripción >]-establece la descripción de la imagen.</br>-[/ Overwrite: {Sí | No | Append}]: determina si el archivo especificado en **/DestinationImage** se debe sobrescribir si ya existe otro archivo con ese nombre en/filepath. **Sí** sobrescribe el archivo existente. **No** (valor predeterminado) provoca un error si ya existe otro archivo con el mismo nombre. **Anexar** adjunta la imagen generada como una nueva imagen en el archivo .wim existente.</br>-   [/UnattendFilePath: \<Ruta de acceso de archivo >]-establece la ruta de acceso completa y el nombre para el archivo de instalación desatendida de imagen de captura.|

## <a name="BKMK_examples"></a>Ejemplos

Para crear una imagen de captura y asígnele el nombre WinPECapture.wim, escriba:
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