---
title: Subcomando STOP-TransportServer
description: Artículo de referencia para STOP-TransportServer
ms.topic: reference
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2981743f422e7d5e8442f26935dbb4afdc97b923
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024639"
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
