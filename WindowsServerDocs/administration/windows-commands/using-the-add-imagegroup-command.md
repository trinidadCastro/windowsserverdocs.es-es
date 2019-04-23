---
title: Con el comando add-ImageGroup
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 71050bfecdac4933bfe36f40ce09dae626735664
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829906"
---
# <a name="using-the-add-imagegroup-command"></a>Con el comando add-ImageGroup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agrega un grupo de imágenes a un servidor de servicios de implementación de Windows.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
mediaGroup:<Image group name>|Especifica el nombre del grupo de imagen que se va a agregar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica un nombre de servidor, se usará el servidor local.|
## <a name="BKMK_examples"></a>Ejemplos
Para agregar un grupo de imágenes, escriba uno de los siguientes:
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:"My Image Group" /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando get-AllImageGroups](using-the-get-allimagegroups-command.md)
[con el comando get-ImageGroup](using-the-get-imagegroup-command.md) 
 [ Con el comando remove-ImageGroup](using-the-remove-imagegroup-command.md)
[subcomando: set-ImageGroup](subcommand-set-imagegroup.md)
