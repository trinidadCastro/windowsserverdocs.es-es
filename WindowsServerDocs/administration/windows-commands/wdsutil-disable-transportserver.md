---
title: WDSUtil Disable-TransportServer
description: Artículo de referencia para WDSUtil Disable-TransportServer, que deshabilita todos los servicios de un servidor de transporte.
ms.topic: reference
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9d404c8f16e876a15e4b683dd3cceef60804fbdb
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730981"
---
# <a name="wdsutil-disable-transportserver"></a>WDSUtil Disable-TransportServer

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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil enable-TransportServer (comando)](wdsutil-enable-transportserver.md)
- [WDSUtil Get-TransportServer (comando)](wdsutil-get-transportserver.md)
- [WDSUtil Set-TransportServer (comando)](wdsutil-set-transportserver.md)
- [WDSUtil Start-TransportServer (comando)](wdsutil-start-transportserver.md)
- [WDSUtil STOP-TransportServer (comando)](wdsutil-stop-transportserver.md)
