---
title: Get-MulticastTransmission
description: Temas de comandos de Windows para Get-MulticastTransmission, que muestra información sobre la transmisión por multidifusión para una imagen especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b733737b-1e81-43d4-a058-d6985a613bef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec66107452be86423a4d542ee3999e86f1446656
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830948"
---
# <a name="get-multicasttransmission"></a>Get-MulticastTransmission

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información sobre la transmisión por multidifusión para una imagen especificada.

## <a name="syntax"></a>Sintaxis
**Windows Server 2008**
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] 
[/Filename:<File name>] [/Show:Clients]
```
**Windows Server 2008 R2** para transmisiones de imagen de arranque:
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
    [/Server:<Server name>]
    [/details:Clients]
  mediatype:Boot
    /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
para las transmisiones de imágenes de instalación:
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
         [/Server:<Server name>]
         [/details:Clients]
       mediatype:Install
    mediaGroup:<Image Group>]
     [/Filename:<File name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
medios:<Image name>|Muestra la transmisión de multidifusión asociada a esta imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
mediatype: instalación|Especifica el tipo de imagen. Tenga en cuenta que esta opción debe estar configurada para **instalar**.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utiliza ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar un grupo de imágenes.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen de arranque que está asociada a la transmisión. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en diferentes arquitecturas, debe especificar la arquitectura para asegurarse de que se usa la imagen correcta.|
|[/Filename:<File name>]|Especifica el archivo que contiene la imagen. Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.|
|[/Show: clients]<p>o bien<p>[/Details: clientes]|Muestra información acerca de los equipos cliente que están conectados a la transmisión por multidifusión.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
**Windows Server 2008** Para ver información sobre la transmisión de una imagen denominada vista con Office, escriba uno de los siguientes:
```
wdsutil /Get-MulticastTransmissiomedia:Vista with Officemediatype:Install
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /Show:Clients
```
**Windows Server 2008 R2** Para ver información sobre la transmisión de una imagen denominada vista con Office, escriba uno de los siguientes:
```
wdsutil /Get-MulticastTransmissiomedia:Vista with Office
 /Imagetype:Install
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /details:Clients
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:X64 Boot Imagemediatype:Boot /Architecture:x64 /Filename:boot.wim /details:Clients
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[con el comando New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
[con el comando Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
[Subcommand: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
