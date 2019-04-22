---
title: Mediante el comando get-AllImageGroups
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 917f61327a3d39ee97c5fd59072884f7844c487e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822356"
---
# <a name="using-the-get-allimagegroups-command"></a>Mediante el comando get-AllImageGroups

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información sobre todos los grupos de imágenes en un servidor y todas las imágenes de esos grupos de imágenes.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|[/ detallada]|Devuelve los metadatos de imagen de cada imagen. Si no se usa este parámetro, el comportamiento predeterminado es devolver el nombre de la imagen, descripción y nombre de archivo para cada imagen.|
## <a name="BKMK_examples"></a>Ejemplos
Para ver información acerca de los grupos de la imagen, escriba uno de los siguientes:
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando add-ImageGroup](using-the-add-imagegroup-command.md)
[con el comando get-ImageGroup](using-the-get-imagegroup-command.md)
[mediante el Comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
[subcomando: set-ImageGroup](subcommand-set-imagegroup.md)
