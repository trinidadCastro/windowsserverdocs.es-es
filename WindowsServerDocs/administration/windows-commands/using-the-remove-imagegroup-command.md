---
title: Remove-ImageGroup
description: Tema de comandos de Windows para Remove-ImageGroup, que quita un grupo de imágenes de un servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8406959037a958ea6d61b8e8145317635955e6d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830358"
---
# <a name="using-the-remove-imagegroup-command"></a>Usar el comando Remove-ImageGroup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita un grupo de imágenes de un servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
mediaGroup:<Image group name>|Especifica el nombre del grupo de imágenes que se va a quitar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para quitar el grupo de imágenes, escriba uno de los siguientes:
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:My Image Group /Server:MyWDSServer 
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
[Uso del comando Add-ImageGroup](using-the-add-imagegroup-command.md)  
[Usar el comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)  
[Usar el comando Get-ImageGroup](using-the-get-imagegroup-command.md)  
[Subcomando: set-ImageGroup](subcommand-set-imagegroup.md)  
