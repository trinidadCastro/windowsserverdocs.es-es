---
title: Usar el comando Remove-ImageGroup
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51d9636013fef182c4abb74ae196e08c51ff11d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362746"
---
# <a name="using-the-remove-imagegroup-command"></a>Usar el comando Remove-ImageGroup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

quita un grupo de imágenes de un servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
mediaGroup: <Image group name>|Especifica el nombre del grupo de imágenes que se va a quitar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="BKMK_examples"></a>Example
Para quitar el grupo de imágenes, escriba uno de los siguientes:
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:"My Image Group" /Server:MyWDSServer 
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando add-ImageGroup](using-the-add-imagegroup-command.md)
[mediante el comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)
[mediante el comando Get-ImageGroup](using-the-get-imagegroup-command.md)
[Subcommand: set-ImageGroup](subcommand-set-imagegroup.md)
