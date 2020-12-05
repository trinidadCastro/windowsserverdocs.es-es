---
title: WDSUtil Get-alldevices
description: Artículo de referencia del comando WDSUtil Get-alldevices, que muestra las propiedades de los servicios de implementación de Windows de todos los equipos preconfigurados.
ms.topic: reference
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a960a30fd61056e2e2eb183e8a873223daa78be2
ms.sourcegitcommit: 28b5ab74cb0b40539ccc1a83998d6391e87fe51f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96614878"
---
# <a name="wdsutil-get-alldevices"></a>WDSUtil Get-alldevices

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra las propiedades de los servicios de implementación de Windows de todos los equipos preconfigurados. Un equipo preconfigurado es un equipo físico que se ha vinculado a una cuenta de equipo en servicios de dominio de Active Directory.

## <a name="syntax"></a>Sintaxis

```
wdsutil [options] /get-alldevices [/forest:{Yes | No}] [/referralserver:<servername>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[/forest:{Yes | No}]` | Especifica si servicios de implementación de Windows debe devolver equipos en todo el bosque o en el dominio local. La configuración predeterminada es **no**, lo que significa que solo se devuelven los equipos del dominio local. |
| `[/referralserver:<servername>]` | Devuelve solo los equipos preconfigurados para el servidor especificado. |

## <a name="examples"></a>Ejemplos

Para ver todos los equipos, escriba:

```
wdsutil /get-alldevices
```

```
wdsutil /verbose /get-alldevices /forest:Yes /referralserver:MyWDSServer
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando set-device de WDSUtil](wdsutil-set-device.md)

- [comando de add-device de WDSUtil](wdsutil-add-device.md)

- [comando Get-Device de WDSUtil](wdsutil-get-device.md)
