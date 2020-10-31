---
title: WDSUtil Disable-Server
description: Artículo de referencia del comando WDSUtil Disable-Server, que deshabilita todos los servicios de un servidor de servicios de implementación de Windows.
ms.topic: reference
ms.assetid: b69fcfe0-b744-4794-bc75-2c9218c0ba66
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d8af0a0c929e2bff341b2b5bff71d0838b09d418
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070397"
---
# <a name="wdsutil-disable-server"></a>WDSUtil Disable-Server

Deshabilita todos los servicios de un servidor de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis

```
wdsutil [Options] /Disable-Server [/Server:<Server name>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Description |
|--|--|
| [/Server:`<Servername>`] | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local. |

## <a name="examples"></a>Ejemplos

Para deshabilitar el servidor, escriba:

```
wdsutil /Disable-Server
```

```
wdsutil /Verbose /Disable-Server /Server:MyWDSServer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
