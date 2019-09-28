---
title: 'Conjunto de subcomandos: imagen'
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ae03c86-7a13-4e38-9182-32e55fffd504
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4584bd6253b1991aba7e87fc42ff484101681081
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383857"
---
# <a name="subcommand-set-image"></a>Subcomando: set-Image

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia los atributos de una imagen.
## <a name="syntax"></a>Sintaxis
para imágenes de arranque:
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] [/Name:<Name>] 
[/Description:<Description>] [/Enabled:{Yes | No}]
```
para imágenes de instalación:
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     [/Name:<Name>]
     [/Description:<Description>]
     [/UserFilter:<SDDL>]
     [/Enabled:{Yes | No}]
     [/UnattendFile:<Unattend file path>]
         [/OverwriteUnattend:{Yes | No}]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
medios: <Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
mediatype: {boot &#124; install}|Especifica el tipo de imagen.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen. Dado que puede tener el mismo nombre de imagen para imágenes de arranque diferentes en distintas arquitecturas, la especificación de la arquitectura garantiza que se modifique la imagen correcta.|
|[/Filename:<File name>]|Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.|
|/Name|Especifica el nombre de la imagen.|
|/Description<Description>]|Establece la descripción de la imagen.|
|[/Enabled: {Yes &#124; no}]|Habilita o deshabilita la imagen.|
|\mediaGroup: <Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utilizará ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar el grupo de imágenes.|
|[/UserFilter: <SDDL>]|Establece el filtro de usuario en la imagen. La cadena de filtro debe estar en el formato de lenguaje de definición de descriptores de seguridad (SDDL). Tenga en cuenta que, a diferencia de la opción **/Security** para los grupos de imágenes, esta opción solo restringe quién puede ver la definición de la imagen y no los recursos de archivos de imagen reales. Para restringir el acceso a los recursos de archivo y, por lo tanto, el acceso a todas las imágenes de un grupo de imágenes, deberá establecer la seguridad para el grupo de imágenes.|
|[/UnattendFile:<Unattend file path>]|Establece la ruta de acceso completa al archivo de instalación desatendida que se va a asociar a la imagen. Por ejemplo: **D:\Files\Unattend\Img1Unattend.xml**|
|[/OverwriteUnattend: {Yes &#124; no}]|Puede especificar **/overwrite** para sobrescribir el archivo de instalación desatendida si ya existe un archivo de instalación desatendida asociado a la imagen. Tenga en cuenta que el valor predeterminado es **no**.|
## <a name="BKMK_examples"></a>Example
Para establecer los valores de una imagen de arranque, escriba uno de los siguientes:
```
wdsutil /Set-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /Description:"New description"
wdsutil /verbose /Set-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim 
/Name:"New Name" /Description:"New Description" /Enabled:Yes
```
Para establecer los valores de una imagen de instalación, escriba uno de los siguientes:
```
wdsutil /Set-Imagmedia:"Windows Vista with Officemediatype:Install /Description:"New description" 
wdsutil /verbose /Set-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /Name:"New name" /Description:"New description" /UserFilter:"O:BAG:DUD:AI(A;ID;FA;;;SY)(A;ID;FA;;;BA)(A;ID;0x1200a9;;;AU)" /Enabled:Yes /UnattendFile:\\server\share\unattend.xml /OverwriteUnattend:Yes
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando add-Image](using-the-add-image-command.md)
[mediante el comando copy-Image](using-the-copy-image-command.md)
[mediante el comando export-Image](using-the-export-image-command.md)
[mediante el comando Get-Image](using-the-get-image-command.md)
 mediante el comando[ Comando Remove-image](using-the-remove-image-command.md)1[mediante el comando Replace-Image](using-the-replace-image-command.md)
