---
title: Mediante el comando enable-servidor
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdf03a778a6c646aa79c2f844212b1728c5c73eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852726"
---
# <a name="using-the-enable-server-command"></a>Mediante el comando enable-servidor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que todos los servicios para servicios de implementación de Windows.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
## <a name="BKMK_examples"></a>Ejemplos
Para habilitar los servicios en el servidor, ejecute uno de los siguientes:
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando disable-servidor](using-the-disable-server-command.md)
[mediante el comando get-servidor](using-the-get-server-command.md)
[mediante el Comando Initialize-Server](using-the-initialize-server-command.md)
[subcomando: set-Server](subcommand-set-server.md)
[subcomando: iniciar el servidor](subcommand-start-server.md) 
 [ Subcomando: detención del servidor](subcommand-stop-server.md)
[la opción de servidor uninitialize](the-uninitialize-server-option.md)
