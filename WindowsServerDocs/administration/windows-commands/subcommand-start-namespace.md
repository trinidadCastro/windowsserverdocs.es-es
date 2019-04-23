---
title: Subcomando start-Namespace
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5dc484a3b159cd510d7a61484f4f46b1f1db7a4e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877136"
---
# <a name="subcommand-start-namespace"></a>Subcomando: start-Namespace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 inicia un espacio de nombres de difusión programada.
## <a name="syntax"></a>Sintaxis
```
wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/ Namespace:<Namespace name>|Especifica el nombre del espacio de nombres. Tenga en cuenta que esto no es el nombre descriptivo y debe ser único.<br /><br />-   **Servidor de implementación**: La sintaxis de nombre de espacio de nombres es /Namspace:WDS:<Image group>/<Image name>/<Index>. Por ejemplo: **WDS:ImageGroup1/install.wim/1**<br />-   **Servidor de transporte**: Este nombre debe coincidir con el nombre asignado al espacio de nombres cuando se creó en el servidor.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
## <a name="BKMK_examples"></a>Ejemplos
Para iniciar un espacio de nombres, escriba uno de los siguientes:
```
wdsutil /start-Namespace /Namespace:"Custom Auto 1"
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[con el comando nuevo Namespace](using-the-new-namespace-command.md)
[Using el comando remove-Namespace](using-the-remove-namespace-command.md)
