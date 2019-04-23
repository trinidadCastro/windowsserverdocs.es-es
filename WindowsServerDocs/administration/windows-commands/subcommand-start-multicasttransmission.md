---
title: Subcomando start-MulticastTransmission
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1b2d459-1ece-49d4-997c-9d206c463b61
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7e3e59a0907caf2769d5df00aeaf00589ab450d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842686"
---
# <a name="subcommand-start-multicasttransmission"></a>Subcomando: start-MulticastTransmission

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

inicia una transmisión de difusión programada de una imagen.
## <a name="syntax"></a>Sintaxis
**Windows Server 2008**
```
wdsutil /start-MulticastTransmissiomedia:<Image name> [/Server:<Server namemediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
**Windows Server 2008 R2** para imágenes de arranque:
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
las imágenes de instalación:
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
Medio:<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
tipo de medio: {instalar&#124;arranque}|Especifica el tipo de imagen. Tenga en cuenta que esta opción debe establecerse en **instalar** para Windows Server 2008.|
|/ Arquitectura: {x86 &#124; ia64 &#124; x64}|La arquitectura de la imagen de arranque que está asociada con la transmisión a iniciar. Puesto que es posible tener el mismo nombre de imagen para las imágenes de arranque en arquitecturas diferentes, debe especificar la arquitectura para asegurarse de que se utiliza la transmisión correcta.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes de la imagen. Si se especifica ningún nombre de grupo de imágenes y grupo de solo imágenes existe en el servidor, se usará ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar el nombre del grupo de imágenes.|
|[/Filename:<File name>]|Especifica el nombre del archivo que contiene la imagen. Si la imagen no se identifica por nombre, debe usar esta opción para especificar el nombre de archivo.|
## <a name="BKMK_examples"></a>Ejemplos
Para iniciar una transmisión por multidifusión, escriba uno de los siguientes:
```
wdsutil /start-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
Para iniciar un arranque de transmisión por multidifusión para Windows Server 2008 R2, tipo de imagen:
```
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"X64 Boot Imagemediatype:Boot /Architecture:x64
/Filename:boot.wim\n\
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[con el comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md) 
 [Con el comando nueva-MulticastTransmission](using-the-new-multicasttransmission-command.md)
[con el comando remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
