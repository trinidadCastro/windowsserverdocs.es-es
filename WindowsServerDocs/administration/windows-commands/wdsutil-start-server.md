---
title: WDSUtil-Inicio-servidor
description: Artículo de referencia para el subcomando Start-Server, que inicia todos los servicios de un servidor de servicios de implementación de Windows.
ms.topic: reference
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 03791bd3a313f1ab2a5a2e076036d0aaddb18a8e
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731369"
---
# <a name="wdsutil-start-server"></a>WDSUtil-Inicio-servidor

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Inicia todos los servicios de un servidor de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor que se va a iniciar. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="examples"></a>Ejemplos
Para iniciar el servidor, escriba uno de los siguientes:
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Disable-Server (comando)](wdsutil-disable-server.md)
- [WDSUtil Enable-Server (comando)](wdsutil-enable-server.md)
- [comando Get-Server de WDSUtil](wdsutil-get-server.md)
- [WDSUtil Initialize-Server (comando)](wdsutil-initialize-server.md)
- [comando set-Server de WDSUtil](wdsutil-set-server.md)
- [WDSUtil STOP-Server (comando)](wdsutil-stop-server.md)
- [comando WDSUtil Start-Server](wdsutil-start-server.md)
- [WDSUtil uninitial-Server (comando)](wdsutil-uninitialize-server.md)
