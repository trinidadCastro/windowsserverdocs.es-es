---
title: Mediante el comando get-AutoaddDevices
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 337c8e76923fe243982ba9c10d18f2e5a5e7d9ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885746"
---
# <a name="using-the-get-autoadddevices-command"></a>Mediante el comando get-AutoaddDevices

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra todos los equipos que están en la base de datos de adición automática en un servidor de servicios de implementación de Windows.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|/ Devicetype: {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|Especifica el tipo de equipo para devolver.<br /><br />-   **PendingDevices** devuelve todos los equipos en la base de datos que tienen un estado pendiente.<br />-   **RejectedDevices** devuelve todos los equipos en la base de datos que ha estado rechazado.<br />-   **ApprovedDevices** devuelve todos los equipos en la base de datos que ha estado aprobado.|
## <a name="BKMK_examples"></a>Ejemplos
Para ver todos los equipos aprobados, escriba:
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
Para ver todos los equipos rechazados, escriba:
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[con el comando Approve-AutoaddDevices](using-the-approve-autoadddevices-command.md) 
 [ Mediante el comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
