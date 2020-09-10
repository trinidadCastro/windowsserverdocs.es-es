---
title: Subcomando Start-TransportServer
description: Artículo de referencia para el subcomando Start-TransportServer, que inicia todos los servicios de un servidor de transporte.
ms.topic: reference
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 635013a5f45d1014c24074728c5ecb951d5dccb2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633877"
---
# <a name="subcommand-start-transportserver"></a>Subcomando: Start-TransportServer

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Inicia todos los servicios de un servidor de transporte.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="examples"></a>Ejemplos
Para iniciar el servidor, escriba uno de los siguientes:
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-disable-transportserver-command.md) 
 Disable-TransportServer [Usar el comando](using-the-enable-transportserver-command.md) 
 enable-TransportServer [Usar el comando](using-the-get-transportserver-command.md) 
 Get-TransportServer [Subcomando: set-TransportServer](subcommand-set-transportserver.md) 
 [Subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
