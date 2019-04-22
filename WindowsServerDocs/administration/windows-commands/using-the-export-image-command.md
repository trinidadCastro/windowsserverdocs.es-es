---
title: Con el comando Export-Image
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: efa9b2d09c37a383a91883ee02c995eedb2f235e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823146"
---
# <a name="using-the-export-image-command"></a>Con el comando Export-Image

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exporta una imagen existente del almacén de imágenes a otro archivo de imagen de Windows (.wim).
## <a name="syntax"></a>Sintaxis
para las imágenes de arranque:
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No}]
```
las imágenes de instalación:
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No | append}]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
Medio:<Image name>|Especifica el nombre de la imagen que se exportarán.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
tipo de medio: {arranque &#124; instalar}|Especifica el tipo de imagen que se exportarán.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen que se exportarán. Si se especifica ningún nombre de grupo de imágenes y grupo de solo imágenes existe en el servidor, se usará ese grupo de imágenes de forma predeterminada. Si existe más de un grupo de imágenes en el servidor, se debe especificar el grupo de imágenes.|
|/ Arquitectura: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen que se exportarán. Dado que es posible tener el mismo nombre de imagen para las imágenes de arranque en distintas arquitecturas, especificando el valor de la arquitectura garantiza que se devolverá la imagen correcta.|
|[/Filename:<Filename>]|Si la imagen no se identifica por nombre, debe especificarse el nombre de archivo.|
|/DestinationImage|Especifica la configuración de la imagen de destino. Puede especificar estas opciones mediante las siguientes opciones:<br /><br />-/ FilePath:<File path and name> -especifica la ruta de acceso completa para la nueva imagen.<br />-[/ Name:<Name>]-establece el nombre para mostrar de la imagen. Si se especifica ningún nombre, se usará el nombre para mostrar de la imagen de origen.<br />-[/ Descripción: <Description>]-Establece la descripción de la imagen.|
|[/ Overwrite: {Sí &#124; No &#124; anexar}]|Determina si el archivo especificado en el **/DestinationImage** opción, se sobrescribirá si ya existe un archivo existente con ese nombre en/filepath.<br /><br />-   **Sí** hace que se puede sobrescribir el archivo existente.<br />-   **No** (opción predeterminada) hace que un error si ya existe un archivo con el mismo nombre.<br />-   **anexar** hace que la imagen generada que se debe anexar como una nueva imagen en el archivo .wim existente.|
## <a name="BKMK_examples"></a>Ejemplos
Para exportar una imagen de arranque, escriba uno de los siguientes:
```
wdsutil /Export-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /DestinationImage /Filepath:"C:\temp\boot.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/DestinationImage /Filepath:"\\Server\Share\ExportImage.wim" /Name:"Exported WinPE image" /Description:"WinPE Image from WDS server" /Overwrite:Yes
```
Para exportar una imagen de instalación, escriba uno de los siguientes:
```
wdsutil /Export-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Filepath:"C:\Temp\Install.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:"Exported Windows image" /Description:"Windows Vista image from WDS server" /Overwrite:append
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando add-Image](using-the-add-image-command.md)
[mediante el comando de Copiar imagen](using-the-copy-image-command.md)
[con la imagen de get Comando](using-the-get-image-command.md)
[mediante el comando remove-Image](using-the-remove-image-command.md)
[mediante el comando replace-Image](using-the-replace-image-command.md)
[subcomando : Establecer imagen](subcommand-set-image.md)
