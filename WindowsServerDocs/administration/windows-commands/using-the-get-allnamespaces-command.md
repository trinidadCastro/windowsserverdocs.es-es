---
title: Get-AllNamespaces
description: Temas de comandos de Windows para Get-AllNamespaces, que muestra información acerca de todos los espacios de nombres de un servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbdba81f68f609c6ed7ba740b68b1ac1123913ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831258"
---
# <a name="get-allnamespaces"></a>Get-AllNamespaces

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de todos los espacios de nombres de un servidor.

## <a name="syntax"></a>Sintaxis
Windows Server 2008:
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/Show:Clients] [/ExcludedeletePending]
```
Windows Server 2008 R2:
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/details:Clients] [/ExcludedeletePending]
```
### <a name="parameters"></a>Parámetros

|         Parámetro         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/Server:<Server name>]  | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local. |                        |
| [/ContentProvider:<name>] |                                                        Muestra solo los espacios de nombres del proveedor de contenido especificado.                                                         |                        |
|      [/Show: clients]      |                            Solo se admite para Windows Server 2008. Muestra información acerca de los equipos cliente que están conectados al espacio de nombres.                             |                        |
|    [/Details: clientes]     |                           Solo se admite para Windows Server 2008 R2. Muestra información acerca de los equipos cliente que están conectados al espacio de nombres.                           |                        |
|  [/ExcludedeletePending]  |                                                              Excluye cualquier transmisión desactivada de la lista.                                                              |                        |

## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver todos los espacios de nombres, escriba:
```
wdsutil /Get-AllNamespaces
```
Para ver todos los espacios de nombres excepto los que están desactivados, escriba:
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /details:Clients /ExcludedeletePending
  ```
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [usar el comando new-namespace](using-the-new-namespace-command.md)
  [mediante el comando Remove-namespace](using-the-remove-namespace-command.md)
  [subcomando: Start-namespace](subcommand-start-namespace.md)
