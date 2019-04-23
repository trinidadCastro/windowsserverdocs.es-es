---
title: Usar el comando get-Image
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b78f4ed9352c21bf6de19136a625a4f4fe7ac5f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877446"
---
# <a name="using-the-get-image-command"></a>Usar el comando get-Image

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información sobre una imagen.
## <a name="syntax"></a>Sintaxis
para las imágenes de arranque:
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
las imágenes de instalación:
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
Medio:<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
tipo de medio: {arranque &#124; instalar}|Especifica el tipo de imagen.|
|/ Arquitectura: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen. Dado que es posible tener el mismo nombre de imagen para las imágenes de arranque en distintas arquitecturas, especificando el valor de la arquitectura garantiza que se devuelve la imagen correcta.|
|[/Filename:<File name>]|Si la imagen no se identifica por nombre, debe usar esta opción para especificar el nombre de archivo.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún grupo de imágenes y grupo de solo imágenes existe en el servidor, se usará ese grupo. Si existe más de un grupo de imágenes en el servidor, debe utilizar este parámetro para especificar el grupo de imágenes.|
## <a name="BKMK_examples"></a>Ejemplos
Para recuperar información acerca de una imagen de arranque, escriba uno de los siguientes:
```
wdsutil /Get-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86
wdsutil /verbose /Get-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
```
Para recuperar información acerca de una imagen de instalación, escriba uno de los siguientes:
```
wdsutil /Get-Imagmedia:"Windows Vista with Officemediatype:Install
wdsutil /verbose /Get-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando add-Image](using-the-add-image-command.md)
[mediante el comando de Copiar imagen](using-the-copy-image-command.md)
[mediante el Comando de Export-Image](using-the-export-image-command.md)
[mediante el comando remove-Image](using-the-remove-image-command.md)
[mediante el comando replace-Image](using-the-replace-image-command.md) 
 [Subcomando: Establecer imagen](subcommand-set-image.md)
