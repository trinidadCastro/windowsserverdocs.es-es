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
ms.openlocfilehash: 30e4c2c7c5cf2668d62e96d8d2a54dc33e3d2a55
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960958"
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
#### <a name="additional-references"></a>referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
[Uso del comando Add-ImageGroup](using-the-add-imagegroup-command.md)  
[Usar el comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)  
[Usar el comando Get-ImageGroup](using-the-get-imagegroup-command.md)  
[Subcomando: set-ImageGroup](subcommand-set-imagegroup.md)  
