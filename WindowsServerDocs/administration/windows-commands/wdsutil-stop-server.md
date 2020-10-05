---
title: WDSUtil STOP-Server
description: Artículo de referencia para el subcomando STOP-Server, que detiene todos los servicios en un servidor de servicios de implementación de Windows.
ms.topic: reference
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0c1288900cdc5eaa46c27b6eb05fd64b9a1b16a4
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730637"
---
# <a name="wdsutil-stop-server"></a>WDSUtil STOP-Server

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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Disable-Server (comando)](wdsutil-disable-server.md)
- [WDSUtil Enable-Server (comando)](wdsutil-enable-server.md)
- [comando Get-Server de WDSUtil](wdsutil-get-server.md)
- [WDSUtil Initialize-Server (comando)](wdsutil-initialize-server.md)
- [comando set-Server de WDSUtil](wdsutil-set-server.md)
- [comando WDSUtil Start-Server](wdsutil-start-server.md)
- [WDSUtil uninitial-Server (comando)](wdsutil-uninitialize-server.md)

