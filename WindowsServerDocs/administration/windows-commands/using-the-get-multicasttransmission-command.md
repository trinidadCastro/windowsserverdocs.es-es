---
title: Usar el comando Get-MulticastTransmission
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b733737b-1e81-43d4-a058-d6985a613bef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54c503abda871d1f2a4fd8a30d7f12317eee6a48
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392127"
---
# <a name="using-the-get-multicasttransmission-command"></a>Usar el comando Get-MulticastTransmission

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
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
medios: <Image name>|Muestra la transmisión de multidifusión asociada a esta imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
mediatype: instalación|Especifica el tipo de imagen. Tenga en cuenta que esta opción debe estar configurada para **instalar**.|
|\mediaGroup: <Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utiliza ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar un grupo de imágenes.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen de arranque que está asociada a la transmisión. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en diferentes arquitecturas, debe especificar la arquitectura para asegurarse de que se usa la imagen correcta.|
|[/Filename:<File name>]|Especifica el archivo que contiene la imagen. Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.|
|[/Show: clients]<br /><br />o bien<br /><br />[/Details: clientes]|Muestra información acerca de los equipos cliente que están conectados a la transmisión por multidifusión.|
## <a name="BKMK_examples"></a>Example
**Windows Server 2008** Para ver información sobre la transmisión de una imagen denominada vista con Office, escriba uno de los siguientes:
```
wdsutil /Get-MulticastTransmissiomedia:"Vista with Officemediatype:Install
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /Show:Clients
```
**Windows Server 2008 R2** Para ver información sobre la transmisión de una imagen denominada vista con Office, escriba uno de los siguientes:
```
wdsutil /Get-MulticastTransmissiomedia:"Vista with Office"
 /Imagetype:Install
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /details:Clients
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"X64 Boot Imagemediatype:Boot /Architecture:x64 /Filename:boot.wim /details:Clients
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[con el comando New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
[mediante el comando Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
[ Subcomando: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
