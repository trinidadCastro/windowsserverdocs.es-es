---
title: Mediante el comando Reject-AutoaddDevices
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: af46aec7c8f02b3600983b66bd1b0ac6f5dd1dcc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852566"
---
# <a name="using-the-reject-autoadddevices-command"></a>Mediante el comando Reject-AutoaddDevices

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rechaza los equipos que están pendientes de aprobación administrativa. Cuando se habilita la directiva de adición automática, se requiere aprobación administrativa antes de que los equipos desconocidos (aquéllos que no están preconfigurados) puedan instalar una imagen. Puede habilitar esta directiva con el **respuesta PXE** ficha de la página de propiedades de servidor s.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|/RequestId:<Request ID &#124; ALL>|Especifica el identificador de solicitud asignado al equipo pendiente. Para rechazar todos los equipos pendientes, especifique **todas**.|
## <a name="BKMK_examples"></a>Ejemplos
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
[con el comando Approve-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[con el comando delete-AutoaddDevices](using-the-delete-autoadddevices-command.md) 
 [ Mediante el comando get-AutoaddDevices](using-the-get-autoadddevices-command.md)
