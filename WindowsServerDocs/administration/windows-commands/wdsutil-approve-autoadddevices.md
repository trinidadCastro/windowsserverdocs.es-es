---
title: WDSUtil aprobar-AutoAddDevices
description: Artículo de referencia del comando WDSUtil APPROVE-AutoAddDevices, que aprueba los equipos que están pendientes de aprobación administrativa.
ms.topic: reference
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 31dba6d0a0bb585f61433f86d1aa0ae4388fe484
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524940"
---
# <a name="wdsutil-approve-autoadddevices"></a>WDSUtil aprobar-AutoAddDevices

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Aprueba los equipos que están pendientes de aprobación administrativa. Cuando se habilita la Directiva de adición automática, se requiere la aprobación administrativa antes de que los equipos desconocidos (los que no están preconfigurados) puedan instalar una imagen. Puede habilitar esta Directiva mediante la pestaña **respuesta PXE** de la página Propiedades del servidor.

## <a name="syntax"></a>Sintaxis

```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| Servidor`<Servername>` | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
| RequestId`{Request ID|ALL}` | Especifica el identificador de solicitud asignado al equipo pendiente. Especifique **All** para aprobar todos los equipos pendientes. |
| MachineName`<Devicename>` | Especifica el nombre del dispositivo que se va a agregar. No se puede usar esta opción cuando se aprueban todos los equipos. |
| [/OU: `<DN of OU>` ] | Nombre distintivo de la unidad organizativa en la que se debe crear el objeto de cuenta de equipo. Por ejemplo: **ou = miuo, CN = test, DC = Domain, DC = com**. La ubicación predeterminada es el contenedor del equipo predeterminado. |
| [/User: `<Domain\User|User@Domain>` ] | Establece permisos en el objeto de cuenta de equipo para conceder al usuario especificado los derechos necesarios para unir el equipo al dominio. |
| [/JoinRights: `{JoinOnly|Full}` ] | Especifica el tipo de derechos que se asignará al usuario.<ul><li>**JoinOnly** : requiere que el administrador restablezca la cuenta de equipo antes de que el usuario pueda unir el equipo al dominio.</li><li>**Completo** : proporciona acceso completo al usuario, que incluye el derecho para unir el equipo al dominio. |
| [/JoinDomain: `{Yes|No}` ] | Especifica si el equipo debe estar unido al dominio como esta cuenta de equipo durante la instalación del sistema operativo. El valor predeterminado es **sí**. |
| [/ReferralServer: `<Servername>` ] | Especifica el nombre del servidor con el que ponerse en contacto para descargar el programa de arranque de red y la imagen de arranque mediante el File Transfer Protocol trivial (TFTP). |
| [/BootProgram: `<Relativepath>` ] | Especifica la ruta de acceso relativa de la carpeta **remoteInstall** al programa de arranque de red que este equipo debe recibir. Por ejemplo: **boot\x86\pxeboot.com**. |
| [/WdsClientUnattend: `<Relativepath>` ] | Especifica la ruta de acceso relativa de la carpeta **remoteInstall** al archivo de instalación desatendida que automatiza el cliente de servicios de implementación de Windows. |
| [/BootImagepath: `<Relativepath>` ] | Especifica la ruta de acceso relativa de la carpeta remoteInstall a la imagen de arranque que este equipo debe recibir. |

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

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando WDSUtil Delete-AutoAddDevices](wdsutil-delete-autoadddevices.md)

- [WDSUtil Get-AutoAddDevices (comando)](wdsutil-get-autoadddevices.md)

- [WDSUtil-AutoAddDevices (comando)](wdsutil-reject-autoadddevices.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
