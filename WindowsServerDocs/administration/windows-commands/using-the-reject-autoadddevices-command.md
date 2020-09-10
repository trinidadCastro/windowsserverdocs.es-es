---
title: Rechazar-AutoaddDevices
description: Artículo de referencia de Reject-AutoaddDevices, que rechaza los equipos que están pendientes de aprobación administrativa.
ms.topic: reference
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 554f3da87ddd3f99284c79614fdde516d171d16d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627911"
---
# <a name="reject-autoadddevices"></a>Rechazar-AutoaddDevices

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Rechaza los equipos que están pendientes de aprobación administrativa. Cuando se habilita la Directiva de adición automática, se requiere la aprobación administrativa antes de que los equipos desconocidos (los que no están preconfigurados) puedan instalar una imagen. Puede habilitar esta Directiva mediante la pestaña **respuesta PXE** de la página Propiedades del servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/RequestId: ID. de solicitud de <&#124; todos los>|Especifica el identificador de solicitud asignado al equipo pendiente. Para rechazar todos los equipos pendientes, especifique **todos**.|
## <a name="examples"></a>Ejemplos
Para rechazar un solo equipo, escriba:
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
Para rechazar todos los equipos, escriba:
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Uso del comando](using-the-approve-autoadddevices-command.md) 
 APPROVE-AutoaddDevices [Usar el comando](using-the-delete-autoadddevices-command.md) 
 Delete-AutoaddDevices [Usar el comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
