---
title: Subcomando STOP-TransportServer
description: Tema de comandos de Windows para STOP-TransportServer
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44d62332307ffda4dcfa6af286c7b95cb12423dc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833708"
---
# <a name="subcommand-stop-transportserver"></a>Subcomando: Stop-TransportServer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Detiene todos los servicios en un servidor de transporte.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún servidor de transporte, se usará el servidor local.|
## <a name="examples"></a><a name="BKMK_examples"></a>Example
Para detener los servicios, escriba uno de los siguientes:
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando DISABLE-TransportServer](using-the-disable-transportserver-command.md)
[mediante el comando enable-TransportServer](using-the-enable-transportserver-command.md)
[mediante el comando Get-TransportServer](using-the-get-transportserver-command.md)
[Subcommand: set-TransportServer](subcommand-set-transportserver.md)
[Subcommand: Start-TransportServer](subcommand-start-transportserver.md)
