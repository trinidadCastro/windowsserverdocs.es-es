---
title: La opción de servidor uninitialize
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73f1ff67331ae41fa0d88cb3a16df5095e0b6d66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873986"
---
# <a name="the-uninitialize-server-option"></a>La opción de servidor uninitialize

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

revierte los cambios realizados en el servidor durante la configuración inicial del servidor. Esto incluye los cambios realizados, ya sea por la **/initialize-server** opción o el complemento mmc servicios de implementación de Windows. Tenga en cuenta que este comando restablece el servidor a un estado no configurado. Este comando no modifica el contenido de la carpeta remoteInstall. En su lugar, restablece el estado del servidor para que se puede reinicializar el servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
## <a name="BKMK_examples"></a>Ejemplos
Para reinicializar el servidor, escriba uno de los siguientes:
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando disable-servidor](using-the-disable-server-command.md)
[mediante el comando enable-servidor](using-the-enable-server-command.md)
[mediante el Comando Get-Server](using-the-get-server-command.md)
[con el comando Initialize-Server](using-the-initialize-server-command.md)
[subcomando: set-Server](subcommand-set-server.md) 
 [ Subcomando: start-Server](subcommand-start-server.md)
[subcomando: detención del servidor](subcommand-stop-server.md)
