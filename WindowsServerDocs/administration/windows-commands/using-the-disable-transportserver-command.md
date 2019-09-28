---
title: Usar el comando DISABLE-TransportServer
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7531f8a638ac8fabdad08cc0134dbc63873505de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363463"
---
# <a name="using-the-disable-transportserver-command"></a>Usar el comando DISABLE-TransportServer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Deshabilita todos los servicios de un servidor de transporte.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte que se va a deshabilitar. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor de transporte, se usará el servidor local.|
## <a name="BKMK_examples"></a>Example
Para deshabilitar el servidor, escriba:
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando enable-TransportServer](using-the-enable-transportserver-command.md)
[mediante el comando Get-TransportServer](using-the-get-transportserver-command.md)
[Subcommand: set-TransportServer](subcommand-set-transportserver.md)
[Subcommand: subcomando Start-TransportServer](subcommand-start-transportserver.md)
[: Stop-TransportServer](subcommand-stop-transportserver.md)
