---
title: WDSUtil-habilitar-servidor
description: Artículo de referencia para WDSUtil Enable-Server, que habilita todos los servicios para servicios de implementación de Windows.
ms.topic: reference
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3ec32f2bd0d269795e1f88b967804c2bb82ac9f4
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730949"
---
# <a name="wdsutil-enable-server"></a>WDSUtil-habilitar-servidor

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Habilita todos los servicios para servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="examples"></a>Ejemplos
Para habilitar los servicios en el servidor, ejecute una de las siguientes opciones:
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [WDSUtil Disable-Server (comando)](wdsutil-disable-server.md) 
 comando Get- [Server de WDSUtil](wdsutil-get-server.md) 
 [WDSUtil Initialize-Server (comando)](wdsutil-initialize-server.md) 
 comando set- [Server de WDSUtil](wdsutil-set-server.md) 
 comando WDSUtil Start [-Server](wdsutil-start-server.md) 
 [WDSUtil STOP-Server (comando)](wdsutil-stop-server.md) 
 [WDSUtil uninitial-Server (comando)](wdsutil-uninitialize-server.md)
