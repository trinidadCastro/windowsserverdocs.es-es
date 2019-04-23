---
title: Subcomando Establecer imagen
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e63c67210764de76edae18a1897a68d763f9d695
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856486"
---
# <a name="subcommand-set-image"></a>Subcomando: set-Image

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia los atributos de una imagen.
## <a name="syntax"></a>Sintaxis
para las imágenes de arranque:
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] [/Name:<Name>] 
[/Description:<Description>] [/Enabled:{Yes | No}]
```
las imágenes de instalación:
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
Medio:<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
tipo de medio: {arranque &#124; instalar}|Especifica el tipo de imagen.|
|/ Arquitectura: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen. Dado que puede tener el mismo nombre de imagen para las imágenes de arranque diferentes en distintas arquitecturas, especificando la arquitectura garantiza que se modifica la imagen correcta.|
|[/Filename:<File name>]|Si la imagen no se identifica por nombre, debe usar esta opción para especificar el nombre de archivo.|
|/ [Name]|Especifica el nombre de la imagen.|
|[/ Descripción:<Description>]|Establece la descripción de la imagen.|
|[/ Enabled: {Sí &#124; n}]|Habilita o deshabilita la imagen.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si se especifica ningún nombre de grupo de imágenes y grupo de solo imágenes existe en el servidor, se usará ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar el grupo de imágenes.|
|[/ UserFilter:<SDDL>]|Establece el filtro de usuarios en la imagen. La cadena de filtro debe estar en formato de lenguaje de definición de descriptores de seguridad (SDDL). Tenga en cuenta que, a diferencia de la **/seguridad** opción para los grupos de la imagen, esta opción solamente restringe quién puede ver la definición de la imagen y no los recursos de archivo de imagen real. Para restringir el acceso a los recursos de archivo y, por lo tanto, tener acceso a todas las imágenes dentro de un grupo de imágenes, deberá establecer la seguridad para el grupo de imágenes propio.|
|[/UnattendFile:<Unattend file path>]|Establece la ruta de acceso completa al archivo de instalación desatendida que se asociará con la imagen. Por ejemplo: **D:\Files\Unattend\Img1Unattend.xml**|
|[/OverwriteUnattend:{Yes &#124; No}]|Puede especificar **/Overwrite** para sobrescribir el archivo de instalación desatendida si ya existe un archivo de instalación desatendida asociado a la imagen. Tenga en cuenta que el valor predeterminado es **No**.|
## <a name="BKMK_examples"></a>Ejemplos
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
[mediante el comando add-Image](using-the-add-image-command.md)
[mediante el comando de Copiar imagen](using-the-copy-image-command.md)
[mediante el Comando de Export-Image](using-the-export-image-command.md)
[mediante el comando get-Image](using-the-get-image-command.md)
[mediante el comando remove-Image](using-the-remove-image-command.md) 
 [ Con la imagen de reemplazar comandos](using-the-replace-image-command.md)
