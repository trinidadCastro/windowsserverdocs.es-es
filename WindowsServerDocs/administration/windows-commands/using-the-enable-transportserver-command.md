---
title: Enable-TransportServer
description: Tema de referencia de enable-TransportServer, que habilita todos los servicios para el servidor de transporte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50cc381b1c178628be7d135868027b4f37787cdd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720919"
---
# <a name="enable-transportserver"></a>Enable-TransportServer

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Habilita todos los servicios para el servidor de transporte.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre, se usará el servidor local.|
## <a name="examples"></a>Ejemplos
Para habilitar los servicios en el servidor, ejecute una de las siguientes opciones:
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave](command-line-syntax-key.md)
de sintaxis de línea de comandos[mediante el comando](using-the-disable-transportserver-command.md)
Disable-TransportServer[mediante el subcomando Get-TransportServer Command](using-the-get-transportserver-command.md)
[: set-TransportServer](subcommand-set-transportserver.md)
[Subcommand: Start-TransportServer](subcommand-start-transportserver.md)
[Subcommand: Stop-TransportServer](subcommand-stop-transportserver.md)
