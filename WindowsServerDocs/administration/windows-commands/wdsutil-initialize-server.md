---
title: WDSUtil-inicializar-servidor
description: Artículo de referencia para WDSUtil Initialize-Server, que configura un servidor de servicios de implementación de Windows para su uso inicial después de haber instalado el rol de servidor.
ms.topic: reference
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 42d579b7139f7ff516d9ff239c535ecd2be42474
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730766"
---
# <a name="wdsutil-initialize-server"></a>WDSUtil-inicializar-servidor

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Configura un servidor de servicios de implementación de Windows para su uso inicial una vez instalado el rol de servidor. Después de ejecutar este comando, debe usar el comando [de comando wdsutiladd-Image](wdsutil-add-image.md) para agregar imágenes al servidor.
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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Disable-Server (comando)](wdsutil-disable-server.md)
- [WDSUtil Enable-Server (comando)](wdsutil-enable-server.md)
- [comando Get-Server de WDSUtil](wdsutil-get-server.md)
- [comando set-Server de WDSUtil](wdsutil-set-server.md)
- [comando WDSUtil Start-Server](wdsutil-start-server.md)
- [WDSUtil STOP-Server (comando)](wdsutil-stop-server.md)
- [WDSUtil uninitial-Server (comando)](wdsutil-uninitialize-server.md)
