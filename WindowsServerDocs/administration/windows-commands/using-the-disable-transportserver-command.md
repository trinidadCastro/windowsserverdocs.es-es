---
title: Disable-TransportServer
description: Windows Commands tema para Disable-TransportServer, que deshabilita todos los servicios de un servidor de transporte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39f930a464364cda680098ef4e7e1081d0995503
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831628"
---
# <a name="disable-transportserver"></a>Disable-TransportServer

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Deshabilita todos los servicios de un servidor de transporte.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor de transporte que se va a deshabilitar. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor de transporte, se usará el servidor local.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para deshabilitar el servidor, escriba:
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando enable-TransportServer](using-the-enable-transportserver-command.md)
[mediante el comando Get-TransportServer](using-the-get-transportserver-command.md)
[Subcommand: set-TransportServer](subcommand-set-transportserver.md)
[Subcommand: Start-TransportServer](subcommand-start-transportserver.md)
[Subcommand: Stop-TransportServer](subcommand-stop-transportserver.md)
