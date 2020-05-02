---
title: Inicializar-servidor
description: Tema de referencia sobre Initialize-Server, que configura un servidor de servicios de implementación de Windows para su uso inicial después de haber instalado el rol de servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54180923a077c0b423e73588bcbd1c03b0154d08
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719721"
---
# <a name="initialize-server"></a>Inicializar-servidor

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Configura un servidor de servicios de implementación de Windows para su uso inicial una vez instalado el rol de servidor. Después de ejecutar este comando, debe usar el comando de [comando Add-image](using-the-add-image-command.md) para agregar imágenes al servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|remInst<Full path>|Especifica la ruta de acceso completa y el nombre de la carpeta remoteInstall. Si la carpeta especificada no existe todavía, esta opción la creará cuando se ejecute el comando. Siempre debe especificar una ruta de acceso local, incluso en el caso de un equipo remoto. Por ejemplo: **D:\remoteInstall**.|
|/Authorize|Autoriza el servidor en el protocolo de control de host dinámico (DHCP). Esta opción solo es necesaria si está habilitada la detección no autorizada de DHCP, lo que significa que el servidor PXE de servicios de implementación de Windows debe estar autorizado en DHCP para poder atender los equipos cliente. Tenga en cuenta que la detección no autorizada de DHCP está deshabilitada de forma predeterminada.|
## <a name="examples"></a>Ejemplos
Para inicializar el servidor y establecer la carpeta compartida remoteInstall en la unidad F:, escriba.
```
wdsutil /Initialize-Server /remInst:F:\remoteInstall
```
Para inicializar el servidor y establecer la carpeta compartida remoteInstall en la unidad C:, escriba.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:C:\remoteInstall
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave](command-line-syntax-key.md)
de sintaxis de línea de comandos[mediante el comando](using-the-disable-server-command.md)
Disable
-Server mediante el[comando Enable-Server](using-the-enable-server-command.md)[mediante el comando Get-Server Command](using-the-get-server-command.md)
[Subcommand: set-Server](subcommand-set-server.md)
Subcommand[: Start-](subcommand-start-server.md)
Server[Subcommand: Stop-Server](subcommand-stop-server.md)
[la opción UnInitialize-Server](the-uninitialize-server-option.md)
