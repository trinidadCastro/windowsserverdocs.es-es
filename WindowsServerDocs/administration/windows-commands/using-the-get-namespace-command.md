---
title: Get-namespace
description: Tema de comandos de Windows para Get-namespace, que muestra información sobre un espacio de nombres personalizado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 58e84ec5c82ea3c2663b38bd2e53f65f2cf47519
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830888"
---
# <a name="get-namespace"></a>Get-namespace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información sobre un espacio de nombres personalizado.

## <a name="syntax"></a>Sintaxis
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/Show:Clients]
```
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/details:Clients]
```
### <a name="parameters"></a>Parámetros

|               Parámetro               |                                                                                                                                                                                         Descripción                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /Namespace:<Namespace name>      | Especifica el nombre del espacio de nombres. Tenga en cuenta que este no es el nombre descriptivo y debe ser único.<p>-Servidor de implementación: la sintaxis del nombre de espacio de nombres es/Namspace: WDS:<ImageGroup>/<ImageName>/<Index>. Por ejemplo: **WDS: ImageGroup1/install. Wim/1**<br />-Servidor de transporte: este valor debe coincidir con el nombre asignado al espacio de nombres cuando se creó en el servidor. |
|        [/Server:<Server name>]        |                                                                                                             Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.                                                                                                              |
| [/Show: clients] o [/Details: clients] |                                                                                                                                                  Muestra información acerca de los equipos cliente que están conectados al espacio de nombres especificado.                                                                                                                                                  |

## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver información acerca de un espacio de nombres, escriba:
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
Para ver información acerca de un espacio de nombres y los clientes que están conectados, escriba uno de los siguientes:
- Windows Server 2008: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [usar el comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
  [usar el comando New-namespace](using-the-new-namespace-command.md)
  [usar el comando Remove-namespace](using-the-remove-namespace-command.md)
  [Subcommand: Start-namespace](subcommand-start-namespace.md)
