---
title: Usar el comando Reject-AutoaddDevices
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e8fda3037ef921e2b2a7a0acb616b8a67545ff9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363002"
---
# <a name="using-the-reject-autoadddevices-command"></a>Usar el comando Reject-AutoaddDevices

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rechaza los equipos que están pendientes de aprobación administrativa. Cuando se habilita la Directiva de adición automática, se requiere la aprobación administrativa antes de que los equipos desconocidos (los que no están preconfigurados) puedan instalar una imagen. Puede habilitar esta Directiva mediante la pestaña **respuesta PXE** de la página Propiedades del servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/RequestId: < ID. &#124; de solicitud todos >|Especifica el identificador de solicitud asignado al equipo pendiente. Para rechazar todos los equipos pendientes, especifique **todos**.|
## <a name="BKMK_examples"></a>Example
Para rechazar un solo equipo, escriba:
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
Para rechazar todos los equipos, escriba:
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando APPROVE-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[mediante el comando DELETE-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[mediante el comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
