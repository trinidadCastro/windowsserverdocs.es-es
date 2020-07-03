---
title: Remove-namespace
description: Artículo de referencia para Remove-namespace, que quita un espacio de nombres personalizado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4b0920e5f42f40b1f0cdce0ffcc3a09bf19b0d2
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935407"
---
# <a name="using-the-remove-namespace-command"></a>Usar el comando Remove-namespace

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
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-get-allnamespaces-command.md) 
 Get-AllNamespaces [Usar el comando](using-the-new-namespace-command.md) 
 New-namespace [Subcomando: Start-namespace](subcommand-start-namespace.md)
