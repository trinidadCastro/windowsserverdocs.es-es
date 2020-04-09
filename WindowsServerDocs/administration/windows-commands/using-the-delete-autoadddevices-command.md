---
title: Delete-AutoaddDevices
description: Comando de comandos de Windows para Delete-AutoaddDevices, que elimina los equipos que están pendientes, rechazados o aprobados en la base de datos de adición automática.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29df0bd92859e9ee0b5b5bedfbd2e66173059cb5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831668"
---
# <a name="delete-autoadddevices"></a>Delete-AutoaddDevices

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina los equipos que están pendientes, rechazados o aprobados en la base de datos de adición automática. Esta base de datos almacena información acerca de estos equipos en el servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Especifica el tipo de equipo que se va a eliminar de la base de datos. Puede ser cualquiera de los tres tipos siguientes:<p>-   **PendingDevices** devuelve todos los equipos de la base de datos que tienen el estado pendiente.<br />-   **RejectedDevices** devuelve todos los equipos de la base de datos que tienen el estado rechazado.<br />-   **ApprovedDevices** devuelve todos los equipos que tienen el estado aprobado.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para eliminar todos los equipos rechazados, escriba:
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Para eliminar todos los equipos aprobados, escriba:
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando APPROVE-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[usar el comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
[con el comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
