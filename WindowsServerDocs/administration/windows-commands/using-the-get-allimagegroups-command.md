---
title: Get-AllImageGroups
description: Temas de comandos de Windows para Get-AllImageGroups, que recupera información sobre todos los grupos de imágenes de un servidor y todas las imágenes de esos grupos de imágenes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19285422612f8be34d39e6152fcf0300e1919b8a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831337"
---
# <a name="get-allimagegroups"></a>Get-AllImageGroups

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información sobre todos los grupos de imágenes de un servidor y todas las imágenes de esos grupos de imágenes.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|[/detailed]|Devuelve los metadatos de la imagen de cada imagen. Si no se utiliza este parámetro, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo de cada imagen.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver información acerca de los grupos de imágenes, escriba uno de los siguientes:
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando add-ImageGroup](using-the-add-imagegroup-command.md)
[mediante el comando Get-ImageGroup](using-the-get-imagegroup-command.md)
[mediante el comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
[Subcommand: set-ImageGroup](subcommand-set-imagegroup.md)
