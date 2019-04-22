---
title: Mediante el comando get-MulticastTransmission
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fdbb9283285bcf56cd83c18ea076e3d36a51b966
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823236"
---
# <a name="using-the-get-multicasttransmission-command"></a>Mediante el comando get-MulticastTransmission

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de la transmisión por multidifusión para una imagen especificada.
## <a name="syntax"></a>Sintaxis
**Windows Server 2008**
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] 
[/Filename:<File name>] [/Show:Clients]
```
**Windows Server 2008 R2** para las transmisiones de imagen de arranque:
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
    [/Server:<Server name>]
    [/details:Clients]
  mediatype:Boot
    /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
para las transmisiones de imagen de instalación:
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
Medio:<Image name>|Muestra la transmisión por multidifusión que está asociada con esta imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usa el servidor local.|
mediatype:Install|Especifica el tipo de imagen. Tenga en cuenta que esta opción debe establecerse en **instalar**.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si se especifica ningún nombre de grupo de imágenes y grupo de solo imágenes existe en el servidor, se utiliza ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar un grupo de imágenes.|
|/ Arquitectura: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen de arranque que está asociada con la transmisión. Dado que es posible tener el mismo nombre de imagen para las imágenes de arranque en arquitecturas diferentes, debe especificar la arquitectura para asegurarse de que se usa la imagen correcta.|
|[/Filename:<File name>]|Especifica el archivo que contiene la imagen. Si la imagen no se identifica por nombre, debe usar esta opción para especificar el nombre de archivo.|
|[/ Show: Clients]<br /><br />o bien<br /><br />[/ clientes: detalles]|Muestra información acerca de los equipos cliente que están conectados a la transmisión por multidifusión.|
## <a name="BKMK_examples"></a>Ejemplos
**Windows Server 2008** para ver información acerca de la transmisión de una imagen denominada Vista con Office, escriba uno de los siguientes:
```
wdsutil /Get-MulticastTransmissiomedia:"Vista with Officemediatype:Install
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /Show:Clients
```
**Windows Server 2008 R2** para ver información acerca de la transmisión de una imagen denominada Vista con Office, escriba uno de los siguientes:
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
[con el comando nueva-MulticastTransmission](using-the-new-multicasttransmission-command.md) 
 [Con el comando remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
[subcomando: start-MulticastTransmission](subcommand-start-multicasttransmission.md)
