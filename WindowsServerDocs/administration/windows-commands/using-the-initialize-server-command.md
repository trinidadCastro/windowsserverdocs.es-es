---
title: Mediante el comando Initialize-Server
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4a1f3a1c02eb21f630aaff7219610864cd1db2a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825196"
---
# <a name="using-the-initialize-server-command"></a>Mediante el comando Initialize-Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura un servidor de servicios de implementación de Windows para su uso inicial una vez instalado el rol de servidor. Después de ejecutar este comando, debe usar el [mediante el comando add-Image](using-the-add-image-command.md) comando para agregar imágenes al servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|/remInst:"<Full path>"|Especifica la ruta de acceso completa y el nombre de la carpeta remoteInstall. Si la carpeta especificada no existe, esta opción se creará cuando se ejecuta el comando. Siempre debe especificar una ruta de acceso local, incluso en el caso de un equipo remoto. Por ejemplo: **D:\remoteInstall**.|
|[/ Autorizar]|Autorice el servidor de protocolo de Control dinámico de Host (DHCP). Esta opción es necesaria solo si está habilitada la detección rogue DHCP, lo que significa que PXE de servicios de implementación de Windows server debe estar autorizada en DHCP para que los equipos cliente que pueden ser reparados. Tenga en cuenta que la detección rogue DHCP está deshabilitada de forma predeterminada.|
## <a name="BKMK_examples"></a>Ejemplos
Para inicializar el servidor y establece la carpeta remoteInstall compartida en la unidad F:, escriba.
```
wdsutil /Initialize-Server /remInst:"F:\remoteInstall"
```
Para inicializar el servidor y establece la carpeta remoteInstall compartida en la unidad C:, escriba.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:"C:\remoteInstall"
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando disable-servidor](using-the-disable-server-command.md)
[mediante el comando enable-servidor](using-the-enable-server-command.md)
[mediante el Comando Get-Server](using-the-get-server-command.md)
[subcomando: set-Server](subcommand-set-server.md)
[subcomando: iniciar el servidor](subcommand-start-server.md)
[subcomando: detención del servidor](subcommand-stop-server.md)
[la opción de servidor uninitialize](the-uninitialize-server-option.md)
