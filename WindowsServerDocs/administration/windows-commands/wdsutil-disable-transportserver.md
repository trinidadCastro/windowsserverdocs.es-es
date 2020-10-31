---
title: WDSUtil Disable-TransportServer
description: Artículo de referencia para el comando WDSUtil Disable-TransportServer, que deshabilita todos los servicios de un servidor de transporte.
ms.topic: reference
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4bbb177897e3a779de275957949b0dcf62e53478
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070357"
---
# <a name="wdsutil-disable-transportserver"></a>WDSUtil Disable-TransportServer

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Deshabilita todos los servicios de un servidor de transporte.

## <a name="syntax"></a>Sintaxis

```
wdsutil [Options] /Disable-TransportServer [/Server:<Servername>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Description|
|-------|--------|
|[/Server:`<Servername>`]|Especifica el nombre del servidor de transporte que se va a deshabilitar. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor de transporte, se usará el servidor local.|

## <a name="examples"></a>Ejemplos

Para deshabilitar el servidor, escriba:

```
wdsutil /Disable-TransportServer
```

```
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil enable-TransportServer (comando)](wdsutil-enable-transportserver.md)

- [WDSUtil Get-TransportServer (comando)](wdsutil-get-transportserver.md)

- [WDSUtil Set-TransportServer (comando)](wdsutil-set-transportserver.md)

- [WDSUtil Start-TransportServer (comando)](wdsutil-start-transportserver.md)

- [WDSUtil STOP-TransportServer (comando)](wdsutil-stop-transportserver.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
