---
title: Stop-TransportServer subcomando
description: Tema de los comandos de Windows para detener-TransportServer
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8f7410a8720337e509325b99863446bd8d19eb26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853456"
---
# <a name="subcommand-stop-transportserver"></a>Subcomando: stop-TransportServer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Detiene todos los servicios en un servidor de transporte.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si se especifica ningún servidor de transporte, se utilizará el servidor local.|
## <a name="BKMK_examples"></a>Ejemplos
Para detener los servicios, escriba uno de los siguientes:
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando disable-TransportServer](using-the-disable-transportserver-command.md)
[con el comando enable-TransportServer](using-the-enable-transportserver-command.md) 
 [ Mediante el comando get-TransportServer](using-the-get-transportserver-command.md)
[subcomando: set-TransportServer](subcommand-set-transportserver.md)
[subcomando: start-TransportServer](subcommand-start-transportserver.md)
