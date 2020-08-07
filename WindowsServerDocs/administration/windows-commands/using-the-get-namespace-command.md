---
title: Get-namespace
description: Artículo de referencia de Get-namespace, que muestra información sobre un espacio de nombres personalizado.
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e998334b8297b06bf5eb23b9106acd3770504ffb
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896936"
---
# <a name="get-namespace"></a>Get-namespace

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información sobre un espacio de nombres personalizado.

## <a name="syntax"></a>Sintaxis
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/Show:Clients]
```
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/details:Clients]
```
### <a name="parameters"></a>Parámetros

|               Parámetro               |                                                                                                                                                                                         Descripción                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      System.IO<Namespace name>      | Especifica el nombre del espacio de nombres. Tenga en cuenta que este no es el nombre descriptivo y debe ser único.<p>-Servidor de implementación: la sintaxis del nombre de espacio de nombres es/Namspace: WDS: <ImageGroup> / <ImageName> / <Index> . Por ejemplo: **WDS: ImageGroup1/install. Wim/1**<br />-Servidor de transporte: este valor debe coincidir con el nombre asignado al espacio de nombres cuando se creó en el servidor. |
|        [/Server:<Server name>]        |                                                                                                             Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.                                                                                                              |
| [/Show: clients] o [/Details: clients] |                                                                                                                                                  Muestra información acerca de los equipos cliente que están conectados al espacio de nombres especificado.                                                                                                                                                  |

## <a name="examples"></a>Ejemplos
Para ver información acerca de un espacio de nombres, escriba:
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
Para ver información acerca de un espacio de nombres y los clientes que están conectados, escriba uno de los siguientes:
- Windows Server 2008:`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2:`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`
  ## <a name="additional-references"></a>Referencias adicionales
  - Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
   [Usar el comando](using-the-get-allnamespaces-command.md) 
   Get-AllNamespaces [Usar el comando](using-the-new-namespace-command.md) 
   New-namespace [Usar el comando](using-the-remove-namespace-command.md) 
   Remove-namespace [Subcomando: Start-namespace](subcommand-start-namespace.md)
