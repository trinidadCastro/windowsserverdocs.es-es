---
title: WDSUtil-copiar imagen
description: Artículo de referencia de WDSUtil Copy-Image, que copia imágenes que se encuentran dentro del mismo grupo de imágenes.
ms.topic: reference
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a04848d0b673697809af08b91ca91856d6491773
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731006"
---
# <a name="wdsutil-copy-image"></a>WDSUtil-copiar imagen

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia las imágenes que se encuentran dentro del mismo grupo de imágenes. Para copiar imágenes entre grupos de imágenes, use el comando [de comando wdsutilExport-Image](wdsutil-export-image.md) y, a continuación, el comando [de comando wdsutiladd-Image](wdsutil-add-image.md) .

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /copy-Image image:<Image name> [/Server:<Server name>]
    imagetype:Install
     imageGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Name:<Name>
         /Filename:<File name>
         [/Description:<Description>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
| impresión<Image name>|Especifica el nombre de la imagen que se va a copiar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
| ImageType: instalación|Especifica el tipo de imagen que se va a copiar. Esta opción debe estar configurada para **instalar**.|
|\imageGroup: <Image group name> ]|Especifica el grupo de imágenes que contiene la imagen que se va a copiar. Si no se especifica ningún grupo de imágenes y solo existe un grupo en el servidor, se utilizará ese grupo de imágenes de forma predeterminada. Si existen varios grupos de imágenes en el servidor, debe especificar el grupo de imágenes.|
|[/Filename:<Filename>]|Especifica el nombre de archivo de la imagen que se va a copiar. Si la imagen de origen no se puede identificar de forma única por nombre, debe especificar el nombre de archivo.|
|/DestinationImage|Especifica la configuración de la imagen de destino, tal y como se describe en la tabla siguiente.<p>-/Name:: <Name> establece el nombre para mostrar de la imagen que se va a copiar.<br />-/Filename: <Filename> -establece el nombre del archivo de imagen de destino que contendrá la copia de la imagen.<br />-[/Description: <Description>]: Establece la descripción de la copia de la imagen.|
## <a name="examples"></a>Ejemplos
Para crear una copia de la imagen especificada y asignarle el nombre WindowsVista. Wim, escriba:
```
wdsutil /copy-Image image:Windows Vista with Office imagetype:Install /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim
```
Para crear una copia de la imagen especificada, aplique la configuración especificada y asigne a la copia el nombre WindowsVista. Wim, escriba:
```
wdsutil /verbose /Progress /copy-Image image:Windows Vista with Office /Server:MyWDSServe imagetype:Install imageGroup:ImageGroup1
/Filename:install.wim /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim /Description:This is a copy of the original Windows image with Office installed
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil-comando Add-image](wdsutil-add-image.md)
- [comando WDSUtil Export-Image](wdsutil-export-image.md)
- [WDSUtil comando Get-Image](wdsutil-get-image.md)
- [comando WDSUtil Remove-image](wdsutil-remove-image.md)
- [WDSUtil-comando Replace-Image](wdsutil-replace-image.md)
- [WDSUtil Set-Image (comando)](wdsutil-set-image.md)
