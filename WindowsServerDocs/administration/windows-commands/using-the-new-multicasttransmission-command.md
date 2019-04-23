---
title: Mediante el comando nueva-MulticastTransmission
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1f1dc46-dd50-4eb9-9f72-cf0e5d71bd3d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84f5aa69f2d4a875995ac6c18fa43bd68518fba3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875146"
---
# <a name="using-the-new-multicasttransmission-command"></a>Mediante el comando nueva-MulticastTransmission

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crea una nueva transmisión por multidifusión para una imagen. Este comando es equivalente a la creación de una transmisión mediante el complemento mmc de servicios de implementación de Windows (haga clic en el **transmisiones por multidifusión** nodo y, a continuación, haga clic en **crear transmisión por multidifusión**). Debe usar este comando cuando tenga el servicio de rol de servidor de implementación y el servicio de rol de servidor de transporte instalado (que es la instalación predeterminada). Si tiene sólo el servicio de rol de servidor de transporte instalado, use [con el comando nuevo Namespace](using-the-new-namespace-command.md).
## <a name="syntax"></a>Sintaxis
para las transmisiones de imágenes de instalación:
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
para las transmisiones de imagen de arranque (solo se admite para Windows Server 2008 R2):
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
Medio:<Image name>|Especifica el nombre de la imagen que se transmiten mediante multidifusión.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|/FriendlyName:<Friendly name>|Especifica el nombre descriptivo de la transmisión.|
|[/ Descripción:<Description>]|Especifica la descripción de la transmisión.|
tipo de medio: {arranque&#124;instalar}|Especifica el tipo de imagen que se transmiten mediante multidifusión. Tenga en cuenta **arranque** solo se admite para Windows Server 2008 R2.|
|\mediaGroup:<Image group name>]|Especifica el grupo de imágenes que contiene la imagen. Si se especifica ningún nombre de grupo de imágenes y grupo de solo imágenes existe en el servidor, se utiliza ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar el nombre del grupo de imágenes.|
|[/Filename:<File name>]|Especifica el nombre de archivo. Si la imagen de origen no se identifica por nombre, debe usar esta opción para especificar el nombre de archivo.|
|/Transmissiontype:{AutoCast &#124; ScheduledCast}|Especifica si se debe iniciar automáticamente la transmisión (AutoCast) o según los criterios de inicio especificada (difusión programada).<br /><br /><ul><li>**Difusión automática**. Este tipo de transmisión indica que tan pronto como un cliente aplicable solicite una imagen de instalación, comienza una transmisión por multidifusión de la imagen seleccionada. Cuando otros clientes soliciten la misma imagen, están unidas a la transmisión que ya se ha iniciado.</li><li>**Difusión programada**. Este tipo de transmisión establece los criterios de inicio para la transmisión en función del número de clientes que soliciten una imagen o una fecha y hora determinada. Puede especificar las siguientes opciones:<br /><br /><ul><li>[/ time: <time>]-establece el tiempo que se debe iniciar la transmisión mediante el formato siguiente: AAAA/MM/número_clientes.</li><li>[/ Clientes: <Number of clients>]-establece el número mínimo de clientes que se esperan antes de inicia la transmisión.</li></ul></li></ul>|
|/ Arquitectura: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen de arranque para transmitir mediante multidifusión. Dado que es posible tener el mismo nombre para las imágenes de arranque de arquitecturas diferentes, debe especificar la arquitectura para asegurarse de que se usa la imagen correcta.|
|[/Filename:<File name>]|Especifica el nombre de archivo. Si la imagen de origen no se identifica por nombre, debe especificar el nombre de archivo.|
## <a name="BKMK_examples"></a>Ejemplos
Para crear una transmisión de difusión automática de una imagen de arranque en Windows Server 2008 R2, escriba:
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS Boot Transmission"
/Image:"X64 Boot Imagemediatype:Boot /Architecture:x64 /Transmissiontype:AutoCast
```
Para crear una transmisión de difusión automática de una imagen de instalación, escriba:
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS AutoCast Transmission"
/Image:"Vista with Officemediatype:Install /Transmissiontype:AutoCast
```
Para crear una transmisión de difusión programada de una imagen de instalación, escriba:
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS SchedCast Transmission" /Server:MyWDSServemedia:"Vista with Officemediatype:Install 
/Transmissiontype:ScheduledCast /time:"2006/11/20:17:00" /Clients:100
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[con el comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md) 
 [Con el comando remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
[subcomando: start-MulticastTransmission](subcommand-start-multicasttransmission.md)
