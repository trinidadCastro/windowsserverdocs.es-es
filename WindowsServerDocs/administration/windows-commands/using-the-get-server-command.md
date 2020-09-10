---
title: Get-Server
description: Artículo de referencia de Get-Server, que recupera información del servidor de servicios de implementación de Windows especificado.
ms.topic: reference
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fe55e707dda58e65d2b86fe553910d010f8b2586
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628507"
---
# <a name="get-server"></a>Get-Server

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
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-disable-server-command.md) 
 Disable-Server [Usar el comando](using-the-enable-server-command.md) 
 enable-Server [Usar el comando](using-the-initialize-server-command.md) 
 Initialize-Server [Subcomando: set-Server](subcommand-set-server.md) 
 [Subcomando: Start-Server](subcommand-start-server.md) 
 [Subcomando: Stop-Server](subcommand-stop-server.md) 
 [La opción UnInitialize-Server](the-uninitialize-server-option.md)
