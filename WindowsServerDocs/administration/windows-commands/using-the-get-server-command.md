---
title: Mediante el comando get-servidor
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: da8bf0fc6e31bd8d0079933f1d7c529c4fe96f42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870986"
---
# <a name="using-the-get-server-command"></a>Mediante el comando get-servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información desde el servidor de servicios de implementación de Windows especificado.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usa el servidor local.|
|/ Mostrar: {Config &#124; imágenes &#124; todas}|Especifica el tipo de información que se va a devolver.<br /><br />-   **Configuración** devuelve información de configuración.<br />-   **Imágenes** devuelve información acerca de los grupos de imágenes, las imágenes de arranque y las imágenes de instalación.<br />-   **Todos los** devuelve información de configuración y la información de la imagen.|
|[/ detallada]|Puede usar esta opción con **/Show:Images** o **/Show:All** para indicar que se deben devolver todos los metadatos de imagen de cada imagen. Si el **/ detallada** no se usa la opción, el comportamiento predeterminado es devolver el nombre de la imagen, descripción y nombre de archivo.|
## <a name="BKMK_examples"></a>Ejemplos
Para ver información acerca del servidor, escriba:
```
wdsutil /Get-Server /Show:Config
```
Para ver información detallada sobre el servidor, escriba:
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando disable-servidor](using-the-disable-server-command.md)
[mediante el comando enable-servidor](using-the-enable-server-command.md)
[mediante el Comando Initialize-Server](using-the-initialize-server-command.md)
[subcomando: set-Server](subcommand-set-server.md)
[subcomando: iniciar el servidor](subcommand-start-server.md) 
 [ Subcomando: detención del servidor](subcommand-stop-server.md)
[la opción de servidor uninitialize](the-uninitialize-server-option.md)
