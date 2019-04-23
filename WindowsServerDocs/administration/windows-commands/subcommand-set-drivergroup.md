---
title: Subcomando conjunto DriverGroup
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e645e16a3d78dd91bad98fedbb04896025b0eaf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852706"
---
# <a name="subcommand-set-drivergroup"></a>Subcomando: set-DriverGroup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece las propiedades de un grupo de controladores existente en un servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/DriverGroup:<Group Name>|Especifica el nombre del grupo de controladores.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se usa el servidor local.|
|[/ Name:<New Group Name>]|Especifica el nuevo nombre para el grupo de controladores.|
|[/ Enabled: {Sí &#124; n}|Habilita o deshabilita al grupo de controladores.|
|[/ Aplicabilidad: {Matched &#124; todas}]|Especifica qué paquetes se instalan si se cumplen los criterios de filtro. **Coincidir** significa instala sólo los paquetes de controladores que coincidan con un hardware de cliente s. **Todos los** significa instala todos los paquetes a los clientes independientemente de su hardware.|
## <a name="BKMK_examples"></a>Ejemplos
Para establecer las propiedades de un grupo de controladores, escriba uno de los siguientes:
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[subcomando: conjunto DriverGroupFilter](subcommand-set-drivergroupfilter.md)
