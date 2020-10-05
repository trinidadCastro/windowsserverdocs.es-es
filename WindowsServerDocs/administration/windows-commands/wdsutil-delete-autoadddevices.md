---
title: WDSUtil Delete-AutoAddDevices
description: Artículo de referencia de WDSUtil Delete-AutoAddDevices, que elimina los equipos que están pendientes, rechazados o aprobados en la base de datos de adición automática.
ms.topic: reference
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 98c5aa38fd5394d4060cf9eba874d1be5cef845b
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730997"
---
# <a name="wdsutil-delete-autoadddevices"></a>WDSUtil Delete-AutoAddDevices

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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
## <a name="examples"></a>Ejemplos
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
- [WDSUtil APPROVE-AutoAddDevices (comando)](wdsutil-approve-autoadddevices.md)
- [WDSUtil Get-AutoAddDevices (comando)](wdsutil-get-autoadddevices.md)
- [WDSUtil-AutoAddDevices (comando)](wdsutil-reject-autoadddevices.md)
