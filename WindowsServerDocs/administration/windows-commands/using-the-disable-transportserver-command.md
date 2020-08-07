---
title: Disable-TransportServer
description: Artículo de referencia de Disable-TransportServer, que deshabilita todos los servicios de un servidor de transporte.
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 48909ffb5a452d4bac1058acc5ddc48ed69b371f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892118"
---
# <a name="disable-transportserver"></a>Disable-TransportServer

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Deshabilita todos los servicios de un servidor de transporte.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte que se va a deshabilitar. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor de transporte, se usará el servidor local.|
## <a name="examples"></a>Ejemplos
Para deshabilitar el servidor, escriba:
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-enable-transportserver-command.md) 
 enable-TransportServer [Usar el comando](using-the-get-transportserver-command.md) 
 Get-TransportServer [Subcomando: set-TransportServer](subcommand-set-transportserver.md) 
 [Subcomando: Start-TransportServer](subcommand-start-transportserver.md) 
 [Subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
