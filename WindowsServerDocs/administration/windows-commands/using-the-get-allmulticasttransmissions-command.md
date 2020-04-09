---
title: Get-AllMulticastTransmissions
description: Temas de comandos de Windows para Get-AllMulticastTransmissions, que muestra información sobre todas las transmisiones de multidifusión en un servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c81db5a5d5ebb9bcc5e2d00165d5c944c687fd97
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831338"
---
# <a name="get-allmulticasttransmissions"></a>Get-AllMulticastTransmissions

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de todas las transmisiones de multidifusión en un servidor.

## <a name="syntax"></a>Sintaxis
para Windows Server 2008:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:Clients] [/ExcludedeletePending]
```
para Windows Server 2008 R2:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:{Boot | Install | All}] [/details:Clients]  [/ExcludedeletePending]
```
### <a name="parameters"></a>Parámetros

|        Parámetro        |                                                                                                                                                                                                                                                                   Explicación                                                                                                                                                                                                                                                                    |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |                                                                                                                                                                                 Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.                                                                                                                                                                                  |
|         /Show         | **Windows Server 2008**<p>/Show: clients: muestra información acerca de los equipos cliente que están conectados a las transmisiones de multidifusión.<p>**Windows Server 2008 R2**<p>Mostrar: {boot &#124; install &#124; All}: el tipo de imagen que se va a devolver.                                El **arranque** solo devuelve transmisiones de imagen de arranque.                                  La **instalación** solo devuelve transmisiones de imagen de instalación. **All** devuelve ambos tipos de imagen. |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /Details: clientes     |                                                                                                                                                                                              Solo se admite para Windows Server 2008 R2. Si está presente, se mostrarán los clientes que están conectados a la transmisión.                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              Excluye cualquier transmisión desactivada de la lista.                                                                                                                                                                                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver información acerca de todas las transmisiones, escriba:
- Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Show:All` para ver información acerca de todas las transmisiones excepto las transmisiones desactivadas, escriba:
- Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  ## <a name="additional-references"></a>Referencias adicionales
  - La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [usar el comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
  [con el comando New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
  [con el comando Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
  [Subcommand: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
