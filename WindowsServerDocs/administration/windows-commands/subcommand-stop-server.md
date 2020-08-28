---
title: Subcomando STOP-Server
description: Artículo de referencia para el subcomando STOP-Server, que detiene todos los servicios en un servidor de servicios de implementación de Windows.
ms.topic: reference
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fd4a2e249b5bbf52cce9d35fcb07821b793fb23
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024669"
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
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-disable-server-command.md) 
 Disable-Server [Usar el comando](using-the-enable-server-command.md) 
 enable-Server [Usar el comando](using-the-get-server-command.md) 
 Get-Server [Usar el comando](using-the-initialize-server-command.md) 
 Initialize-Server [Subcomando: set-Server](subcommand-set-server.md) 
 [Subcomando: Start-Server](subcommand-start-server.md) 
 [La opción UnInitialize-Server](the-uninitialize-server-option.md)
