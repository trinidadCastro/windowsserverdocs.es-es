---
title: Subcomando STOP-Server
description: Tema de referencia para el subcomando STOP-Server, que detiene todos los servicios en un servidor de servicios de implementación de Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68cc73ac016e2ffded774567034801e1c11944d1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721630"
---
# <a name="subcommand-stop-server"></a>Subcomando: Stop-Server

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Detiene todos los servicios en un servidor de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="examples"></a>Ejemplos
Para detener los servicios, escriba uno de los siguientes:
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave](command-line-syntax-key.md)
de sintaxis de línea de comandos[mediante el comando DISABLE-Server](using-the-disable-server-command.md)
mediante el comando[enable-Server](using-the-enable-server-command.md)
mediante el
[comando Get-Server](using-the-get-server-command.md)[mediante el subcomando Initialize-Server Command](using-the-initialize-server-command.md)
[: set-](subcommand-set-server.md)
Server[Subcommand: Start-](subcommand-start-server.md)
Server[la opción UnInitialize-Server](the-uninitialize-server-option.md)
