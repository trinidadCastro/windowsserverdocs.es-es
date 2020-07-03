---
title: Remove-MulticastTransmission
description: Artículo de referencia de Remove-MulticastTransmission, que deshabilita la transmisión por multidifusión para una imagen.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5f695e4743b06eb8a2e1c59081a4661e616c8711
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931234"
---
# <a name="using-the-remove-multicasttransmission-command"></a>Usar el comando Remove-MulticastTransmission

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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
soporte<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
mediatype: {instalar&#124;arranque}|Especifica el tipo de imagen. Tenga en cuenta que esta opción debe estar establecida en **instalar** para Windows Server 2008.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen de arranque que está asociada a la transmisión que se va a iniciar. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en diferentes arquitecturas, debe especificar la arquitectura para asegurarse de que se utiliza la transmisión correcta.|
|\mediaGroup: <Image group name> ]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utiliza ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar el nombre del grupo de imágenes.|
|[/Filename:<File name>]|especifica el nombre de archivo. Si la imagen de origen no se puede identificar de forma única mediante el nombre, debe usar esta opción para especificar el nombre de archivo.|
|/Force.|quita la transmisión y finaliza todos los clientes. A menos que especifique un valor para la opción **/Force** , los clientes existentes pueden completar la transferencia de imágenes, pero los clientes nuevos no podrán unirse.|
## <a name="examples"></a>Ejemplos
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
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-get-allmulticasttransmissions-command.md) 
 Get-AllMulticastTransmissions [Usar el comando](using-the-get-multicasttransmission-command.md) 
 Get-MulticastTransmission [Usar el comando](using-the-new-multicasttransmission-command.md) 
 New-MulticastTransmission [Subcomando: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
