---
title: WDSUtil-add-device
description: Artículo de referencia del comando WDSUtil add-device, que preensaya un equipo en Active Directory Domain Services.
ms.topic: reference
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: de05d5510e61f5cba0813a7e11215935fd380968
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524641"
---
# <a name="wdsutil-add-device"></a>WDSUtil-add-device

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Ensaya previamente un equipo en Active Directory Domain Services (AD DS). Los equipos preconfigurados también se denominan *equipos conocidos*. Esto le permite configurar las propiedades para controlar la instalación del cliente. Por ejemplo, puede configurar el programa de arranque de red y el archivo de instalación desatendida que debe recibir el cliente, así como el servidor desde el que el cliente debe descargar el programa de arranque de red.

## <a name="syntax"></a>Sintaxis

```
wdsutil /add-Device /Device:<Devicename> /ID:<UUID | MAC address> [/ReferralServer:<Servername>] [/BootProgram:<Relativepath>] [/WdsClientUnattend:<Relativepath>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relativepath>] [/OU:<DN of OU>] [/Domain:<Domain>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| Dispositivos`<Devicename>` | Especifica el nombre del dispositivo que se va a agregar. |
| Sesión`<UUID|MAC address>` | Especifica el GUID/UUID o la dirección MAC del equipo. Un GUID/UUID debe estar en uno de los dos formatos siguientes: cadena binaria ( `/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6` ) o cadena GUID ( `/ID:E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6` ). Una dirección MAC debe tener el formato siguiente: **00B056882FDC** (sin guiones) o **00-B0-56-88-2F-DC** (con guiones). |
| [/ReferralServer: `<Servername>` ] | Especifica el nombre del servidor con el que se va a establecer contacto para descargar el programa de arranque de red y la imagen de arranque mediante el File Transfer Protocol trivial (TFTP). |
| [/BootProgram: `<Relativepath>` ] | Especifica la ruta de acceso relativa de la carpeta **remoteInstall** al programa de arranque de red que este equipo debe recibir. Por ejemplo: `boot\x86\pxeboot.com` |
| [/WdsClientUnattend: `<Relativepath>` ] | Especifica la ruta de acceso relativa de la carpeta **remoteInstall** al archivo de instalación desatendida que automatiza las pantallas de instalación del cliente de servicios de implementación de Windows. |
| [/User: `<Domain\User|User@Domain>` ] | Establece permisos en el objeto de cuenta de equipo para conceder al usuario especificado los derechos necesarios para unir el equipo al dominio. |
| [/JoinRights: `{JoinOnly|Full}` ] | Especifica el tipo de derechos que se asignará al usuario.<ul><li>**JoinOnly** : requiere que el administrador restablezca la cuenta de equipo antes de que el usuario pueda unir el equipo al dominio.</li><li>**Completo** : proporciona acceso completo al usuario, que incluye el derecho para unir el equipo al dominio. |
| [/JoinDomain: `{Yes|No}` ] | Especifica si el equipo debe estar unido al dominio como esta cuenta de equipo durante la instalación del sistema operativo. El valor predeterminado es **sí**. |
| [/BootImagepath: `<Relativepath>` ] | Especifica la ruta de acceso relativa de la carpeta **remoteInstall** a la imagen de arranque que este equipo debe usar. |
| [/OU: `<DN of OU>` ] | Nombre distintivo de la unidad organizativa en la que se debe crear el objeto de cuenta de equipo. Por ejemplo: **ou = miuo, CN = test, DC = Domain, DC = com**. La ubicación predeterminada es el contenedor del equipo predeterminado. |
| [/Domain: `<Domain>` ] | Dominio en el que se debe crear el objeto de cuenta de equipo. La ubicación predeterminada es el dominio local. |

## <a name="examples"></a>Ejemplos

Para agregar un equipo mediante una dirección MAC, escriba:

```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```

Para agregar un equipo mediante una cadena de GUID, escriba:

```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:OU=MyOU,CN=Test,DC=Domain,DC=com
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil Get-alldevices (comando)](wdsutil-get-alldevices.md)

- [comando Get-Device de WDSUtil](wdsutil-get-device.md)

- [comando set-device de WDSUtil](wdsutil-set-device.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)

- [Nuevo: Cliente WDS](/powershell/module/wds/New-WdsClient)
