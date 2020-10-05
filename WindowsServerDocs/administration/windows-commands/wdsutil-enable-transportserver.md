---
title: WDSUtil enable-TransportServer
description: Artículo de referencia para WDSUtil enable-TransportServer, que habilita todos los servicios para el servidor de transporte.
ms.topic: reference
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4d4a294c0bda291e8340a4d8ffe54a11d487e0dd
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730942"
---
# <a name="wdsutil-enable-transportserver"></a>WDSUtil enable-TransportServer

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Habilita todos los servicios para el servidor de transporte.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre, se usará el servidor local.|
## <a name="examples"></a>Ejemplos
Para habilitar los servicios en el servidor, ejecute una de las siguientes opciones:
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Disable-TransportServer (comando)](wdsutil-disable-transportserver.md)
- [WDSUtil Get-TransportServer (comando)](wdsutil-get-transportserver.md)
- [WDSUtil Set-TransportServer (comando)](wdsutil-set-transportserver.md)
- [WDSUtil Start-TransportServer (comando)](wdsutil-start-transportserver.md)
- [WDSUtil STOP-TransportServer (comando)](wdsutil-stop-transportserver.md)
