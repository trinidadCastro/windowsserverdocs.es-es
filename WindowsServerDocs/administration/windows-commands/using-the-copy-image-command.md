---
title: Copiar imagen
description: Artículo de referencia de Copy-Image, que copia imágenes que se encuentran dentro del mismo grupo de imágenes.
ms.topic: article
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4995d45de3897c590dd232b316756330081b13ee
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892200"
---
# <a name="copy-image"></a>Copiar imagen

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia las imágenes que se encuentran dentro del mismo grupo de imágenes. Para copiar imágenes entre grupos de imágenes, use el comando de [comando export-Image](using-the-export-image-command.md) y, a continuación, [mediante el comando de comando Add-image](using-the-add-image-command.md) .

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /copy-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Name:<Name>
         /Filename:<File name>
         [/Description:<Description>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
soporte<Image name>|Especifica el nombre de la imagen que se va a copiar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
mediatype: instalación|Especifica el tipo de imagen que se va a copiar. Esta opción debe estar configurada para **instalar**.|
|\mediaGroup: <Image group name> ]|Especifica el grupo de imágenes que contiene la imagen que se va a copiar. Si no se especifica ningún grupo de imágenes y solo existe un grupo en el servidor, se utilizará ese grupo de imágenes de forma predeterminada. Si existen varios grupos de imágenes en el servidor, debe especificar el grupo de imágenes.|
|[/Filename:<Filename>]|Especifica el nombre de archivo de la imagen que se va a copiar. Si la imagen de origen no se puede identificar de forma única por nombre, debe especificar el nombre de archivo.|
|/DestinationImage|Especifica la configuración de la imagen de destino, tal y como se describe en la tabla siguiente.<p>-/Name:: <Name> establece el nombre para mostrar de la imagen que se va a copiar.<br />-/Filename: <Filename> -establece el nombre del archivo de imagen de destino que contendrá la copia de la imagen.<br />-[/Description: <Description>]: Establece la descripción de la copia de la imagen.|
## <a name="examples"></a>Ejemplos
Para crear una copia de la imagen especificada y asignarle el nombre WindowsVista. Wim, escriba:
```
wdsutil /copy-Imagmedia:Windows Vista with Officemediatype:Install /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim
```
Para crear una copia de la imagen especificada, aplique la configuración especificada y asigne a la copia el nombre WindowsVista. Wim, escriba:
```
wdsutil /verbose /Progress /copy-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim /Description:This is a copy of the original Windows image with Office installed
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-add-image-command.md) 
 Add-image [Usar el comando](using-the-export-image-command.md) 
 Export-Image [Usar el comando](using-the-get-image-command.md) 
 Get-Image [Usar el comando](using-the-remove-image-command.md) 
 Remove-image [Usar el comando](using-the-replace-image-command.md) 
 Replace-Image [Subcomando: set-Image](subcommand-set-image.md)
