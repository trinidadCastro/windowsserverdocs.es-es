---
title: Mediante el comando get-TransportServer
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 08aa1273d09ba92de15e13f7bfcc8283ac2fedb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817426"
---
# <a name="using-the-get-transportserver-command"></a>Mediante el comando get-TransportServer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de un servidor de transporte especificado.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|/Show:{Config}|Devuelve información de configuración acerca del servidor de transporte especificado.|
## <a name="BKMK_examples"></a>Ejemplos
Para ver información acerca del servidor, escriba:
```
wdsutil /Get-TransportServer /Show:Config
```
Para ver información de configuración, escriba:
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando disable-TransportServer](using-the-disable-transportserver-command.md)
[con el comando enable-TransportServer](using-the-enable-transportserver-command.md) 
 [ Subcomando: set-TransportServer](subcommand-set-transportserver.md)
[subcomando: start-TransportServer](subcommand-start-transportserver.md)
[subcomando: stop-TransportServer](subcommand-stop-transportserver.md)
