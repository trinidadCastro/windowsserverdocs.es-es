---
title: Add-ImageGroup
description: Tema de comandos de Windows para Add-ImageGroup, que agrega un grupo de imágenes a un servidor de servicios de implementación de Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f12ed27ca1a809ec34dbefbc4ff7288ff194a83e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831898"
---
# <a name="add-imagegroup"></a>Add-ImageGroup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agrega un grupo de imágenes a un servidor de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
mediaGroup:<Image group name>|Especifica el nombre del grupo de imágenes que se va a agregar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica un nombre de servidor, se usará el servidor local.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para agregar un grupo de imágenes, escriba uno de los siguientes:
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando get-AllImageGroups](using-the-get-allimagegroups-command.md)
[mediante el comando Get-ImageGroup](using-the-get-imagegroup-command.md)
[mediante el comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
[Subcommand: set-ImageGroup](subcommand-set-imagegroup.md)
