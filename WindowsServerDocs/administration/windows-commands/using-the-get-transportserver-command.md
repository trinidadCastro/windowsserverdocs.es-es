---
title: Usar el comando Get-TransportServer
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 282b69162cf3550c5bcba3282b60f15072c96ed6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363075"
---
# <a name="using-the-get-transportserver-command"></a>Usar el comando Get-TransportServer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de un servidor de transporte especificado.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/Show: {config}|Devuelve información de configuración sobre el servidor de transporte especificado.|
## <a name="BKMK_examples"></a>Example
Para ver información sobre el servidor, escriba:
```
wdsutil /Get-TransportServer /Show:Config
```
Para ver la información de configuración, escriba:
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando DISABLE-TransportServer](using-the-disable-transportserver-command.md)
[mediante el comando enable-TransportServer](using-the-enable-transportserver-command.md)
[Subcommand: set-TransportServer](subcommand-set-transportserver.md)
[Subcommand: subcomando Start-TransportServer](subcommand-start-transportserver.md)
[: Stop-TransportServer](subcommand-stop-transportserver.md)
