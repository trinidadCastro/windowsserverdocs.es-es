---
title: 'Conjunto de subcomandos: dispositivo'
description: Tema de comandos de Windows para el subcomando set-device, que cambia los atributos de un equipo preconfigurado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 401567f8-eaeb-4a2d-b811-140bb007028d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ced5f44c9edda0176c93479ded60d809644bf4fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834048"
---
# <a name="subcommand-set-device"></a>Subcomando: set-device

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia los atributos de un equipo preconfigurado. Un equipo preconfigurado es un equipo que se ha vinculado a un objeto de cuenta de equipo en servidores de dominio de Active Directory (AD DS). Los clientes preconfigurados también se denominan equipos conocidos. Puede configurar las propiedades de la cuenta de equipo para controlar la instalación del cliente de. Por ejemplo, puede configurar el programa de arranque de red y el archivo de instalación desatendida que debe recibir el cliente, así como el servidor desde el que el cliente debe descargar el programa de arranque de red.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] 
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/Device:<computer name>|Especifica el nombre del equipo (SAM-Account-Name).|
|[/ID: < dirección &#124; Mac de UUID >]|Especifica el GUID/UUID o la dirección MAC del equipo. Este valor debe estar en uno de los tres formatos siguientes:<p>-Cadena binaria: **/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-GUID/UUID cadena:/ID:**E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br />-Dirección MAC: **00B056882FDC** (sin guiones) o **00-B0-56-88-2F-DC** (con guiones)|
|[/ReferralServer:<Server name>]|Especifica el nombre del servidor con el que se contactará para descargar el programa de arranque de red y la imagen de arranque mediante el File Transfer Protocol trivial (TFTP).|
|[/BootProgram:<Relative path>]|Especifica la ruta de acceso relativa de la carpeta remoteInstall al programa de arranque de red que recibirá el equipo especificado. Por ejemplo: **boot\x86\pxeboot.com**|
|[/WdsClientUnattend:<Relative path>]|Especifica la ruta de acceso relativa de la carpeta remoteInstall al archivo de instalación desatendida que automatiza las pantallas de instalación del cliente de servicios de implementación de Windows.|
|[/User: < Dominio\usuario &#124; User@Domain>]|Establece permisos en el objeto de cuenta de equipo para conceder al usuario especificado los derechos necesarios para unir el equipo al dominio.|
|[/JoinRights: {JoinOnly &#124; Full}]|Especifica el tipo de derechos que se asignará al usuario.<p>-   **JoinOnly** requiere que el administrador restablezca la cuenta de equipo antes de que el usuario pueda unir el equipo al dominio.<br />-   **Full** proporciona acceso completo al usuario, incluido el derecho a unir el equipo al dominio.|
|[/JoinDomain: {Yes &#124; no}]|Especifica si el equipo debe unirse al dominio como esta cuenta de equipo durante una instalación de servicios de implementación de Windows. El valor predeterminado es **sí**.|
|[/BootImagepath:<Relative path>]|Especifica la ruta de acceso relativa de la carpeta remoteInstall a la imagen de arranque que el equipo va a usar.|
|[/Domain:<Domain>]|Especifica el dominio en el que se va a buscar el equipo preconfigurado. El valor predeterminado es el dominio local.|
|[/resetAccount]|restablece los permisos en el equipo especificado para que todos los usuarios con los permisos adecuados puedan unirse al dominio mediante esta cuenta.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para establecer el programa de arranque de red y el servidor de referencia para un equipo, escriba:
```
wdsutil /Set-Device /Device:computer1 /ReferralServer:MyWDSServer
/BootProgram:boot\x86\pxeboot.n12
```
Para establecer varias opciones de configuración para un equipo, escriba:
```
wdsutil /verbose /Set-Device /Device:computer2 /ID:00-B0-56-88-2F-DC /WdsClientUnattend:WDSClientUnattend\unattend.xml 
/User:Domain\user /JoinRights:JoinOnly /JoinDomain:No /BootImagepath:boot\x86\images\boot.wim /Domain:NorthAmerica /resetAccount
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando add-device](using-the-add-device-command.md)
[mediante el comando Get-AllDevices](using-the-get-alldevices-command.md)
[mediante el comando Get-Device](using-the-get-device-command.md)
