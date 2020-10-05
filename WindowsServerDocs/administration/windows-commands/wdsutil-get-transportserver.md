---
title: WDSUtil Get-TransportServer
description: Artículo de referencia para WDSUtil Get-TransportServer, que muestra información sobre un servidor de transporte especificado.
ms.topic: reference
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a0d807035fa60ca757b1b27cc63c4bfce6e92070
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730773"
---
# <a name="wdsutil-get-transportserver"></a>WDSUtil Get-TransportServer

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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Disable-TransportServer (comando)](wdsutil-disable-transportserver.md)
- [WDSUtil enable-TransportServer (comando)](wdsutil-enable-transportserver.md)
- [WDSUtil Set-TransportServer (comando)](wdsutil-set-transportserver.md)
- [WDSUtil Start-TransportServer (comando)](wdsutil-start-transportserver.md)
- [WDSUtil STOP-TransportServer (comando)](wdsutil-stop-transportserver.md)
