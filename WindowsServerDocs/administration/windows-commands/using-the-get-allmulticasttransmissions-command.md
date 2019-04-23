---
title: Mediante el comando get-AllMulticastTransmissions
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf4c3449a5c3194ec27efc2ee4adaccb54f9f7e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889156"
---
# <a name="using-the-get-allmulticasttransmissions-command"></a>Mediante el comando get-AllMulticastTransmissions

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información sobre todas las transmisiones por multidifusión en un servidor.
## <a name="syntax"></a>Sintaxis
for Windows Server 2008:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:Clients] [/ExcludedeletePending]
```
for Windows Server 2008 R2:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:{Boot | Install | All}] [/details:Clients]  [/ExcludedeletePending]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Explicación|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|[/Show]|**Windows Server 2008**<br /><br />/ Show: Clients - muestra información acerca de los equipos cliente que están conectados a las transmisiones de multidifusión.<br /><br />**Windows Server 2008 R2**<br /><br />Mostrar: {arranque &#124; instalar &#124; todas}: el tipo de imagen para devolver.                                **Arranque** devuelve solo las transmisiones de imagen de arranque.                                  **Instalar** devuelve instala sólo las transmisiones de imagen. **Todos los** devuelve ambos tipos de imagen.|
|||
|/Details:Clients|Solo se admite para Windows Server 2008 R2. Si está presente, se mostrará en los clientes que están conectados a la transmisión.|
|[/ExcludedeletePending]|Excluye cualquier transmisiones desactivadas en la lista.|
## <a name="BKMK_examples"></a>Ejemplos
Para ver información acerca de todas las transmisiones, escriba:
-   Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions`
-   Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Show:All` Para ver información acerca de todas las transmisiones, excepto las transmisiones desactivadas, escriba:
-   Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
-   Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
[con el comando nueva-MulticastTransmission](using-the-new-multicasttransmission-command.md) 
 [Con el comando remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
[subcomando: start-MulticastTransmission](subcommand-start-multicasttransmission.md)
