---
title: Get-TransportServer
description: Artículo de referencia de Get-TransportServer, que muestra información sobre un servidor de transporte especificado.
ms.topic: reference
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83b79c689e16af16e0a6ca253dd9dce6afb1fa35
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029493"
---
# <a name="get-transportserver"></a>Get-TransportServer

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de un servidor de transporte especificado.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/Show: {config}|Devuelve información de configuración sobre el servidor de transporte especificado.|
## <a name="examples"></a>Ejemplos
Para ver información sobre el servidor, escriba:
```
wdsutil /Get-TransportServer /Show:Config
```
Para ver la información de configuración, escriba:
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-disable-transportserver-command.md) 
 Disable-TransportServer [Usar el comando](using-the-enable-transportserver-command.md) 
 enable-TransportServer [Subcomando: set-TransportServer](subcommand-set-transportserver.md) 
 [Subcomando: Start-TransportServer](subcommand-start-transportserver.md) 
 [Subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
