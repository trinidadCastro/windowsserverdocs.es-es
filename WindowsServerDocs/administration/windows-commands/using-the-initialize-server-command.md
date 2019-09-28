---
title: Usar el comando Initialize-Server
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9e95972838fc70ee1e617d1e299c9e35db5b979
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392093"
---
# <a name="using-the-initialize-server-command"></a>Usar el comando Initialize-Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura un servidor de servicios de implementación de Windows para su uso inicial una vez instalado el rol de servidor. Después de ejecutar este comando, debe usar el comando de [comando Add-image](using-the-add-image-command.md) para agregar imágenes al servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/remInst: "<Full path>"|Especifica la ruta de acceso completa y el nombre de la carpeta remoteInstall. Si la carpeta especificada no existe todavía, esta opción la creará cuando se ejecute el comando. Siempre debe especificar una ruta de acceso local, incluso en el caso de un equipo remoto. Por ejemplo: **D:\remoteInstall**.|
|/Authorize|Autoriza el servidor en el protocolo de control de host dinámico (DHCP). Esta opción solo es necesaria si está habilitada la detección no autorizada de DHCP, lo que significa que el servidor PXE de servicios de implementación de Windows debe estar autorizado en DHCP para poder atender los equipos cliente. Tenga en cuenta que la detección no autorizada de DHCP está deshabilitada de forma predeterminada.|
## <a name="BKMK_examples"></a>Example
Para inicializar el servidor y establecer la carpeta compartida remoteInstall en la unidad F:, escriba.
```
wdsutil /Initialize-Server /remInst:"F:\remoteInstall"
```
Para inicializar el servidor y establecer la carpeta compartida remoteInstall en la unidad C:, escriba.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:"C:\remoteInstall"
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando DISABLE-Server](using-the-disable-server-command.md)
[mediante el comando Enable-Server](using-the-enable-server-command.md)
[mediante el comando Get-Server](using-the-get-server-command.md)
[Subcommand: set-Server](subcommand-set-server.md)
[Subcommand: subcomando Start-Server](subcommand-start-server.md)1[: Stop-Server](subcommand-stop-server.md)3[The UnInitialize-Server Option](the-uninitialize-server-option.md)
