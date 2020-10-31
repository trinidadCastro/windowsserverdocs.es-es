---
title: WDSUtil enable-TransportServer
description: Artículo de referencia para el comando WDSUtil enable-TransportServer, que habilita todos los servicios para el servidor de transporte.
ms.topic: reference
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b698a9fae2d730a78497fffe52dc91a68175f0f3
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070307"
---
# <a name="wdsutil-enable-transportserver"></a>WDSUtil enable-TransportServer

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Habilita todos los servicios para el servidor de transporte.

## <a name="syntax"></a>Sintaxis

```
wdsutil [options] /Enable-TransportServer [/Server:<Servername>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Description |
|--|--|
| [/Server:`<Servername>`] | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |

## <a name="examples"></a>Ejemplos

Para habilitar los servicios en el servidor, escriba:

```
wdsutil /Enable-TransportServer
```

```
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil Disable-TransportServer (comando)](wdsutil-disable-transportserver.md)

- [WDSUtil Get-TransportServer (comando)](wdsutil-get-transportserver.md)

- [WDSUtil Set-TransportServer (comando)](wdsutil-set-transportserver.md)

- [WDSUtil Start-TransportServer (comando)](wdsutil-start-transportserver.md)

- [WDSUtil STOP-TransportServer (comando)](wdsutil-stop-transportserver.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
