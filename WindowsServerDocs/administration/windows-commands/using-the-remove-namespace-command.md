---
title: Remove-namespace
description: Tema de comandos de Windows para Remove-namespace, que quita un espacio de nombres personalizado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f8b830a5d99d13ed00a3a19f2cf246ad71d1c5f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830238"
---
# <a name="using-the-remove-namespace-command"></a>Usar el comando Remove-namespace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita un espacio de nombres personalizado.

## <a name="syntax"></a>Sintaxis
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/Namespace:<Namespace name>|Especifica el nombre del espacio de nombres. Este no es el nombre descriptivo y debe ser único.<p>-   **servicio de rol servidor de implementación**: la sintaxis del nombre de espacio de nombres es/namespace: WDS:<ImageGroup>/<ImageName>/<Index>. Por ejemplo: **WDS: ImageGroup1/install. Wim/1**<br />-   **servicio de función de servidor de transporte**: este valor debe coincidir con el nombre asignado al espacio de nombres cuando se creó en el servidor.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
|/Force.|quita el espacio de nombres inmediatamente y finaliza todos los clientes. Tenga en cuenta que, a menos que especifique **/Force**, los clientes existentes pueden completar la transferencia, pero los clientes nuevos no pueden unirse.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
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
[usar el comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[usar el comando New-namespace](using-the-new-namespace-command.md)
[subcomando: Start-namespace](subcommand-start-namespace.md)
