---
title: Usar el comando Add-image
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0d671dd482710c486a6936cdbe3b1cc6b331866
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363743"
---
# <a name="using-the-add-image-command"></a>Usar el comando Add-image

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

agrega imágenes a un servidor de servicios de implementación de Windows. para obtener ejemplos de cómo puede usar este comando, vea [ejemplos](#BKMK_examples).
## <a name="syntax"></a>Sintaxis
en el caso de las imágenes de arranque, use la siguiente sintaxis:
```
wdsutil /add-ImagmediaFile:<wim file path> [/Server:<Server name>mediatype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>] 
[/Filename:<New wim file name>]
```
en el caso de las imágenes de instalación, use la siguiente sintaxis:
```
wdsutil /add-ImagmediaFile:<wim file path>
     [/Server:<Server name>]
   mediatype:Install
     [/Skipverify]
    mediaGroup:<Image group name>]
     [/SingleImage:<Single image name>]
         [/Name:<Name>]
         [/Description:<Description>]
     [/Filename:<File name>]
     [/UnattendFile:<Unattend file path>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
mediaFile: ruta de acceso del archivo <. Wim >|Especifica la ruta de acceso completa y el nombre de archivo del archivo de imagen de Windows (. wim) que contiene las imágenes que se van a agregar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica un nombre de servidor, se usará el servidor local.|
mediatype: {boot&#124;install}|Especifica el tipo de imágenes que se van a agregar.|
|[/Skipverify]|Especifica que no se realizará la comprobación de integridad en el archivo de imagen de origen antes de que se agregue la imagen.|
|[/Name: <Name>]|Establece el nombre para mostrar de la imagen.|
|/Description<Description>]|Establece la descripción de la imagen.|
|[/Filename:<Filename>]|Especifica el nuevo nombre de archivo para el archivo. Wim. Esto le permite cambiar el nombre de archivo del archivo. Wim al agregar la imagen. Si no se especifica ningún nombre de archivo, se utilizará el nombre del archivo de imagen de origen. En todos los casos, servicios de implementación de Windows comprueba si el nombre de archivo es único en el almacén de imágenes de arranque del equipo de destino.|
|\mediaGroup: <Image group name>]|Especifica el nombre del grupo de imágenes en el que se van a agregar las imágenes. Si existe más de un grupo de imágenes en el servidor, se debe especificar el grupo de imágenes. Si no está especificado y no existe en ese momento un grupo de imágenes, se creará uno nuevo. De otro modo, se usará el grupo de imágenes existente.|
|[/SingleImage: <Single image name>] [/Name: <Name>] /Description<Description>]|Copia la imagen única especificada de un archivo. Wim y establece el nombre para mostrar y la descripción de la imagen.|
|[/UnattendFile:<Unattend file path>]|Especifica la ruta de acceso completa al archivo de instalación desatendida que se va a asociar a las imágenes que se van a agregar. Si no se especifica **/SingleImage** , el mismo archivo de instalación desatendida se asociará a todas las imágenes del archivo. Wim.|
## <a name="BKMK_examples"></a>Example
Para agregar una imagen de arranque, escriba:
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Boot.wimmediatype:Boot
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share\Boot.wim /Server:MyWDSServemediatype:Boot /Name:"My WinPE Image" 
/Description:"WinPE Image containing the WDS Client" /Filename:WDSBoot.wim
```
Para agregar una imagen de instalación, escriba una de las siguientes opciones:
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Install.wimmediatype:Install
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share \Install.wim /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/SingleImage:"Windows Pro" /Name:"My WDS Image"
/Description:"Windows Pro image with Microsoft Office" /Filename:"Win Pro.wim" /UnattendFile:"\\server\share\unattend.xml"
```
#### <a name="additional-references"></a>Referencias adicionales
La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando copy-Image](using-the-copy-image-command.md)
[mediante el comando export-Image](using-the-export-image-command.md)
[mediante el comando Get-Image](using-the-get-image-command.md)
[con el comando Remove-image](using-the-remove-image-command.md)
[mediante Comando Replace-Image](using-the-replace-image-command.md)1[subcomando: set-Image](subcommand-set-image.md)
