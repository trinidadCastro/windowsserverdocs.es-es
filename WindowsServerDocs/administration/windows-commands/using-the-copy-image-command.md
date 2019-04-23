---
title: Uso de la comandos de Copiar imagen
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52c9c0bb45e60e76077bf90534e93f2c6fc1df18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884046"
---
# <a name="using-the-copy-image-command"></a>Uso de la comandos de Copiar imagen

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imágenes de copias que están dentro del mismo grupo de imágenes. Para copiar imágenes entre grupos de imágenes, use la [con el comando Export-Image](using-the-export-image-command.md) comando y, a continuación, el [mediante el comando add-Image](using-the-add-image-command.md) comando.
Para obtener ejemplos de cómo se puede utilizar este comando, consulte [ejemplos](#BKMK_examples).
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /copy-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Name:<Name>
         /Filename:<File name>
         [/Description:<Description>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
Medio:<Image name>|Especifica el nombre de la imagen que se va a copiar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
mediatype:Install|Especifica el tipo de imagen que se copiará. Esta opción debe establecerse en **instalar**.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen que se va a copiar. Si no se especifica ningún grupo de imágenes y un único grupo existe en el servidor, se usará ese grupo de imágenes de forma predeterminada. Si existen varios grupos de imágenes en el servidor, debe especificar el grupo de imágenes.|
|[/Filename:<Filename>]|Especifica el nombre de archivo de la imagen que se va a copiar. Si la imagen de origen no se identifica por nombre, debe especificar el nombre de archivo.|
|/DestinationImage|Especifica la configuración de la imagen de destino, como se describe en la tabla siguiente.<br /><br />-/ Name:<Name> -establece el nombre para mostrar de la imagen que se va a copiar.<br />-/ FileName:<Filename> -establece el nombre del archivo de imagen de destino que contendrá la copia de la imagen.<br />-[/ Descripción: <Description>]-Establece la descripción de la copia de la imagen.|
## <a name="BKMK_examples"></a>Ejemplos
Para crear una copia de la imagen especificada y asígnele el nombre WindowsVista.wim, escriba:
```
wdsutil /copy-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim"
```
Para crear una copia de la imagen especificada, se aplica la configuración especificada y el nombre de WindowsVista.wim, tipo:
```
wdsutil /verbose /Progress /copy-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim" /Description:"This is a copy of the original Windows image with Office installed"
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando add-Image](using-the-add-image-command.md)
[con el comando Export-Image](using-the-export-image-command.md)
[mediante el Comando Get-imagen](using-the-get-image-command.md)
[mediante el comando remove-Image](using-the-remove-image-command.md)
[mediante el comando replace-Image](using-the-replace-image-command.md) 
 [ Subcomando: set-Image](subcommand-set-image.md)
