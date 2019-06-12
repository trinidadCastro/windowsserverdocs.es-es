---
title: Mediante el comando get-Namespace
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c30f9ef375bdf368f81f5a69961746851a2aac8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440431"
---
# <a name="using-the-get-namespace-command"></a>Mediante el comando get-Namespace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de un espacio de nombres personalizado.
## <a name="syntax"></a>Sintaxis
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/Show:Clients]
```
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/details:Clients]
```
## <a name="parameters"></a>Parámetros

|               Parámetro               |                                                                                                                                                                                         Descripción                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      / Namespace:<Namespace name>      | Especifica el nombre del espacio de nombres. Tenga en cuenta que esto no es el nombre descriptivo y debe ser único.<br /><br />-Implementación servidor: La sintaxis de nombre de espacio de nombres es /Namspace:WDS:<ImageGroup>/<ImageName>/<Index>. Por ejemplo: **WDS:ImageGroup1/install.wim/1**<br />-Transporte Server: Este valor debe coincidir con el nombre asignado al espacio de nombres cuando se creó en el servidor. |
|        [/Server:<Server name>]        |                                                                                                             Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usa el servidor local.                                                                                                              |
| [/ Show: Clients] o [/ clientes: detalles] |                                                                                                                                                  Muestra información acerca de los equipos cliente que están conectados al espacio de nombres especificado.                                                                                                                                                  |

## <a name="BKMK_examples"></a>Ejemplos
Para ver información acerca de un espacio de nombres, escriba:
```
wdsutil /Get-Namespace /Namespace:"Custom Auto 1"
```
Para ver información acerca de un espacio de nombres y los clientes que están conectados, escriba uno de los siguientes:
- Windows Server 2008: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /Show:Clients`
- Windows Server 2008 R2: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /details:Clients`
  #### <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [con el comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
  [con el comando nuevo Namespace](using-the-new-namespace-command.md)
  [Using el comando remove-Namespace](using-the-remove-namespace-command.md)
  [subcomando: start-Namespace](subcommand-start-namespace.md)
