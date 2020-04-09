---
title: Remove-MulticastTransmission
description: Tema de comandos de Windows para Remove-MulticastTransmission, que deshabilita la transmisión por multidifusión para una imagen.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36bb666b841c4c0c33f12c5ca766e619afc176ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830338"
---
# <a name="using-the-remove-multicasttransmission-command"></a>Usar el comando Remove-MulticastTransmission

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Deshabilita la transmisión por multidifusión para una imagen. A menos que especifique **/Force**, los clientes existentes completarán la transferencia de imágenes, pero no se permitirá que se unan a los clientes nuevos.

## <a name="syntax"></a>Sintaxis
**Windows Server 2008**
```
wdsutil /remove-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image Group>] [/Filename:<File name>] [/force]
```
**Windows Server 2008 R2** para imágenes de arranque:
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
\x20    [/Server:<Server name>]
\x20  mediatype:Boot
\x20    /Architecture:{x86 | ia64 | x64}
\x20    [/Filename:<File name>]
```
para imágenes de instalación:
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group
        [/Filename:<File name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
medios:<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
mediatype: {instalación&#124;de arranque}|Especifica el tipo de imagen. Tenga en cuenta que esta opción debe estar establecida en **instalar** para Windows Server 2008.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen de arranque que está asociada a la transmisión que se va a iniciar. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en diferentes arquitecturas, debe especificar la arquitectura para asegurarse de que se utiliza la transmisión correcta.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utiliza ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar el nombre del grupo de imágenes.|
|[/Filename:<File name>]|Especifica el nombre de archivo. Si la imagen de origen no se puede identificar de forma única mediante el nombre, debe usar esta opción para especificar el nombre de archivo.|
|/Force.|quita la transmisión y finaliza todos los clientes. A menos que especifique un valor para la opción **/Force** , los clientes existentes pueden completar la transferencia de imágenes, pero los clientes nuevos no podrán unirse.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para detener un espacio de nombres (los clientes actuales completarán la transmisión, pero los clientes nuevos no podrán unirse), escriba:
```
wdsutil /remove-MulticastTransmissiomedia:Vista with Office
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmissiomedia:x64 Boot Image
/Imagetype:Boot /Architecture:x64
```
Para forzar la finalización de todos los clientes, escriba:
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[mediante el comando Get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
[con el comando New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
[Subcommand: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
