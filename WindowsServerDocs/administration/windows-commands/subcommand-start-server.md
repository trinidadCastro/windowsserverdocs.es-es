---
title: Subcomando Start-Server
description: Tema de comandos de Windows para el subcomando Start-Server, que inicia todos los servicios de un servidor de servicios de implementación de Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc3e0d11348dd6b531671e62139f2ce2c884c5ee
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833748"
---
# <a name="subcommand-start-server"></a>Subcomando: Start-Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia todos los servicios de un servidor de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor que se va a iniciar. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para iniciar el servidor, escriba uno de los siguientes:
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
usar el comando [Disable-Server](using-the-disable-server-command.md)
[mediante el comando Enable-Server](using-the-enable-server-command.md)
[mediante el comando Get-Server](using-the-get-server-command.md)
[mediante el comando Initialize-Server](using-the-initialize-server-command.md)
[Subcommand: set-Server](subcommand-set-server.md)
[Subcommand: Stop-Server](subcommand-stop-server.md)
[la opción UnInitialize](the-uninitialize-server-option.md) -Server
