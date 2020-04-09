---
title: Get-ImageGroup
description: Temas de comandos de Windows para Get-ImageGroup, que recupera información sobre un grupo de imágenes y las imágenes que hay en él.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0066e5d52c1d10b1f78ea627ee7a476bfd98f19d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830958"
---
# <a name="get-imagegroup"></a>Get-ImageGroup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información sobre un grupo de imágenes y las imágenes que contiene.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
mediaGroup:<Image group name>|Especifica el nombre del grupo de imágenes.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|[/detailed]|Devuelve los metadatos de la imagen de cada imagen. Si no se usa este parámetro, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver información acerca de un grupo de imágenes, escriba:
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Para ver información, incluidos los metadatos, escriba:
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando add-ImageGroup](using-the-add-imagegroup-command.md)
[mediante el comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)
[mediante el comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
[Subcommand: set-ImageGroup](subcommand-set-imagegroup.md)
