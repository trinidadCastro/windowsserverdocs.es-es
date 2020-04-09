---
title: Get-Image
description: Temas de comandos de Windows para Get-Image, que recupera información acerca de una imagen.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfebb8948d21d4f09855683bbf6c42d725d877e8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831008"
---
# <a name="get-image"></a>Get-Image

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información acerca de una imagen.

## <a name="syntax"></a>Sintaxis
para imágenes de arranque:
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
para imágenes de instalación:
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
medios:<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
mediatype: {boot &#124; install}|Especifica el tipo de imagen.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en distintas arquitecturas, la especificación del valor de arquitectura garantiza que se devuelva la imagen correcta.|
|[/Filename:<File name>]|Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún grupo de imágenes y solo existe un grupo de imágenes en el servidor, se usará ese grupo. Si existe más de un grupo de imágenes en el servidor, debe usar este parámetro para especificar el grupo de imágenes.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para recuperar información acerca de una imagen de arranque, escriba una de las siguientes opciones:
```
wdsutil /Get-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86
wdsutil /verbose /Get-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
```
Para recuperar información acerca de una imagen de instalación, escriba una de las siguientes opciones:
```
wdsutil /Get-Imagmedia:Windows Vista with Officemediatype:Install
wdsutil /verbose /Get-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando add-Image](using-the-add-image-command.md)
[con el comando copy-](using-the-copy-image-command.md) Image
mediante el comando [Export-Image](using-the-export-image-command.md)
[mediante el comando Remove-image](using-the-remove-image-command.md)
[con el comando Replace-Image](using-the-replace-image-command.md)
[Subcommand: set-Image](subcommand-set-image.md)
