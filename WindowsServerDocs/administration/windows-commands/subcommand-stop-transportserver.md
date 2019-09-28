---
title: Subcomando STOP-TransportServer
description: Tema de comandos de Windows para STOP-TransportServer
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a2444328a426429c2dce5ceee3272cf1dc814cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370719"
---
# <a name="subcommand-stop-transportserver"></a>Subcomando: Stop-TransportServer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Detiene todos los servicios en un servidor de transporte.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún servidor de transporte, se usará el servidor local.|
## <a name="BKMK_examples"></a>Example
Para detener los servicios, escriba uno de los siguientes:
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando DISABLE-TransportServer](using-the-disable-transportserver-command.md)
[mediante el comando enable-TransportServer](using-the-enable-transportserver-command.md)
[mediante el comando Get-TransportServer](using-the-get-transportserver-command.md)
[: subcomando set-TransportServer](subcommand-set-transportserver.md)
[: Start-TransportServer](subcommand-start-transportserver.md)
