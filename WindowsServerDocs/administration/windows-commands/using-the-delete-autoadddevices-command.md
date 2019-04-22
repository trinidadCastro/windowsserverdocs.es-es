---
title: Con el comando delete-AutoaddDevices
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b3375418c5ce0b02e187e292cac5b168f0de5dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813506"
---
# <a name="using-the-delete-autoadddevices-command"></a>Con el comando delete-AutoaddDevices

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina los equipos que están pendientes, rechazado o aprobado de la base de datos de adición automática. Esta base de datos almacena información acerca de estos equipos en el servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|/Devicetype:{PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Especifica el tipo de equipo para eliminar de la base de datos. Esto puede ser cualquiera de los tres tipos siguientes:<br /><br />-   **PendingDevices** devuelve todos los equipos en la base de datos que tienen un estado pendiente.<br />-   **RejectedDevices** devuelve todos los equipos en la base de datos que ha estado rechazado.<br />-   **ApprovedDevices** devuelve todos los equipos que tienen un estado de aprobado.|
## <a name="BKMK_examples"></a>Ejemplos
Para eliminar rechazadas en todos los equipos, escriba:
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Para eliminar los equipos aprobados todo, escriba:
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando Approve-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[con el comando get-AutoaddDevices](using-the-get-autoadddevices-command.md) 
 [Con el comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
