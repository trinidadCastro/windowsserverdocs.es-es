---
title: Get-TransportServer
description: Temas de comandos de Windows para Get-TransportServer, que muestra información sobre un servidor de transporte especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 549eb32536bda201a81e5f40ef408359fae39f4b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830818"
---
# <a name="get-transportserver"></a>Get-TransportServer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de un servidor de transporte especificado.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/Show: {config}|Devuelve información de configuración sobre el servidor de transporte especificado.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver información sobre el servidor, escriba:
```
wdsutil /Get-TransportServer /Show:Config
```
Para ver la información de configuración, escriba:
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando DISABLE-TransportServer](using-the-disable-transportserver-command.md)
[mediante el comando enable-TransportServer](using-the-enable-transportserver-command.md)
[Subcommand: set-TransportServer](subcommand-set-transportserver.md)
[Subcommand: Start-TransportServer](subcommand-start-transportserver.md)
[Subcommand: Stop-TransportServer](subcommand-stop-transportserver.md)
