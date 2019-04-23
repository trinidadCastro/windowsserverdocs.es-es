---
title: Con el comando remove-Namespace
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 115c0a90a60e18ee4b89758200773d1dfec2163f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842046"
---
# <a name="using-the-remove-namespace-command"></a>Con el comando remove-Namespace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita un espacio de nombres personalizado.
## <a name="syntax"></a>Sintaxis
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/ Namespace:<Namespace name>|Especifica el nombre del espacio de nombres. Esto no es el nombre descriptivo y debe ser único.<br /><br />-   **Servicio de rol de servidor de implementación**: La sintaxis de nombre de espacio de nombres es /Namespace:WDS:<ImageGroup>/<ImageName>/<Index>. Por ejemplo: **WDS:ImageGroup1/install.wim/1**<br />-   **Servicio de rol servidor de transporte**: Este valor debe coincidir con el nombre asignado al espacio de nombres cuando se creó en el servidor.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usa el servidor local.|
|[/force]|Quita el espacio de nombres inmediatamente y finaliza a todos los clientes. Tenga en cuenta que, a menos que especifique **/force**, los clientes existentes pueden completar la transferencia, pero no están posible unirse a nuevos clientes.|
## <a name="BKMK_examples"></a>Ejemplos
Para detener un espacio de nombres (clientes actuales pueden completar la transferencia pero no están posible unirse a nuevos clientes), escriba:
```
wdsutil /remove-Namespace /Namespace:"Custom Auto 1"
```
Para forzar la terminación de todos los clientes, escriba:
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /force
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[con el comando nuevo Namespace](using-the-new-namespace-command.md) 
 [ Subcomando: start-Namespace](subcommand-start-namespace.md)
