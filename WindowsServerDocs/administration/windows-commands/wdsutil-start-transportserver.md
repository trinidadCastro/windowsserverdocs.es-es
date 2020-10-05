---
title: WDSUtil Start-TransportServer
description: Artículo de referencia para el subcomando Start-TransportServer, que inicia todos los servicios de un servidor de transporte.
ms.topic: reference
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3aa948fb86b1b69448ac131c6894ff0c41f548e8
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731382"
---
# <a name="wdsutil-start-transportserver"></a>WDSUtil Start-TransportServer

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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Disable-TransportServer (comando)](wdsutil-disable-transportserver.md)
- [WDSUtil enable-TransportServer (comando)](wdsutil-enable-transportserver.md)
- [WDSUtil Get-TransportServer (comando)](wdsutil-get-transportserver.md)
- [WDSUtil Set-TransportServer (comando)](wdsutil-set-transportserver.md)
- [WDSUtil STOP-TransportServer (comando)](wdsutil-stop-transportserver.md)
