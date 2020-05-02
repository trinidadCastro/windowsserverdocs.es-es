---
title: Rechazar-AutoaddDevices
description: Tema de referencia de Reject-AutoaddDevices, que rechaza los equipos que están pendientes de aprobación administrativa.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e377d4e2d4aecea2e0ba3af023af39ab7695c0a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725927"
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
- [Clave](command-line-syntax-key.md)
de sintaxis de línea de comandos[mediante el comando APPROVE-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[con el comando DELETE-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[mediante el comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
