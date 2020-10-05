---
title: WDSUtil UnInitialize-Server
description: Artículo de referencia para la anulación de la inicialización del servidor, que revierte los cambios realizados en el servidor durante la configuración inicial del servidor.
ms.topic: reference
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4f8f7623c31f355ece5df389af474001d996e755
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730621"
---
# <a name="wdsutil-uninitialize-server"></a>WDSUtil UnInitialize-Server

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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Disable-Server (comando)](wdsutil-disable-server.md)
- [WDSUtil Enable-Server (comando)](wdsutil-enable-server.md)
- [comando Get-Server de WDSUtil](wdsutil-get-server.md)
- [WDSUtil Initialize-Server (comando)](wdsutil-initialize-server.md)
- [comando set-Server de WDSUtil](wdsutil-set-server.md)
- [comando WDSUtil Start-Server](wdsutil-start-server.md)
- [WDSUtil STOP-Server (comando)](wdsutil-stop-server.md)
