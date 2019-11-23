---
title: Usar el comando Get-Server
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7e0ee4529858b16cdc63ea1d6d358a190b8b1a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392121"
---
# <a name="using-the-get-server-command"></a>Usar el comando Get-Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información del servidor de servicios de implementación de Windows especificado.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
|/Show: {config &#124; images &#124; All}|Especifica el tipo de información que se va a devolver.<br /><br />-   **configuración** devuelve información de configuración.<br />-   **imágenes** devuelve información acerca de los grupos de imágenes, las imágenes de arranque y las imágenes de instalación.<br />-   **All** devuelve información de configuración e información de la imagen.|
|[/detailed]|Puede usar esta opción con **/Show: images** o **/Show: ALL** para indicar que se deben devolver todos los metadatos de imagen de cada imagen. Si no se usa la opción **/Detailed** , el comportamiento predeterminado es devolver el nombre de la imagen, la descripción y el nombre de archivo.|
## <a name="BKMK_examples"></a>Example
Para ver información sobre el servidor, escriba:
```
wdsutil /Get-Server /Show:Config
```
Para ver información detallada acerca del servidor, escriba:
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>referencias adicionales
La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando DISABLE-Server](using-the-disable-server-command.md)
[mediante el comando Enable-Server](using-the-enable-server-command.md)
[mediante el comando Initialize-Server](using-the-initialize-server-command.md)
[Subcommand: set-Server](subcommand-set-server.md)
[Subcommand: Start-Server](subcommand-start-server.md)
[Subcommand: Stop-Server](subcommand-stop-server.md)
[la opción UnInitialize-](the-uninitialize-server-option.md) Server
