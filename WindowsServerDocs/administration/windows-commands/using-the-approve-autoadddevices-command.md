---
title: Mediante el comando AutoaddDevices aprobar
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ce9824c45a00ccb9f1f9e357c7e3d36b2857f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886826"
---
# <a name="using-the-approve-autoadddevices-command"></a>Mediante el comando AutoaddDevices aprobar

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aprueba los equipos que están pendientes de aprobación administrativa. Cuando se habilita la directiva de adición automática, se requiere aprobación administrativa antes de que los equipos desconocidos (aquéllos que no están preconfigurados) puedan instalar una imagen. Puede habilitar esta directiva con el **respuesta PXE** ficha de la página de propiedades de servidor s.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica un nombre de servidor, se usará el servidor local.|
|/RequestId:{Request ID &#124; ALL}|Especifica el identificador de solicitud asignado al equipo pendiente. Especificar **todas** para aprobar todos los equipos pendientes.|
|[/MachineName:<Device name>]|Especifica el nombre del equipo va a agregar. No se puede usar esta opción al aprobar todos los equipos.|
|[/OU:<DN of OU>]|Especifica el nombre distintivo de la unidad organizativa (UO) donde se debe crear el objeto de cuenta de equipo. Por ejemplo: **OU = Miuo, CN = Test, DC = Domain, DC = com**. La ubicación predeterminada es el contenedor del equipo predeterminado.|
|[/User:<Domain\User &#124; User@Domain>]|Establece permisos en el objeto de cuenta de equipo para asignar al usuario especificado en los derechos necesarios.|
|[/JoinRights:{JoinOnly &#124; Full}]|Especifica el tipo de derechos que se asignará al usuario especificado.<br /><br />-   **JoinOnly** requiere que el administrador restablecer la cuenta de equipo antes de que el usuario puede unir el equipo al dominio.<br />-   **Completa** proporciona acceso total al usuario, que incluye el derecho de unir el equipo al dominio.|
|[/ JoinDomain: {Sí &#124; n}]|Especifica si el equipo debe estar unido al dominio como esta cuenta de equipo durante la instalación del sistema operativo. El valor predeterminado es **Sí**.|
|[/ ReferralServer:<Server name>]|Especifica el nombre del servidor que se va a ser contactado para descargar la imagen de arranque y el programa de arranque de red mediante el protocolo Trivial de transferencia de archivos (tftp).|
|[/BootProgram:<Relative path>]|Especifica la ruta de acceso relativa desde la carpeta remoteInstall el programa de arranque de red que debe recibir este equipo. Por ejemplo: **boot\x86\pxeboot.com**.|
|[/WdsClientUnattend:<Relative path>]|Especifica la ruta de acceso relativa desde la carpeta remoteInstall en el archivo de instalación desatendida que automatiza al cliente de servicios de implementación de Windows.|
|[/BootImagepath:<Relative path>]|Especifica la ruta de acceso relativa desde la carpeta remoteInstall la imagen de arranque que debe recibir este equipo.|
## <a name="BKMK_examples"></a>Ejemplos
Para aprobar el equipo con un Id. de solicitud de 12, escriba:
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
Para aprobar el equipo con un Id. de solicitud de 20 e implementar la imagen con la configuración especificada, escriba:
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:"OU=Test,CN=company,DC=Domain,DC=Com" /User:Domain\User1 
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
Para aprobar todos los equipos pendientes, escriba:
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[con el comando get-AutoaddDevices](using-the-get-autoadddevices-command.md) 
 [Con el comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
