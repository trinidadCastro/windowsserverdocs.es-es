---
title: anular la inicialización del servidor
description: En el tema comandos de Windows para la anulación de la inicialización, se revierten los cambios realizados en el servidor durante la configuración inicial del servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ed912af1ec3bc505e1e7465650a119af979b407
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833118"
---
# <a name="uninitialize-server"></a>anular la inicialización del servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Revierte los cambios realizados en el servidor durante la configuración inicial del servidor. Esto incluye los cambios realizados por la opción **/Initialize-Server** o el complemento MMC de servicios de implementación de Windows. Tenga en cuenta que este comando restablece el estado de configuración del servidor. Este comando no modifica el contenido de la carpeta compartida remoteInstall. En su lugar, restablece el estado del servidor para que pueda reinicializar el servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para reinicializar el servidor, escriba uno de los siguientes:
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [La clave de sintaxis de línea de comandos](command-line-syntax-key.md)
usar el comando [Disable-Server](using-the-disable-server-command.md)
[mediante el comando Enable-Server](using-the-enable-server-command.md)
[mediante el comando Get-Server](using-the-get-server-command.md)
[mediante el comando Initialize-Server](using-the-initialize-server-command.md)
[Subcommand: set-Server](subcommand-set-server.md)
[Subcommand: Start-Server](subcommand-start-server.md)
[Subcommand: Stop-Server](subcommand-stop-server.md)
