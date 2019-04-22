---
title: Con la imagen de reemplazar comandos
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68ded3df-e309-420f-9f5d-caeb609385a5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e396ee678e22885a50c02800d77ecea1cc5ef8ba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819656"
---
# <a name="using-the-replace-image-command"></a>Con la imagen de reemplazar comandos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

reemplaza una imagen existente con una nueva versión de esa imagen.
## <a name="syntax"></a>Sintaxis
para las imágenes de arranque:
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
las imágenes de instalación:
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
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
Medio:<Image name>|Especifica el nombre de la imagen que se debe reemplazar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
tipo de medio: {arranque &#124; instalar}|Especifica el tipo de imagen que se debe reemplazar.|
|/ Arquitectura: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen que se debe reemplazar. Dado que es posible tener el mismo nombre de imagen para las imágenes de arranque diferentes en distintas arquitecturas, especificar la arquitectura garantiza que se reemplaza la imagen correcta.|
|[/Filename:<File name>]|Si la imagen no se identifica por nombre, debe usar esta opción para especificar el nombre de archivo.|
|/replacementImage|Especifica la configuración de la imagen de reemplazo. Establezca estas opciones mediante las siguientes opciones:<br /><br />-mediaFile: <file path> : especifica el nombre y la ubicación (ruta de acceso completa) del archivo .wim nuevo.<br />-[/ SourceImage: <image name>]: especifica la imagen que se utilizará si el archivo .wim contiene varias imágenes. Esta opción solo se aplica a imágenes de instalación.<br />-[/ Name:<Image name>] establece el nombre para mostrar de la imagen.<br />-[/ Descripción:<Image description>]-establece la descripción de la imagen.|
## <a name="BKMK_examples"></a>Ejemplos
Para reemplazar una imagen de arranque, escriba uno de los siguientes:
```
wdsutil /replace-Imagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /replacementImagmediaFile:"C:\MyFolder\Boot.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/replacementImagmediaFile:\\MyServer\Share\Boot.wim /Name:"My WinPE Image" /Description:"WinPE Image with drivers"
```
Para reemplazar una imagen de instalación, escriba uno de los siguientes:
```
wdsutil /replace-Imagmedia:"Windows Vista Homemediatype:Install /replacementImagmediaFile:"C:\MyFolder\Install.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"Windows Vista Pro" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:Install.wim /replacementImagmediaFile:\\MyServer\Share \Install.wim /SourceImage:"Windows Vista Ultimate" /Name:"Windows Vista Desktop" /Description:"Windows Vista Ultimate with standard business applications."
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando add-Image](using-the-add-image-command.md)
[mediante el comando de Copiar imagen](using-the-copy-image-command.md)
[mediante el Comando de Export-Image](using-the-export-image-command.md)
[mediante el comando get-Image](using-the-get-image-command.md)
[mediante el comando replace-Image](using-the-replace-image-command.md) 
 [ Subcomando: set-Image](subcommand-set-image.md)
