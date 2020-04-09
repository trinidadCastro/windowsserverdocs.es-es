---
title: Get-Server
description: Tema de comandos de Windows para Get-Server, que recupera información del servidor de servicios de implementación de Windows especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65903dc89730eb9d1da23be31ecc1909daece9c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830848"
---
# <a name="get-server"></a>Get-Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información del servidor de servicios de implementación de Windows especificado.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
|/Show: {config &#124; images &#124; All}|Especifica el tipo de información que se va a devolver.<p>-   **configuración** devuelve información de configuración.<br />-   **imágenes** devuelve información acerca de los grupos de imágenes, las imágenes de arranque y las imágenes de instalación.<br />-   **All** devuelve información de configuración e información de la imagen.|
|[/detailed]|Puede usar esta opción con **/Show: images** o **/Show: ALL** para indicar que se deben devolver todos los metadatos de imagen de cada imagen. Si no se usa la opción **/Detailed** , el comportamiento predeterminado es devolver el nombre de la imagen, la descripción y el nombre de archivo.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver información sobre el servidor, escriba:
```
wdsutil /Get-Server /Show:Config
```
Para ver información detallada acerca del servidor, escriba:
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando DISABLE-Server](using-the-disable-server-command.md)
[mediante el comando Enable-Server](using-the-enable-server-command.md)
[mediante el comando Initialize-Server](using-the-initialize-server-command.md)
[Subcommand: set-Server](subcommand-set-server.md)
[Subcommand: Start-Server](subcommand-start-server.md)
[Subcommand: Stop-Server](subcommand-stop-server.md)
[la opción UnInitialize-](the-uninitialize-server-option.md) Server
