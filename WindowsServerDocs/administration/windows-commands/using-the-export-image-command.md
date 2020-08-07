---
title: Exportar-imagen
description: Artículo de referencia de Export-Image, que exporta una imagen existente del almacén de imágenes a otro archivo de imagen de Windows (. wim).
ms.topic: article
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f06ded887cf7fcd134ffb8d5b9bfbe588a584e1
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896990"
---
# <a name="export-image"></a>Exportar-imagen

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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
soporte<Image name>|Especifica el nombre de la imagen que se va a exportar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
mediatype: {boot &#124; instalar}|Especifica el tipo de imagen que se va a exportar.|
|\mediaGroup: <Image group name> ]|Especifica el grupo de imágenes que contiene la imagen que se va a exportar. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utilizará ese grupo de imágenes de forma predeterminada. Si existe más de un grupo de imágenes en el servidor, se debe especificar el grupo de imágenes.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen que se va a exportar. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en distintas arquitecturas, la especificación del valor de arquitectura garantiza que se devolverá la imagen correcta.|
|[/Filename:<Filename>]|Si la imagen no se puede identificar de forma única por nombre, se debe especificar el nombre de archivo.|
|/DestinationImage|Especifica la configuración de la imagen de destino. Puede especificar esta configuración con las siguientes opciones:<p>-/Filepath: <File path and name> -especifica la ruta de acceso completa del archivo para la nueva imagen.<br />-[/Name: <Name> ]: establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre, se usará el nombre para mostrar de la imagen de origen.<br />-[/Description: <Description>]: Establece la descripción de la imagen.|
|[/Overwrite: {Yes &#124; no &#124; Append}]|Determina si el archivo especificado en la opción **/DestinationImage** se sobrescribirá si ya existe un archivo con ese nombre en el/FilePath.<p>-   **Sí** hace que se sobrescriba el archivo existente.<br />-   **No** (la opción predeterminada) hace que se produzca un error si ya existe un archivo con el mismo nombre.<br />-   **Append** hace que la imagen generada se anexe como una nueva imagen dentro del archivo. Wim existente.|
## <a name="examples"></a>Ejemplos
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
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-add-image-command.md) 
 Add-image [Usar el comando](using-the-copy-image-command.md) 
 Copy-Image [Usar el comando](using-the-get-image-command.md) 
 Get-Image [Usar el comando](using-the-remove-image-command.md) 
 Remove-image [Usar el comando](using-the-replace-image-command.md) 
 Replace-Image [Subcomando: set-Image](subcommand-set-image.md)
