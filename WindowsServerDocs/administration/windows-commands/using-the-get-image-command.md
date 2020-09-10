---
title: Get-Image
description: Artículo de referencia de Get-Image, que recupera información acerca de una imagen.
ms.topic: reference
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a39741cb7b728b83e965fe59760973bac8d573d7
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638169"
---
# <a name="get-image"></a>Get-Image

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera información acerca de una imagen.

## <a name="syntax"></a>Syntax
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
soporte<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
mediatype: {boot &#124; instalar}|Especifica el tipo de imagen.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en distintas arquitecturas, la especificación del valor de arquitectura garantiza que se devuelva la imagen correcta.|
|[/Filename:<File name>]|Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.|
|\mediaGroup: <Image group name> ]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún grupo de imágenes y solo existe un grupo de imágenes en el servidor, se usará ese grupo. Si existe más de un grupo de imágenes en el servidor, debe usar este parámetro para especificar el grupo de imágenes.|
## <a name="examples"></a>Ejemplos
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
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-add-image-command.md) 
 Add-image [Usar el comando](using-the-copy-image-command.md) 
 Copy-Image [Usar el comando](using-the-export-image-command.md) 
 Export-Image [Usar el comando](using-the-remove-image-command.md) 
 Remove-image [Usar el comando](using-the-replace-image-command.md) 
 Replace-Image [Subcomando: set-Image](subcommand-set-image.md)
