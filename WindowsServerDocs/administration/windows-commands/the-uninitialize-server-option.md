---
title: anular la inicialización del servidor
description: Tema de referencia sobre la anulación de la inicialización del servidor, que revierte los cambios realizados en el servidor durante la configuración inicial del servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d7747a44172b7382bd22a7d48ccc717a89ccacb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721393"
---
# <a name="uninitialize-server"></a>anular la inicialización del servidor

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Revierte los cambios realizados en el servidor durante la configuración inicial del servidor. Esto incluye los cambios realizados por la opción **/Initialize-Server** o el complemento MMC de servicios de implementación de Windows. Tenga en cuenta que este comando restablece el estado de configuración del servidor. Este comando no modifica el contenido de la carpeta compartida remoteInstall. En su lugar, restablece el estado del servidor para que pueda reinicializar el servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="examples"></a>Ejemplos
Para reinicializar el servidor, escriba uno de los siguientes:
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave](command-line-syntax-key.md)
de sintaxis de línea de comandos[mediante el comando DISABLE-Server](using-the-disable-server-command.md)
mediante el comando[enable-Server](using-the-enable-server-command.md)
mediante el
[comando Get-Server](using-the-get-server-command.md)[mediante el subcomando Initialize-Server Command](using-the-initialize-server-command.md)
[: set-](subcommand-set-server.md)
Server Subcommand[: Start-](subcommand-start-server.md)
Server[Subcommand: Stop-Server](subcommand-stop-server.md)
