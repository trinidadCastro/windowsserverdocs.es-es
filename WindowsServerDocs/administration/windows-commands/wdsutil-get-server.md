---
title: WDSUtil Get-Server
description: Artículo de referencia para WDSUtil Get-Server, que recupera información del servidor de servicios de implementación de Windows especificado.
ms.topic: reference
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e855724833a90c64dfb3692ccd82fad67a572873
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730781"
---
# <a name="wdsutil-get-server"></a>WDSUtil Get-Server

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera información del servidor de servicios de implementación de Windows especificado.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
|/Show: {config &#124; imágenes &#124; todas}|Especifica el tipo de información que se va a devolver.<p>-   **Config** devuelve información de configuración.<br />-   **Imágenes** devuelve información acerca de los grupos de imágenes, las imágenes de arranque y las imágenes de instalación.<br />-   **All** devuelve información de configuración e información de imagen.|
|[/detailed]|Puede usar esta opción con **/Show: images** o **/Show: ALL** para indicar que se deben devolver todos los metadatos de imagen de cada imagen. Si no se usa la opción **/Detailed** , el comportamiento predeterminado es devolver el nombre de la imagen, la descripción y el nombre de archivo.|
## <a name="examples"></a>Ejemplos
Para ver información sobre el servidor, escriba:
```
wdsutil /Get-Server /Show:Config
```
Para ver información detallada acerca del servidor, escriba:
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Disable-Server (comando)](wdsutil-disable-server.md)
- [WDSUtil Enable-Server (comando)](wdsutil-enable-server.md)
- [WDSUtil Initialize-Server (comando)](wdsutil-initialize-server.md)
- [comando set-Server de WDSUtil](wdsutil-set-server.md)
- [comando WDSUtil Start-Server](wdsutil-start-server.md)
- [WDSUtil STOP-Server (comando)](wdsutil-stop-server.md)
- [WDSUtil uninitial-Server (comando)](wdsutil-uninitialize-server.md)
