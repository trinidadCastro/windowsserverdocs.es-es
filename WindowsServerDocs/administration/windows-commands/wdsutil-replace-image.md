---
title: WDSUtil-reemplazar-imagen
description: Artículo de referencia de WDSUtil Replace-Image, que reemplaza una imagen existente por una nueva versión de la imagen.
ms.topic: reference
ms.assetid: 68ded3df-e309-420f-9f5d-caeb609385a5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b03bf133fa84f1a37578889ef8f3b3fefd5a8d16
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730638"
---
# <a name="wdsutil-replace-image"></a>WDSUtil-reemplazar-imagen

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Reemplaza una imagen existente por una nueva versión de la imagen.
## <a name="syntax"></a>Sintaxis
para imágenes de arranque:
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/Name:<Image name>]
         [/Description:<Image description>]
```
para imágenes de instalación:
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/SourceImage:<Source image name>]
         [/Name:<Image name>]
         [/Description:<Image description>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
soporte<Image name>|Especifica el nombre de la imagen que se va a reemplazar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
mediatype: {boot &#124; instalar}|Especifica el tipo de imagen que se va a reemplazar.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen que se va a reemplazar. Dado que es posible tener el mismo nombre de imagen para diferentes imágenes de arranque en distintas arquitecturas, la especificación de la arquitectura garantiza que se reemplace la imagen correcta.|
|[/Filename:<File name>]|Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.|
|/replacementImage|Especifica los valores para la imagen de reemplazo. Puede establecer esta configuración con las siguientes opciones:<p>-mediaFile: <file path> -especifica el nombre y la ubicación (ruta de acceso completa) del nuevo archivo. Wim.<br />-[/SourceImage: <image name> ]-especifica la imagen que se va a usar si el archivo. wim contiene varias imágenes. Esta opción solo se aplica a las imágenes de instalación.<br />-[/Name: <Image name> ] establece el nombre para mostrar de la imagen.<br />-[/Description: <Image description> ]-establece la descripción de la imagen.|
## <a name="examples"></a>Ejemplos
Para reemplazar una imagen de arranque, escriba uno de los siguientes:
```
wdsutil /replace-Imagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /replacementImagmediaFile:C:\MyFolder\Boot.wim
wdsutil /verbose /Progress /replace-Imagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim
/replacementImagmediaFile:\\MyServer\Share\Boot.wim /Name:My WinPE Image /Description:WinPE Image with drivers
```
Para reemplazar una imagen de instalación, escriba una de las siguientes opciones:
```
wdsutil /replace-Imagmedia:Windows Vista Homemediatype:Install /replacementImagmediaFile:C:\MyFolder\Install.wim
wdsutil /verbose /Progress /replace-Imagmedia:Windows Vista Pro /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1
/Filename:Install.wim /replacementImagmediaFile:\\MyServer\Share \Install.wim /SourceImage:Windows Vista Ultimate /Name:Windows Vista Desktop /Description:Windows Vista Ultimate with standard business applications.
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil-comando Add-image](wdsutil-add-image.md)
- [comando de copia de imagen de WDSUtil](wdsutil-copy-image.md)
- [comando WDSUtil Export-Image](wdsutil-export-image.md)
- [WDSUtil comando Get-Image](wdsutil-get-image.md)
- [WDSUtil-comando Replace-Image](wdsutil-replace-image.md)
- [WDSUtil Set-Image (comando)](wdsutil-set-image.md)
