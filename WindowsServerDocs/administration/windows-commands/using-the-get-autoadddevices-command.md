---
title: Get-AutoaddDevices
description: Temas de comandos de Windows para Get-AutoaddDevices, que muestra todos los equipos que se encuentran en la base de datos de adición automática en un servidor de servicios de implementación de Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b373fca81769ff1296d0e9a0788fe536c4fc3132
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831188"
---
# <a name="get-autoadddevices"></a>Get-AutoaddDevices

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra todos los equipos que se encuentran en la base de datos de adición automática en un servidor de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|Especifica el tipo de equipo que se va a devolver.<p>-   **PendingDevices** devuelve todos los equipos de la base de datos que tienen el estado pendiente.<br />-   **RejectedDevices** devuelve todos los equipos de la base de datos que tienen el estado rechazado.<br />-   **ApprovedDevices** devuelve todos los equipos de la base de datos que tienen el estado aprobado.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver todos los equipos aprobados, escriba:
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
Para ver todos los equipos rechazados, escriba:
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[mediante el comando APPROVE-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[con el comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
