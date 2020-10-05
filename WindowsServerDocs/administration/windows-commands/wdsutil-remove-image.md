---
title: WDSUtil-quitar imagen
description: Artículo de referencia de WDSUtil Remove-image, que elimina una imagen de un servidor.
ms.topic: reference
ms.assetid: ce5e2384-2264-4b22-92af-74eec8c10ae0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f1d955f54f345b6d2832eabe2334dd77f32633e7
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731239"
---
# <a name="wdsutil-remove-image"></a>WDSUtil-quitar imagen

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Elimina una imagen de un servidor.

## <a name="syntax"></a>Sintaxis
para imágenes de arranque:
```
wdsutil [Options] /remove-Image:<Image name> [/Server:<Server name> type:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
```
para imágenes de instalación:
```
wdsutil [Options] /remove-image:<Image name> [/Server:<Server name> type:Install ImageGroup:<Image group name>] [/Filename:<Filename>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
| /remove-image:<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
mediatype: {boot &#124; instalar}|Especifica el tipo de imagen.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen. Dado que es posible tener el mismo nombre de imagen para diferentes imágenes de arranque en distintas arquitecturas, la especificación del valor de la arquitectura garantiza que se quitará la imagen correcta.|
|\ImageGroup: <Image group name> ]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utilizará ese grupo de imágenes. Si existe más de un grupo de imágenes, debe usar esta opción para especificar el grupo de imágenes.|
|[/Filename:<File name>]|Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.|
## <a name="examples"></a>Ejemplos
Para quitar una imagen de arranque, escriba:
```
wdsutil /remove-Imagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86
```
```
wdsutil /verbose /remove-Image:WinPE Boot Image /Server:MyWDSServer type:Boot /Architecture:x64 /Filename:boot.wim
```
Para quitar una imagen de instalación, escriba:
```
wdsutil /remove-Image:Windows Vista with Officemediatype:Install
```
```
wdsutil /verbose /remove-Image:Windows Vista with Office /Server:MyWDSServemediatype:Instal ImageGroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil-comando Add-image](wdsutil-add-image.md)
- [comando de copia de imagen de WDSUtil](wdsutil-copy-image.md)
- [comando WDSUtil Export-Image](wdsutil-export-image.md)
- [WDSUtil comando Get-Image](wdsutil-get-image.md)
- [WDSUtil-comando Replace-Image](wdsutil-replace-image.md)
- [WDSUtil Set-Image (comando)](wdsutil-set-image.md)
