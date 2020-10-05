---
title: WDSUtil STOP-TransportServer
description: Artículo de referencia para STOP-TransportServer
ms.topic: reference
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 800c99cba18ed2158749b1638627b31f8fdbaf5f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731252"
---
# <a name="wdsutil-stop-transportserver"></a>WDSUtil STOP-TransportServer

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
## <a name="examples"></a>Ejemplos
Para detener los servicios, escriba uno de los siguientes:
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Disable-TransportServer (comando)](wdsutil-disable-transportserver.md)
- [WDSUtil enable-TransportServer (comando)](wdsutil-enable-transportserver.md)
- [WDSUtil Get-TransportServer (comando)](wdsutil-get-transportserver.md)
- [WDSUtil Set-TransportServer (comando)](wdsutil-set-transportserver.md)
- [WDSUtil Start-TransportServer (comando)](wdsutil-start-transportserver.md)
