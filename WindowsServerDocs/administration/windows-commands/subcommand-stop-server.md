---
title: Subcomando STOP-Server
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7584dcbca5bfc52d303f187f62be24cbad407416
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383744"
---
# <a name="subcommand-stop-server"></a>Subcomando: Stop-Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Detiene todos los servicios en un servidor de servicios de implementación de Windows.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="BKMK_examples"></a>Example
Para detener los servicios, escriba uno de los siguientes:
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando DISABLE-Server](using-the-disable-server-command.md)
[mediante el comando Enable-Server](using-the-enable-server-command.md)
[mediante el comando Get-Server](using-the-get-server-command.md)
[mediante el comando Initialize-Server](using-the-initialize-server-command.md)
[ Subcomando: set-Server](subcommand-set-server.md)1[subcomando: Start-Server](subcommand-start-server.md)3[The UnInitialize-Server Option](the-uninitialize-server-option.md)
