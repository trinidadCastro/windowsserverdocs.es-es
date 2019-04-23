---
title: Subcomando start-Server
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a89e3f17aeed7eb3156e28997206517342be109
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852046"
---
# <a name="subcommand-start-server"></a>Subcomando: start-Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

inicia todos los servicios para un servidor de servicios de implementación de Windows.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor al que se puede iniciar. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
## <a name="BKMK_examples"></a>Ejemplos
Para iniciar el servidor, escriba uno de los siguientes:
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando disable-servidor](using-the-disable-server-command.md)
[mediante el comando enable-servidor](using-the-enable-server-command.md)
[mediante el Comando Get-Server](using-the-get-server-command.md)
[con el comando Initialize-Server](using-the-initialize-server-command.md)
[subcomando: set-Server](subcommand-set-server.md) 
 [ Subcomando: detención del servidor](subcommand-stop-server.md)
[la opción de servidor uninitialize](the-uninitialize-server-option.md)
