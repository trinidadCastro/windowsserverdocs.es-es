---
title: anular la inicialización del servidor
description: Artículo de referencia para la anulación de la inicialización del servidor, que revierte los cambios realizados en el servidor durante la configuración inicial del servidor.
ms.topic: reference
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ce98df7fa7c094970474432dd8fdedc56e302c6
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029983"
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
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-disable-server-command.md) 
 Disable-Server [Usar el comando](using-the-enable-server-command.md) 
 enable-Server [Usar el comando](using-the-get-server-command.md) 
 Get-Server [Usar el comando](using-the-initialize-server-command.md) 
 Initialize-Server [Subcomando: set-Server](subcommand-set-server.md) 
 [Subcomando: Start-Server](subcommand-start-server.md) 
 [Subcomando: Stop-Server](subcommand-stop-server.md)
