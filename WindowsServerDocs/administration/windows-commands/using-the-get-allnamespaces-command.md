---
title: Get-AllNamespaces
description: Artículo de referencia de Get-AllNamespaces, que muestra información acerca de todos los espacios de nombres de un servidor.
ms.topic: reference
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9cd6010e759c5b33abe011263abf3464e3d7a356
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035973"
---
# <a name="get-allnamespaces"></a>Get-AllNamespaces

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de todos los espacios de nombres de un servidor.

## <a name="syntax"></a>Sintaxis
Windows Server 2008:
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/Show:Clients] [/ExcludedeletePending]
```
Windows Server 2008 R2:
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/details:Clients] [/ExcludedeletePending]
```
### <a name="parameters"></a>Parámetros

|         Parámetro         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/Server:<Server name>]  | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local. |                        |
| [/ContentProvider: <name> ] |                                                        Muestra solo los espacios de nombres del proveedor de contenido especificado.                                                         |                        |
|      [/Show: clients]      |                            Solo se admite para Windows Server 2008. Muestra información acerca de los equipos cliente que están conectados al espacio de nombres.                             |                        |
|    [/Details: clientes]     |                           Solo se admite para Windows Server 2008 R2. Muestra información acerca de los equipos cliente que están conectados al espacio de nombres.                           |                        |
|  [/ExcludedeletePending]  |                                                              Excluye cualquier transmisión desactivada de la lista.                                                              |                        |

## <a name="examples"></a>Ejemplos
Para ver todos los espacios de nombres, escriba:
```
wdsutil /Get-AllNamespaces
```
Para ver todos los espacios de nombres excepto los que están desactivados, escriba:
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /details:Clients /ExcludedeletePending
  ```
  ## <a name="additional-references"></a>Referencias adicionales
  - Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
   [Usar el comando](using-the-new-namespace-command.md) 
   New-namespace [Usar el comando](using-the-remove-namespace-command.md) 
   Remove-namespace [Subcomando: Start-namespace](subcommand-start-namespace.md)
