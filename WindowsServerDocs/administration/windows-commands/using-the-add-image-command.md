---
title: Usar el comando add-Image
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0433e0775bd2088170ae17fcfe432cdaee0bf99d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817466"
---
# <a name="using-the-add-image-command"></a>Usar el comando add-Image

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agrega imágenes a un servidor de servicios de implementación de Windows. Para obtener ejemplos de cómo se puede utilizar este comando, consulte [ejemplos](#BKMK_examples).
## <a name="syntax"></a>Sintaxis
para imágenes de arranque, use la sintaxis siguiente:
```
wdsutil /add-ImagmediaFile:<wim file path> [/Server:<Server name>mediatype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>] 
[/Filename:<New wim file name>]
```
para las imágenes de instalación, use la sintaxis siguiente:
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
mediaFile: < ruta del archivo .wim >|Especifica la ruta de acceso y el nombre completo del archivo de imagen de Windows (.wim) que contiene las imágenes que se va a agregar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica un nombre de servidor, se usará el servidor local.|
tipo de medio: {arranque&#124;instalar}|Especifica el tipo de imágenes que se va a agregar.|
|[/Skipverify]|Especifica que la comprobación de integridad no se realizará en el archivo de imagen de origen antes de agrega la imagen.|
|[/ Name:<Name>]|Establece el nombre para mostrar de la imagen.|
|[/ Descripción:<Description>]|Establece la descripción de la imagen.|
|[/Filename:<Filename>]|Especifica el nuevo nombre de archivo para el archivo WIM. Esto le permite cambiar el nombre de archivo del archivo .wim al agregar la imagen. Si no se especifica ningún nombre de archivo, se usará el nombre de archivo de imagen de origen. En todos los casos, los servicios de implementación de Windows se comprueba para determinar si el nombre de archivo es único en el almacén de imágenes de arranque del equipo de destino.|
|\mediaGroup:<Image group name>]|Especifica el nombre del grupo de imágenes en el que las imágenes se van a ser agregados. Si existe más de un grupo de imágenes en el servidor, se debe especificar el grupo de imágenes. Si no está especificado y no existe en ese momento un grupo de imágenes, se creará uno nuevo. De otro modo, se usará el grupo de imágenes existente.|
|[/SingleImage:<Single image name>] [/Name:<Name>] [/Description:<Description>]|Copia la imagen especificada solo fuera de un archivo .wim y establece el nombre para mostrar la imagen y una descripción.|
|[/UnattendFile:<Unattend file path>]|Especifica la ruta de acceso completa al archivo de instalación desatendida que se asociará con las imágenes que se va a agregar. Si **/singleimage** no se especifica, el mismo archivo de instalación desatendida se asociará con todas las imágenes en el archivo WIM.|
## <a name="BKMK_examples"></a>Ejemplos
Para agregar una imagen de arranque, escriba:
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Boot.wimmediatype:Boot
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share\Boot.wim /Server:MyWDSServemediatype:Boot /Name:"My WinPE Image" 
/Description:"WinPE Image containing the WDS Client" /Filename:WDSBoot.wim
```
Para agregar una imagen de instalación, escriba uno de los siguientes:
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Install.wimmediatype:Install
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share \Install.wim /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/SingleImage:"Windows Pro" /Name:"My WDS Image"
/Description:"Windows Pro image with Microsoft Office" /Filename:"Win Pro.wim" /UnattendFile:"\\server\share\unattend.xml"
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando de Copiar imagen](using-the-copy-image-command.md)
[con el comando Export-Image](using-the-export-image-command.md)
[mediante el Comando Get-imagen](using-the-get-image-command.md)
[mediante el comando remove-Image](using-the-remove-image-command.md)
[mediante el comando replace-Image](using-the-replace-image-command.md) 
 [ Subcomando: set-Image](subcommand-set-image.md)
