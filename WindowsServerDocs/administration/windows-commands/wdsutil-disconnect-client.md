---
title: WDSUtil Disconnect-Client
description: Artículo de referencia del comando WDSUtil Disconnect-Client, que desconecta a un cliente de un espacio de nombres o transmisión por multidifusión.
ms.topic: reference
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dd837fd9a677f78b5890cd1f2092529d44a2bc68
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070337"
---
# <a name="wdsutil-disconnect-client"></a>WDSUtil Disconnect-Client

Desconecta un cliente de un espacio de nombres o transmisión de multidifusión. A menos que especifique **/Force** , el cliente revertirá a otro método de transferencia (si el cliente lo admite).

## <a name="syntax"></a>Sintaxis

```
wdsutil /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Description |
|--|--|
| ClientID`<ClientID>` | Especifica el identificador del cliente que se va a desconectar. Para ver el identificador de un cliente, ejecute el `wdsutil /get-multicasttransmission /show:clients` comando. |
| [/Server:`<Servername>`] | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
| /Force. | Detiene la instalación por completo y no utiliza un método de reserva. Dado que Wdsmcast.exe no admite ningún mecanismo de reserva, el comportamiento predeterminado es el siguiente:<ul><li>**Si utiliza el cliente de servicios de implementación de Windows:** El cliente continúa la instalación mediante unidifusión.</li><li>**Si no usa el cliente de servicios de implementación de Windows:** Se produce un error en la instalación.</li></ul>**Importante:** Se recomienda encarecidamente usar este parámetro con precaución, ya que si se produce un error en la instalación, el equipo puede quedar en un estado inutilizable. |

## <a name="examples"></a>Ejemplos

Para desconectar un cliente, escriba:

```
wdsutil /Disconnect-Client /ClientId:1
```

Para desconectar un cliente y forzar que se produzca un error en la instalación, escriba:

```
wdsutil /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil Get-multicasttransmission (comando)](wdsutil-get-multicasttransmission.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
