---
title: Usar el comando DELETE-AutoaddDevices
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5e914506f731685b17117b359359f60ea91992dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363538"
---
# <a name="using-the-delete-autoadddevices-command"></a>Usar el comando DELETE-AutoaddDevices

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

elimina los equipos que están pendientes, rechazados o aprobados en la base de datos de adición automática. Esta base de datos almacena información acerca de estos equipos en el servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Especifica el tipo de equipo que se va a eliminar de la base de datos. Puede ser cualquiera de los tres tipos siguientes:<br /><br />-   **PendingDevices** devuelve todos los equipos de la base de datos que tienen el estado pendiente.<br />-   **RejectedDevices** devuelve todos los equipos de la base de datos que tienen el estado rechazado.<br />-   **ApprovedDevices** devuelve todos los equipos que tienen el estado aprobado.|
## <a name="BKMK_examples"></a>Example
Para eliminar todos los equipos rechazados, escriba:
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Para eliminar todos los equipos aprobados, escriba:
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando APPROVE-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[mediante el comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
[mediante el comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
