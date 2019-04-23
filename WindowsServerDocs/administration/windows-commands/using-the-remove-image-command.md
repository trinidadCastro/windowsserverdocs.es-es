---
title: Usar el comando remove-Image
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce5e2384-2264-4b22-92af-74eec8c10ae0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2aaf7d63d858045f9dd5df399c5f3f92b038bc2e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846706"
---
# <a name="using-the-remove-image-command"></a>Usar el comando remove-Image

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

elimina una imagen de un servidor.
## <a name="syntax"></a>Sintaxis
para las imágenes de arranque:
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
```
las imágenes de instalación:
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<Filename>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
Medio:<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
tipo de medio: {arranque &#124; instalar}|Especifica el tipo de imagen.|
|/ Arquitectura: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen. Dado que es posible tener el mismo nombre de imagen para las imágenes de arranque diferentes en distintas arquitecturas, especificando el valor de la arquitectura garantiza que se eliminará la imagen correcta.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si se especifica ningún nombre de grupo de imágenes y grupo de solo imágenes existe en el servidor, se usará ese grupo de imágenes. Si existe más de un grupo de imágenes, debe usar esta opción para especificar el grupo de imágenes.|
|[/Filename:<File name>]|Si la imagen no se identifica por nombre, debe usar esta opción para especificar el nombre de archivo.|
## <a name="BKMK_examples"></a>Ejemplos
Para quitar una imagen de arranque, escriba:
```
wdsutil /remove-Imagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86
```
```
wdsutil /verbose /remove-Imagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim
```
Para quitar una imagen de instalación, escriba:
```
wdsutil /remove-Imagmedia:"Windows Vista with Officemediatype:Install
```
```
wdsutil /verbose /remove-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando add-Image](using-the-add-image-command.md)
[mediante el comando de Copiar imagen](using-the-copy-image-command.md)
[mediante el Comando de Export-Image](using-the-export-image-command.md)
[mediante el comando get-Image](using-the-get-image-command.md)
[mediante el comando replace-Image](using-the-replace-image-command.md) 
 [ Subcomando: set-Image](subcommand-set-image.md)
