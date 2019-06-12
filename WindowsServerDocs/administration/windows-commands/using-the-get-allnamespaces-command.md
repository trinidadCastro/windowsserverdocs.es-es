---
title: Mediante el comando get-AllNamespaces
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8b77bb80238ee63cc0d71d88592d75850720e33b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440525"
---
# <a name="using-the-get-allnamespaces-command"></a>Mediante el comando get-AllNamespaces

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información sobre todos los espacios de nombres en un servidor.
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
|  [/Server:<Server name>]  | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local. |                        |
| [/ContentProvider:<name>] |                                                        Muestra los espacios de nombres para que solo el proveedor de contenido especificado.                                                         |                        |
|      [/ Show: Clients]      |                            Solo se admite para Windows Server 2008. Muestra información acerca de los equipos cliente que están conectados al espacio de nombres.                             |                        |
|    [/ clientes: detalles]     |                           Solo se admite para Windows Server 2008 R2. Muestra información acerca de los equipos cliente que están conectados al espacio de nombres.                           |                        |
|  [/ExcludedeletePending]  |                                                              Excluye cualquier transmisiones desactivadas en la lista.                                                              |                        |

## <a name="BKMK_examples"></a>Ejemplos
Para ver todos los espacios de nombres, escriba:
```
wdsutil /Get-AllNamespaces
```
Para ver todos los espacios de nombres, excepto las que están desactivadas, escriba:
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
  [con el comando nuevo Namespace](using-the-new-namespace-command.md)
  [con el comando remove-Namespace](using-the-remove-namespace-command.md) 
   [ Subcomando: start-Namespace](subcommand-start-namespace.md)
