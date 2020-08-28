---
title: Subcomando set-DriverGroup
description: Artículo de referencia para el subcomando set-DriverGroup, que establece las propiedades de un grupo de controladores existente en un servidor.
ms.topic: reference
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15289ee5f4e8b705f6deb1841422c5b49f73b784
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036883"
---
# <a name="subcommand-set-drivergroup"></a>Subcomando: set-DriverGroup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Establece las propiedades de un grupo de controladores existente en un servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/DriverGroup:<Group Name>|Especifica el nombre del grupo de controladores.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local.|
|[/Name: <New Group Name> ]|Especifica el nuevo nombre del grupo de controladores.|
|[/Enabled: {Yes &#124; no}|Habilita o deshabilita el grupo de controladores.|
|[/Applicability: {matched &#124; All}]|Especifica los paquetes que se van a instalar si se cumplen los criterios de filtro. **Matched** significa instalar solo los paquetes de controladores que coinciden con el hardware del cliente. **Todo** significa instalar todos los paquetes en los clientes, independientemente de su hardware.|
## <a name="examples"></a>Ejemplos
Para establecer las propiedades de un grupo de controladores, escriba uno de los siguientes:
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Subcomando: set-DriverGroupFilter](subcommand-set-drivergroupfilter.md)
