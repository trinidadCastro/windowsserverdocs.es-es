---
title: Con el comando remove-MulticastTransmission
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc3ba385644ef9da9b5d592142091ff087cd7545
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839686"
---
# <a name="using-the-remove-multicasttransmission-command"></a>Con el comando remove-MulticastTransmission

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Deshabilita la transmisión de multidifusión para una imagen. A menos que especifique **/force**, los clientes existentes completará la transferencia de imágenes, pero no se podrá unirse a nuevos clientes.
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
las imágenes de instalación:
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
Medio:<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usa el servidor local.|
tipo de medio: {instalar&#124;arranque}|Especifica el tipo de imagen. Tenga en cuenta que esta opción debe establecerse en **instalar** para Windows Server 2008.|
|/ Arquitectura: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen de arranque que está asociada con la transmisión al iniciar. Dado que es posible tener el mismo nombre de imagen para las imágenes de arranque en arquitecturas diferentes, debe especificar la arquitectura para asegurarse de que se utiliza la transmisión correcta.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si se especifica ningún nombre de grupo de imágenes y grupo de solo imágenes existe en el servidor, se utiliza ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar el nombre del grupo de imágenes.|
|[/Filename:<File name>]|Especifica el nombre de archivo. Si la imagen de origen no se identifica por nombre, debe usar esta opción para especificar el nombre de archivo.|
|[/force]|Quita la transmisión y finaliza a todos los clientes. A menos que especifique un valor para el **/force** opción existente de los clientes puede completar la transferencia de la imagen pero no están posible unirse a nuevos clientes.|
## <a name="BKMK_examples"></a>Ejemplos
Para detener un espacio de nombres (los clientes actuales completarán la transmisión, pero nuevos clientes no podrán unirse a), escriba:
```
wdsutil /remove-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmissiomedia:"x64 Boot Image"
/Imagetype:Boot /Architecture:x64
```
Para forzar la terminación de todos los clientes, escriba:
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[con el comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md) 
 [Con el comando nueva-MulticastTransmission](using-the-new-multicasttransmission-command.md)
[subcomando: start-MulticastTransmission](subcommand-start-multicasttransmission.md)
