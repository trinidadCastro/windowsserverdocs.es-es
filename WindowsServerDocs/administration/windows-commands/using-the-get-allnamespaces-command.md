---
title: Usar el comando Get-AllNamespaces
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0cd90fc650271c863459dd809e47ca6309132de5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363292"
---
# <a name="using-the-get-allnamespaces-command"></a>Usar el comando Get-AllNamespaces

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="parameters"></a>Parámetros

|         Parámetro         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/Server:<Server name>]  | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local. |                        |
| [/ContentProvider: <name>] |                                                        Muestra solo los espacios de nombres del proveedor de contenido especificado.                                                         |                        |
|      [/Show: clients]      |                            Solo se admite para Windows Server 2008. Muestra información acerca de los equipos cliente que están conectados al espacio de nombres.                             |                        |
|    [/Details: clientes]     |                           Solo se admite para Windows Server 2008 R2. Muestra información acerca de los equipos cliente que están conectados al espacio de nombres.                           |                        |
|  [/ExcludedeletePending]  |                                                              Excluye cualquier transmisión desactivada de la lista.                                                              |                        |

## <a name="BKMK_examples"></a>Example
Para ver todos los espacios de nombres, escriba:
```
wdsutil /Get-AllNamespaces
```
Para ver todos los espacios de nombres excepto los que están desactivados, escriba:
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /details:Clients /ExcludedeletePending
  ```
  #### <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [con el comando new-namespace](using-the-new-namespace-command.md)
  [mediante el comando Remove-namespace](using-the-remove-namespace-command.md)
  [subcomando: Start-namespace](subcommand-start-namespace.md)
