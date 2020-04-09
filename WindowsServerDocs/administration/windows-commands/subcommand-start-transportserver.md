---
title: Subcomando Start-TransportServer
description: Tema de comandos de Windows para el subcomando Start-TransportServer, que inicia todos los servicios de un servidor de transporte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1356d415006324d75783d4e12ad6882d0fcc779
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833758"
---
# <a name="subcommand-start-transportserver"></a>Subcomando: Start-TransportServer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia todos los servicios de un servidor de transporte.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para iniciar el servidor, escriba uno de los siguientes:
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando DISABLE-TransportServer](using-the-disable-transportserver-command.md)
[usar el comando enable-TransportServer](using-the-enable-transportserver-command.md)
[mediante el comando Get-TransportServer](using-the-get-transportserver-command.md)
[Subcommand: set-TransportServer](subcommand-set-transportserver.md)
[Subcommand: Stop-TransportServer](subcommand-stop-transportserver.md)
