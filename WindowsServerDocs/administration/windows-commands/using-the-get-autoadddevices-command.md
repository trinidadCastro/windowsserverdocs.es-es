---
title: Get-AutoaddDevices
description: Artículo de referencia de Get-AutoaddDevices, que muestra todos los equipos que se encuentran en la base de datos de adición automática en un servidor de servicios de implementación de Windows.
ms.topic: reference
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f186d36fbdc4ccfac26eae9092c8bb89973d8835
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626348"
---
# <a name="get-autoadddevices"></a>Get-AutoaddDevices

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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
## <a name="examples"></a>Ejemplos
Para ver todos los equipos aprobados, escriba:
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
Para ver todos los equipos rechazados, escriba:
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-delete-autoadddevices-command.md) 
 Delete-AutoaddDevices [Uso del comando](using-the-approve-autoadddevices-command.md) 
 APPROVE-AutoaddDevices [Usar el comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
