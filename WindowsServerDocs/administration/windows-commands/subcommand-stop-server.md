---
title: Subcomando STOP-Server
description: Tema de comandos de Windows para el subcomando STOP-Server, que detiene todos los servicios en un servidor de servicios de implementación de Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2671e7a2c2e5bb542cecd9374ad6364d68ff5bd4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833718"
---
# <a name="subcommand-stop-server"></a>Subcomando: Stop-Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Detiene todos los servicios en un servidor de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para detener los servicios, escriba uno de los siguientes:
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [La clave de sintaxis de línea de comandos](command-line-syntax-key.md)
usar el comando [Disable-Server](using-the-disable-server-command.md)
[mediante el comando Enable-Server](using-the-enable-server-command.md)
[mediante el comando Get-Server](using-the-get-server-command.md)
[mediante el comando Initialize-Server](using-the-initialize-server-command.md)
[Subcommand: set-Server](subcommand-set-server.md)
[Subcommand: Start-Server](subcommand-start-server.md)
[la opción UnInitialize-](the-uninitialize-server-option.md) Server
