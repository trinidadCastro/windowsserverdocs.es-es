---
title: Get-Image de WDSUtil
description: Artículo de referencia de WDSUtil Get-Image, que recupera información acerca de una imagen.
ms.topic: reference
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0f6cfcf3783ef49f2dd57941ae9a2ae4feb3742e
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730813"
---
# <a name="wdsutil-get-image"></a>Get-Image de WDSUtil

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera información acerca de una imagen.

## <a name="syntax"></a>Sintaxis
para imágenes de arranque:
```
wdsutil [Options] /Get-Image image:<Image name> [/Server:<Server name> imagetype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
para imágenes de instalación:
```
wdsutil [Options] /Get-image image:<Image name> [/Server:<Server name> imagetype:Install imagegroup:<Image group name>] [/Filename:<File name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
| impresión<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
| ImageType: {boot &#124; install}|Especifica el tipo de imagen.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en distintas arquitecturas, la especificación del valor de arquitectura garantiza que se devuelva la imagen correcta.|
|[/Filename:<File name>]|Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.|
|\imagegroup: <Image group name> ]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún grupo de imágenes y solo existe un grupo de imágenes en el servidor, se usará ese grupo. Si existe más de un grupo de imágenes en el servidor, debe usar este parámetro para especificar el grupo de imágenes.|
## <a name="examples"></a>Ejemplos
Para recuperar información acerca de una imagen de arranque, escriba una de las siguientes opciones:
```
wdsutil /Get-Image image:WinPE boot imagetype:Boot /Architecture:x86
wdsutil /verbose /Get-Image image:WinPE boot image /Server:MyWDSServer imagetype:Boot /Architecture:x86 /Filename:boot.wim
```
Para recuperar información acerca de una imagen de instalación, escriba una de las siguientes opciones:
```
wdsutil /Get-Image:Windows Vista with Office imagetype:Install
wdsutil /verbose /Get-Image:Windows Vista with Office /Server:MyWDSServer imagetype:Install imagegroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil-comando Add-image](wdsutil-add-image.md)
- [comando de copia de imagen de WDSUtil](wdsutil-copy-image.md)
- [comando WDSUtil Export-Image](wdsutil-export-image.md)
- [comando WDSUtil Remove-image](wdsutil-remove-image.md)
- [WDSUtil-comando Replace-Image](wdsutil-replace-image.md)
- [WDSUtil Set-Image (comando)](wdsutil-set-image.md)
