---
title: WDSUtil-Remove-namespace
description: Artículo de referencia para WDSUtil Remove-namespace, que quita un espacio de nombres personalizado.
ms.topic: reference
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bc5d44b727bf93bf6630f7289833640397e69dae
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731279"
---
# <a name="wdsutil-remove-namespace"></a>WDSUtil-Remove-namespace

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Quita un espacio de nombres personalizado.

## <a name="syntax"></a>Sintaxis
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|System.IO<Namespace name>|Especifica el nombre del espacio de nombres. Este no es el nombre descriptivo y debe ser único.<p>-   **Servicio de rol servidor de implementación**: la sintaxis del nombre de espacio de nombres es/namespace: WDS: <ImageGroup> / <ImageName> / <Index> . Por ejemplo: **WDS: ImageGroup1/install. Wim/1**<br />-   **Servicio de función de servidor de transporte**: este valor debe coincidir con el nombre asignado al espacio de nombres cuando se creó en el servidor.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
|/Force.|quita el espacio de nombres inmediatamente y finaliza todos los clientes. Tenga en cuenta que, a menos que especifique **/Force**, los clientes existentes pueden completar la transferencia, pero los clientes nuevos no pueden unirse.|
## <a name="examples"></a>Ejemplos
Para detener un espacio de nombres (los clientes actuales pueden completar la transferencia, pero los clientes nuevos no pueden unirse), escriba:
```
wdsutil /remove-Namespace /Namespace:Custom Auto 1
```
Para forzar la finalización de todos los clientes, escriba:
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /force
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Get-allnamespaces (comando)](wdsutil-get-allnamespaces.md)
- [comando WDSUtil New-namespace](wdsutil-new-namespace.md)
- [comando WDSUtil Start-namespace](wdsutil-start-namespace.md)
