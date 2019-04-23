---
title: Mediante el comando enable-TransportServer
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 40793ac0b9dc7d8b4a80d6a66b55244202aa37d1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834336"
---
# <a name="using-the-enable-transportserver-command"></a>Mediante el comando enable-TransportServer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que todos los servicios para el servidor de transporte.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si se especifica ningún nombre, se usará el servidor local.|
## <a name="BKMK_examples"></a>Ejemplos
Para habilitar los servicios en el servidor, ejecute uno de los siguientes:
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando disable-TransportServer](using-the-disable-transportserver-command.md)
[con el comando get-TransportServer](using-the-get-transportserver-command.md) 
 [Subcomando: set-TransportServer](subcommand-set-transportserver.md)
[subcomando: start-TransportServer](subcommand-start-transportserver.md)
[subcomando: stop-TransportServer](subcommand-stop-transportserver.md)
