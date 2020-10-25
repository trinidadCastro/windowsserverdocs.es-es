---
title: WDSUtil Delete-AutoAddDevices
description: Artículo de referencia del comando WDSUtil Delete-AutoAddDevices, que elimina los equipos que están pendientes, rechazados o aprobados en la base de datos de adición automática.
ms.topic: reference
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 610b57c7db49c5d8cf354502634cf82212d52c19
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524820"
---
# <a name="wdsutil-delete-autoadddevices"></a>WDSUtil Delete-AutoAddDevices

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Elimina los equipos que están pendientes, rechazados o aprobados en la base de datos de adición automática. Esta base de datos almacena información acerca de estos equipos en el servidor.

## <a name="syntax"></a>Sintaxis

```
wdsutil /delete-AutoaddDevices [/Server:<Servername>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| [/Server:`<Servername>`] | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local. |
| TipoDeDispositivo`{PendingDevices|RejectedDevices|ApprovedDevices}` | Especifica el tipo de equipo que se va a eliminar de la base de datos. Este tipo puede ser **PendingDevices**, que devuelve todos los equipos de la base de datos que tienen el estado pending, **RejectedDevices**, que devuelve todos los equipos de la base de datos que tienen el estado rechazado, o **ApprovedDevices**, que devuelve todos los equipos que tienen el estado aprobado. |

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

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
