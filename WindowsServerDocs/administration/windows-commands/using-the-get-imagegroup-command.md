---
title: Get-ImageGroup
description: Artículo de referencia de Get-ImageGroup, que recupera información sobre un grupo de imágenes y las imágenes que hay en él.
ms.topic: reference
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c135072be3ff8ccf0993ed72ddf8f079f1d88c05
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638098"
---
# <a name="get-imagegroup"></a>Get-ImageGroup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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
## <a name="examples"></a>Ejemplos
Para ver información acerca de un grupo de imágenes, escriba:
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Para ver información, incluidos los metadatos, escriba:
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Uso del comando](using-the-add-imagegroup-command.md) 
 Add-ImageGroup [Usar el comando](using-the-get-allimagegroups-command.md) 
 Get-AllImageGroups [Usar el comando](using-the-remove-imagegroup-command.md) 
 Remove-ImageGroup [Subcomando: set-ImageGroup](subcommand-set-imagegroup.md)
