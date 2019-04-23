---
title: Mediante el comando Agregar dispositivo
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85e9ef4445b4dabbe85c2397d62b06756e17879d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878546"
---
# <a name="using-the-add-device-command"></a>Mediante el comando Agregar dispositivo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Preconfigura un equipo en servicios de dominio de active directory. Los equipos preconfigurados también se denominan equipos conocidos. Esto le permite configurar las propiedades para controlar la instalación del cliente. Por ejemplo, puede configurar el programa de arranque de red y el archivo de instalación desatendida que debe recibir el cliente, así como el servidor desde el cual el cliente debe descargar el programa de arranque de red.
Para obtener ejemplos de cómo se puede utilizar este comando, consulte [ejemplos](#BKMK_examples).
## <a name="syntax"></a>Sintaxis
```
wdsutil /add-Device /Device:<Device name> /ID:<UUID | MAC address> [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/OU:<DN of OU>] [/Domain:<Domain>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/ Dispositivo:<computer name>|Especifica el nombre del equipo va a agregar.|
|/ ID: < UUID &#124; dirección MAC >|Especifica el GUID/UUID o la dirección MAC del equipo. Debe ser un GUID/UUID en uno de cadena binaria de los dos formatos o cadena GUID. Por ejemplo:<br /><br />Cadena binaria: **/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6**<br /><br />Cadena GUID: **/ID:E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br /><br />Debe ser una dirección MAC en el formato siguiente: **00B056882FDC** (sin guiones) o **00-B0-56-88-2F-DC** (con guiones)|
|[/ ReferralServer:<Server name>]|Especifica el nombre del servidor que se va a ser contactado para descargar el programa de arranque de red y la imagen de arranque mediante el protocolo Trivial de transferencia de archivos (tftp).|
|[/BootProgram:<Relative path>]|Especifica la ruta de acceso relativa desde la carpeta remoteInstall el programa de arranque de red que debe recibir este equipo. Por ejemplo: "boot\x86\pxeboot.com"|
|[/WdsClientUnattend:<Relative path>]|Especifica la ruta de acceso relativa desde la carpeta remoteInstall en el archivo de instalación desatendida que automatiza las pantallas de instalación del cliente de servicios de implementación de Windows.|
|[/User:<Domain\User &#124; User@Domain>]|Establece permisos en el objeto de cuenta de equipo para proporcionar al usuario especificado los derechos necesarios para unir el equipo al dominio.|
|[/JoinRights:{JoinOnly &#124; Full}]|Especifica el tipo de derechos que se asignará al usuario.<br /><br />-   **JoinOnly** requiere que el administrador restablecer la cuenta de equipo antes de que el usuario puede unir el equipo al dominio.<br />-   **Completa** proporciona acceso total al usuario, que incluye el derecho de unir el equipo al dominio.|
|[/ JoinDomain: {Sí &#124; n}]|Especifica si el equipo debe estar unido al dominio como esta cuenta de equipo durante la instalación del sistema operativo. El valor predeterminado es **Sí**.|
|[/BootImagepath:<Relative path>]|Especifica la ruta de acceso relativa desde la carpeta remoteInstall la imagen de arranque que debe usar este equipo.|
|[/OU:<DN of OU>]|El nombre distintivo de la unidad organizativa donde se debe crear el objeto de cuenta de equipo. Por ejemplo: **OU = Miuo, CN = Test, DC = Domain, DC = com**. La ubicación predeterminada es el contenedor del equipo predeterminado.|
|[/Domain:<Domain>]|El dominio donde se debe crear el objeto de cuenta de equipo. La ubicación predeterminada es el dominio local.|
## <a name="BKMK_examples"></a>Ejemplos
Para agregar un equipo con una dirección MAC, escriba:
```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```
Para agregar un equipo mediante una cadena GUID, escriba:
```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com 
/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:"OU=MyOU,CN=Test,DC=Domain,DC=com"
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando get-AllDevices](using-the-get-alldevices-command.md)
[mediante el comando get-dispositivo](using-the-get-device-command.md)
[subcomando: Set-Device](subcommand-set-device.md)
[WdsClient de nuevo](https://technet.microsoft.com/library/dn283430.aspx)
