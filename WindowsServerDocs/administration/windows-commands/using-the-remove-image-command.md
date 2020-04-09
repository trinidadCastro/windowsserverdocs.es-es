---
title: Quitar imagen
description: Tema de comandos de Windows para Remove-image, que elimina una imagen de un servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce5e2384-2264-4b22-92af-74eec8c10ae0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 760c61c3109255000fe1177a456243a5c91c883f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830368"
---
# <a name="remove-image"></a>Quitar imagen

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina una imagen de un servidor.

## <a name="syntax"></a>Sintaxis
para imágenes de arranque:
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
```
para imágenes de instalación:
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<Filename>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
medios:<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
mediatype: {boot &#124; install}|Especifica el tipo de imagen.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen. Dado que es posible tener el mismo nombre de imagen para diferentes imágenes de arranque en distintas arquitecturas, la especificación del valor de la arquitectura garantiza que se quitará la imagen correcta.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utilizará ese grupo de imágenes. Si existe más de un grupo de imágenes, debe usar esta opción para especificar el grupo de imágenes.|
|[/Filename:<File name>]|Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para quitar una imagen de arranque, escriba:
```
wdsutil /remove-Imagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86
```
```
wdsutil /verbose /remove-Imagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim
```
Para quitar una imagen de instalación, escriba:
```
wdsutil /remove-Imagmedia:Windows Vista with Officemediatype:Install
```
```
wdsutil /verbose /remove-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>Referencias adicionales
- [La clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando add-Image](using-the-add-image-command.md)
[con el comando copy-](using-the-copy-image-command.md) Image
[mediante el comando export-Image](using-the-export-image-command.md)
[mediante el comando Get-image](using-the-get-image-command.md)
[mediante el comando Replace-Image](using-the-replace-image-command.md)
[Subcommand: set-Image](subcommand-set-image.md)
