---
title: Subcomando set-Device
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 401567f8-eaeb-4a2d-b811-140bb007028d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80bb9144936cf493784603bcbdb8a0d1e5c870bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848066"
---
# <a name="subcommand-set-device"></a>Subcomando: set-Device

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia los atributos de un equipo preconfigurado. Un equipo preconfigurado es un equipo que se ha vinculado a un objeto de cuenta de equipo en los servidores de dominio de Active Directory (AD DS). Los clientes preconfigurados también se denominan equipos conocidos. Puede configurar las propiedades de la cuenta de equipo para controlar la instalación del cliente. Por ejemplo, puede configurar el programa de arranque de red y el archivo de instalación desatendida que debe recibir el cliente, así como el servidor desde el cual el cliente debe descargar el programa de arranque de red.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] 
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/ Dispositivo:<computer name>|Especifica el nombre del equipo (nombre de cuenta SAM).|
|[/ ID: < UUID &#124; dirección MAC >]|Especifica el GUID/UUID o la dirección MAC del equipo. Este valor debe estar en uno de los tres formatos siguientes:<br /><br />-Cadena binaria: **/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-Cadena GUID/UUID: / ID:**E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br />-Dirección MAC: **00B056882FDC** (sin guiones) o **00-B0-56-88-2F-DC** (con guiones)|
|[/ ReferralServer:<Server name>]|Especifica el nombre del servidor que se va a ser contactado para descargar la imagen arranque y el programa de arranque red mediante el protocolo Trivial de transferencia de archivos (tftp).|
|[/BootProgram:<Relative path>]|Especifica la ruta de acceso relativa desde la carpeta remoteInstall el programa de arranque de red que recibirá el equipo especificado. Por ejemplo: **boot\x86\pxeboot.com**|
|[/WdsClientUnattend:<Relative path>]|Especifica la ruta de acceso relativa desde la carpeta remoteInstall en el archivo de instalación desatendida que automatiza las pantallas de instalación para el cliente de servicios de implementación de Windows.|
|[/User:<Domain\User &#124; User@Domain>]|Establece permisos en el objeto de cuenta de equipo para proporcionar al usuario especificado los derechos necesarios para unir el equipo al dominio.|
|[/JoinRights:{JoinOnly &#124; Full}]|Especifica el tipo de derechos que se asignará al usuario.<br /><br />-   **JoinOnly** requiere que el administrador restablecer la cuenta de equipo antes de que el usuario puede unir el equipo al dominio.<br />-   **Completa** proporciona acceso total al usuario, incluido el derecho de unir el equipo al dominio.|
|[/ JoinDomain: {Sí &#124; n}]|Especifica si el equipo debe estar unido al dominio como esta cuenta de equipo durante una instalación de servicios de implementación de Windows. El valor predeterminado es **Sí**.|
|[/BootImagepath:<Relative path>]|Especifica la ruta de acceso relativa desde la carpeta remoteInstall la imagen de arranque que usará el equipo.|
|[/Domain:<Domain>]|Especifica el dominio que se va a buscar el equipo preconfigurado. El valor predeterminado es el dominio local.|
|[/resetAccount]|Restablece los permisos en el equipo especificado para que cualquier persona que tenga los permisos adecuados pueda unirse al dominio mediante el uso de esta cuenta.|
## <a name="BKMK_examples"></a>Ejemplos
Para configurar el servidor de referencia y el programa de arranque de red para un equipo, escriba:
```
wdsutil /Set-Device /Device:computer1 /ReferralServer:MyWDSServer
/BootProgram:boot\x86\pxeboot.n12
```
Para establecer diversas opciones para un equipo, escriba:
```
wdsutil /verbose /Set-Device /Device:computer2 /ID:00-B0-56-88-2F-DC /WdsClientUnattend:WDSClientUnattend\unattend.xml 
/User:Domain\user /JoinRights:JoinOnly /JoinDomain:No /BootImagepath:boot\x86\images\boot.wim /Domain:NorthAmerica /resetAccount
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando Agregar dispositivos](using-the-add-device-command.md)
[con el comando get-AllDevices](using-the-get-alldevices-command.md)
[mediante el Comando Get-dispositivo](using-the-get-device-command.md)
