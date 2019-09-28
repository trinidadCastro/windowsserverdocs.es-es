---
title: Subcomando Start-namespace
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 55fe4a6136fe4f8e886dc62fff746a1e5ff1898f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370750"
---
# <a name="subcommand-start-namespace"></a>Subcomando: Start-namespace

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 inicia un espacio de nombres de difusión programada.
> ## <a name="syntax"></a>Sintaxis
> ```
> wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
> ```
> ## <a name="parameters"></a>Parámetros
> 
> |          Parámetro          |                                                                                                                                                                                             Descripción                                                                                                                                                                                             |
> |-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | /Namespace: <Namespace name> | Especifica el nombre del espacio de nombres. Tenga en cuenta que este no es el nombre descriptivo y debe ser único.<br /><br />**servidor de implementación**-   : La sintaxis del nombre de espacio de nombres es/Namspace: WDS: <Image group> @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4. Por ejemplo: **WDS: ImageGroup1/install. Wim/1**<br />**servidor de transporte**-   : Este nombre debe coincidir con el nombre asignado al espacio de nombres cuando se creó en el servidor. |
> |   [/Server:<Server name>]   |                                                                                                           Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.                                                                                                           |
> 
> ## <a name="BKMK_examples"></a>Example
> Para iniciar un espacio de nombres, escriba uno de los siguientes:
> ```
> wdsutil /start-Namespace /Namespace:"Custom Auto 1"
> wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
> ```
> #### <a name="additional-references"></a>Referencias adicionales
> [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> [mediante el comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
> [con el comando New-namespace](using-the-new-namespace-command.md)
> [mediante el comando Remove-namespace](using-the-remove-namespace-command.md)
