---
title: Subcomando STOP-TransportServer
description: Artículo de referencia para STOP-TransportServer
ms.topic: reference
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b7fb17b885708fb039f85fe88a7b4d04f2f0a63f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639458"
---
# <a name="subcommand-stop-transportserver"></a>Subcomando: Stop-TransportServer

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Detiene todos los servicios en un servidor de transporte.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún servidor de transporte, se usará el servidor local.|
## <a name="examples"></a><a name="BKMK_examples"></a>Ejemplos
Para detener los servicios, escriba uno de los siguientes:
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-disable-transportserver-command.md) 
 Disable-TransportServer [Usar el comando](using-the-enable-transportserver-command.md) 
 enable-TransportServer [Usar el comando](using-the-get-transportserver-command.md) 
 Get-TransportServer [Subcomando: set-TransportServer](subcommand-set-transportserver.md) 
 [Subcomando: Start-TransportServer](subcommand-start-transportserver.md)
