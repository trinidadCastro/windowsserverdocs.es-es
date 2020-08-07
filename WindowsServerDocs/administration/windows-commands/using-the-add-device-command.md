---
title: Agregar dispositivo
description: Artículo de referencia para add-device, que preconfigura un equipo en servicios de dominio de Active Directory. Los equipos preconfigurados también se denominan equipos conocidos.
ms.topic: article
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d23e5a2bc69b782e635fa9a47158274796715edf
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87897014"
---
# <a name="add-device"></a>Agregar dispositivo

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Preconfigura un equipo en servicios de dominio de Active Directory. Los equipos preconfigurados también se denominan equipos conocidos. Esto le permite configurar las propiedades para controlar la instalación del cliente. Por ejemplo, puede configurar el programa de arranque de red y el archivo de instalación desatendida que debe recibir el cliente, así como el servidor desde el que el cliente debe descargar el programa de arranque de red.

## <a name="syntax"></a>Sintaxis
```
wdsutil /add-Device /Device:<Device name> /ID:<UUID | MAC address> [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>]
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/OU:<DN of OU>] [/Domain:<Domain>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|Dispositivos<computer name>|Especifica el nombre del equipo que se va a agregar.|
|/ID: <UUID &#124; dirección MAC>|Especifica el GUID/UUID o la dirección MAC del equipo. Un GUID/UUID debe estar en uno de los dos formatos de cadena binaria o cadena de GUID. Por ejemplo:<p>Cadena binaria: **/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6**<p>Cadena GUID: **/ID: E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<p>Una dirección MAC debe tener el formato siguiente: **00B056882FDC** (sin guiones) o **00-B0-56-88-2F-DC** (con guiones).|
|[/ReferralServer: <Server name> ]|Especifica el nombre del servidor con el que se va a establecer contacto para descargar el programa de arranque de red y la imagen de arranque mediante el File Transfer Protocol trivial (TFTP).|
|[/BootProgram: <Relative path> ]|Especifica la ruta de acceso relativa de la carpeta remoteInstall al programa de arranque de red que este equipo debe recibir. Por ejemplo: boot\x86\pxeboot.com|
|[/WdsClientUnattend: <Relative path> ]|Especifica la ruta de acceso relativa de la carpeta remoteInstall al archivo de instalación desatendida que automatiza las pantallas de instalación del cliente de servicios de implementación de Windows.|
|[/User: <Dominio\usuario &#124; User@Domain>]|Establece permisos en el objeto de cuenta de equipo para conceder al usuario especificado los derechos necesarios para unir el equipo al dominio.|
|[/JoinRights: {JoinOnly &#124; Full}]|Especifica el tipo de derechos que se asignará al usuario.<p>-   **JoinOnly** requiere que el administrador restablezca la cuenta de equipo antes de que el usuario pueda unir el equipo al dominio.<br />-   **Full** proporciona acceso completo al usuario, que incluye el derecho para unir el equipo al dominio.|
|[/JoinDomain: {Yes &#124; no}]|Especifica si el equipo debe unirse al dominio como esta cuenta de equipo durante la instalación del sistema operativo. El valor predeterminado es **sí**.|
|[/BootImagepath: <Relative path> ]|Especifica la ruta de acceso relativa de la carpeta remoteInstall a la imagen de arranque que este equipo debe usar.|
|[/OU: <DN of OU> ]|Nombre distintivo de la unidad organizativa en la que se debe crear el objeto de cuenta de equipo. Por ejemplo: **ou = miuo, CN = test, DC = Domain, DC = com**. La ubicación predeterminada es el contenedor del equipo predeterminado.|
|[/Domain: <Domain> ]|Dominio en el que se debe crear el objeto de cuenta de equipo. La ubicación predeterminada es el dominio local.|
## <a name="examples"></a>Ejemplos
Para agregar un equipo mediante una dirección MAC, escriba:
```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```
Para agregar un equipo mediante una cadena de GUID, escriba:
```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com
/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:OU=MyOU,CN=Test,DC=Domain,DC=com
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-get-alldevices-command.md) 
 Get-AllDevices [Uso del comando](using-the-get-device-command.md) 
 Get-Device [Subcomando: set-device](subcommand-set-device.md) 
 [Nuevo: Cliente WDS](/previous-versions/windows/powershell-scripting/dn283430(v=wps.630))
