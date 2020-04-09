---
title: Exportar-imagen
description: Tema de comandos de Windows para export-Image, que exporta una imagen existente del almacén de imágenes a otro archivo de imagen de Windows (. wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3e45c254cc6782a61828fa12e479110836e5de5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831428"
---
# <a name="export-image"></a>Exportar-imagen

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exporta una imagen existente del almacén de imágenes a otro archivo de imagen de Windows (. wim).

## <a name="syntax"></a>Sintaxis
para imágenes de arranque:
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No}]
```
para imágenes de instalación:
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No | append}]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
medios:<Image name>|Especifica el nombre de la imagen que se va a exportar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
mediatype: {boot &#124; install}|Especifica el tipo de imagen que se va a exportar.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen que se va a exportar. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utilizará ese grupo de imágenes de forma predeterminada. Si existe más de un grupo de imágenes en el servidor, se debe especificar el grupo de imágenes.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen que se va a exportar. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en distintas arquitecturas, la especificación del valor de arquitectura garantiza que se devolverá la imagen correcta.|
|[/Filename:<Filename>]|Si la imagen no se puede identificar de forma única por nombre, se debe especificar el nombre de archivo.|
|/DestinationImage|Especifica la configuración de la imagen de destino. Puede especificar esta configuración con las siguientes opciones:<p>-/Filepath:<File path and name>-especifica la ruta de acceso completa del archivo para la nueva imagen.<br />-[/Name:<Name>]: establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre, se usará el nombre para mostrar de la imagen de origen.<br />-[/Description: <Description>]: Establece la descripción de la imagen.|
|[/Overwrite: {Yes &#124; no &#124; Append}]|Determina si el archivo especificado en la opción **/DestinationImage** se sobrescribirá si ya existe un archivo con ese nombre en el/FilePath.<p>-   **sí** hace que se sobrescriba el archivo existente.<br />-   **no** (opción predeterminada) hace que se produzca un error si ya existe un archivo con el mismo nombre.<br />-   **Append** hace que la imagen generada se anexe como una nueva imagen dentro del archivo. Wim existente.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para exportar una imagen de arranque, escriba uno de los siguientes:
```
wdsutil /Export-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86 /DestinationImage /Filepath:C:\temp\boot.wim
wdsutil /verbose /Progress /Export-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/DestinationImage /Filepath:\\Server\Share\ExportImage.wim /Name:Exported WinPE image /Description:WinPE Image from WDS server /Overwrite:Yes
```
Para exportar una imagen de instalación, escriba una de las siguientes opciones:
```
wdsutil /Export-Imagmedia:Windows Vista with Officemediatype:Install /DestinationImage /Filepath:C:\Temp\Install.wim
wdsutil /verbose /Progress /Export-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:Exported Windows image /Description:Windows Vista image from WDS server /Overwrite:append
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando add-Image](using-the-add-image-command.md)
[con el comando copy-](using-the-copy-image-command.md) Image
[mediante el comando Get-Image](using-the-get-image-command.md)
[mediante el comando Remove-image](using-the-remove-image-command.md)
[con el comando Replace-Image](using-the-replace-image-command.md)
[Subcommand: set-Image](subcommand-set-image.md)
