---
title: Rechazar-AutoaddDevices
description: Tema de comandos de Windows para Reject-AutoaddDevices, que rechaza los equipos que están pendientes de aprobación administrativa.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39bdddbbc169a0a0810fcc67e95224360858b728
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830658"
---
# <a name="reject-autoadddevices"></a>Rechazar-AutoaddDevices

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rechaza los equipos que están pendientes de aprobación administrativa. Cuando se habilita la Directiva de adición automática, se requiere la aprobación administrativa antes de que los equipos desconocidos (los que no están preconfigurados) puedan instalar una imagen. Puede habilitar esta Directiva mediante la pestaña **respuesta PXE** de la página Propiedades del servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/RequestId: < ID. &#124; de solicitud todos >|Especifica el identificador de solicitud asignado al equipo pendiente. Para rechazar todos los equipos pendientes, especifique **todos**.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para rechazar un solo equipo, escriba:
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
Para rechazar todos los equipos, escriba:
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando APPROVE-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[con el comando DELETE-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[mediante el comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
