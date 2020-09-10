---
title: 'Aprobar: AutoaddDevices'
description: Artículo de referencia para APPROVE-AutoaddDevices, que aprueba equipos que están pendientes de aprobación administrativa.
ms.topic: reference
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e0f44d647eb2934a3acbc5cc78502240a2f18031
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622197"
---
# <a name="approve-autoadddevices"></a>Aprobar: AutoaddDevices

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Aprueba los equipos que están pendientes de aprobación administrativa. Cuando se habilita la Directiva de adición automática, se requiere la aprobación administrativa antes de que los equipos desconocidos (los que no están preconfigurados) puedan instalar una imagen. Puede habilitar esta Directiva mediante la pestaña **respuesta PXE** de la página Propiedades del servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>]
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica un nombre de servidor, se usará el servidor local.|
|/RequestId: {ID. de solicitud &#124; todos}|Especifica el identificador de solicitud asignado al equipo pendiente. Especifique **All** para aprobar todos los equipos pendientes.|
|[/MachineName: <Device name> ]|Especifica el nombre del equipo que se va a agregar. No se puede usar esta opción cuando se aprueban todos los equipos.|
|[/OU: <DN of OU> ]|Especifica el nombre distintivo de la unidad organizativa (OU) en la que se debe crear el objeto de cuenta de equipo. Por ejemplo: **ou = miuo, CN = test, DC = Domain, DC = com**. La ubicación predeterminada es el contenedor del equipo predeterminado.|
|[/User: <Dominio\usuario &#124; User@Domain>]|Establece permisos en el objeto de cuenta de equipo para asignar al usuario especificado los derechos necesarios.|
|[/JoinRights: {JoinOnly &#124; Full}]|Especifica el tipo de derechos que se asignará al usuario especificado.<p>-   **JoinOnly** requiere que el administrador restablezca la cuenta de equipo antes de que el usuario pueda unir el equipo al dominio.<br />-   **Full** proporciona acceso completo al usuario, que incluye el derecho para unir el equipo al dominio.|
|[/JoinDomain: {Yes &#124; no}]|Especifica si el equipo debe unirse al dominio como esta cuenta de equipo durante la instalación del sistema operativo. El valor predeterminado es **sí**.|
|[/ReferralServer: <Server name> ]|Especifica el nombre del servidor con el que se va a establecer la conexión para descargar el programa de arranque de red y la imagen de arranque mediante el File Transfer Protocol trivial (TFTP).|
|[/BootProgram: <Relative path> ]|Especifica la ruta de acceso relativa de la carpeta remoteInstall al programa de arranque de red que este equipo debe recibir. Por ejemplo: **boot\x86\pxeboot.com**.|
|[/WdsClientUnattend: <Relative path> ]|Especifica la ruta de acceso relativa de la carpeta remoteInstall al archivo de instalación desatendida que automatiza el cliente de servicios de implementación de Windows.|
|[/BootImagepath: <Relative path> ]|Especifica la ruta de acceso relativa de la carpeta remoteInstall a la imagen de arranque que este equipo debe recibir.|
## <a name="examples"></a>Ejemplos
Para aprobar el equipo con un RequestId de 12, escriba:
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
Para aprobar el equipo con un RequestID de 20 e implementar la imagen con la configuración especificada, escriba:
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:OU=Test,CN=company,DC=Domain,DC=Com /User:Domain\User1
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
Para aprobar todos los equipos pendientes, escriba:
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-delete-autoadddevices-command.md) 
 Delete-AutoaddDevices [Usar el comando](using-the-get-autoadddevices-command.md) 
 Get-AutoaddDevices [Usar el comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
