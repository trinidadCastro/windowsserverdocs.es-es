---
title: Subcomando set-DriverGroup
description: Tema de comandos de Windows para el subcomando set-DriverGroup, que establece las propiedades de un grupo de controladores existente en un servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c49381dc65f3b2ffc9a04e4fb2699818515a931f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834038"
---
# <a name="subcommand-set-drivergroup"></a>Subcomando: set-DriverGroup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|[/Name:<New Group Name>]|Especifica el nuevo nombre del grupo de controladores.|
|[/Enabled: {Yes &#124; no}|Habilita o deshabilita el grupo de controladores.|
|[/Applicability: {matched &#124; All}]|Especifica los paquetes que se van a instalar si se cumplen los criterios de filtro. **Matched** significa instalar solo los paquetes de controladores que coinciden con el hardware del cliente. **Todo** significa instalar todos los paquetes en los clientes, independientemente de su hardware.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para establecer las propiedades de un grupo de controladores, escriba uno de los siguientes:
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[subcomando: set-DriverGroupFilter](subcommand-set-drivergroupfilter.md)
