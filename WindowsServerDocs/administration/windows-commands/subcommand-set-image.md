---
title: 'Conjunto de subcomandos: imagen'
description: Artículo de referencia de Subcommand Set-Image, que cambia los atributos de una imagen.
ms.topic: reference
ms.assetid: 2ae03c86-7a13-4e38-9182-32e55fffd504
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3b0df9dd062c537bbfcdb0436fcf8dafe0ce3905
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640870"
---
# <a name="subcommand-set-image"></a>Subcomando: set-Image

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia los atributos de una imagen.

## <a name="syntax"></a>Syntax
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
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
soporte<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
mediatype: {boot &#124; instalar}|Especifica el tipo de imagen.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen. Dado que puede tener el mismo nombre de imagen para imágenes de arranque diferentes en distintas arquitecturas, la especificación de la arquitectura garantiza que se modifique la imagen correcta.|
|[/Filename:<File name>]|Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.|
|/Name|Especifica el nombre de la imagen.|
|/Description<Description>]|Establece la descripción de la imagen.|
|[/Enabled: {Yes &#124; no}]|Habilita o deshabilita la imagen.|
|\mediaGroup: <Image group name> ]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utilizará ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar el grupo de imágenes.|
|[/UserFilter: <SDDL> ]|Establece el filtro de usuario en la imagen. La cadena de filtro debe estar en el formato de lenguaje de definición de descriptores de seguridad (SDDL). Tenga en cuenta que, a diferencia de la opción **/Security** para los grupos de imágenes, esta opción solo restringe quién puede ver la definición de la imagen y no los recursos de archivos de imagen reales. Para restringir el acceso a los recursos de archivo y, por lo tanto, el acceso a todas las imágenes de un grupo de imágenes, deberá establecer la seguridad para el grupo de imágenes.|
|[/UnattendFile:<Unattend file path>]|Establece la ruta de acceso completa al archivo de instalación desatendida que se va a asociar a la imagen. Por ejemplo: **D:\Files\Unattend\Img1Unattend.xml**|
|[/OverwriteUnattend: {Yes &#124; no}]|Puede especificar **/overwrite** para sobrescribir el archivo de instalación desatendida si ya existe un archivo de instalación desatendida asociado a la imagen. Tenga en cuenta que el valor predeterminado es **no**.|
## <a name="examples"></a>Ejemplos
Para establecer los valores de una imagen de arranque, escriba uno de los siguientes:
```
wdsutil /Set-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86 /Description:New description
wdsutil /verbose /Set-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
/Name:New Name /Description:New Description /Enabled:Yes
```
Para establecer los valores de una imagen de instalación, escriba uno de los siguientes:
```
wdsutil /Set-Imagmedia:Windows Vista with Officemediatype:Install /Description:New description
wdsutil /verbose /Set-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /Name:New name /Description:New description /UserFilter:O:BAG:DUD:AI(A;ID;FA;;;SY)(A;ID;FA;;;BA)(A;ID;0x1200a9;;;AU) /Enabled:Yes /UnattendFile:\\server\share\unattend.xml /OverwriteUnattend:Yes
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-add-image-command.md) 
 Add-image [Usar el comando](using-the-copy-image-command.md) 
 Copy-Image [Usar el comando](using-the-export-image-command.md) 
 Export-Image [Usar el comando](using-the-get-image-command.md) 
 Get-Image [Usar el comando](using-the-remove-image-command.md) 
 Remove-image [Usar el comando Replace-Image](using-the-replace-image-command.md)
