---
title: Add-ImageGroup
description: Artículo de referencia de Add-ImageGroup, que agrega un grupo de imágenes a un servidor de servicios de implementación de Windows.
ms.topic: article
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 611425c7c3ff997009e16792fea1924a403f364e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896592"
---
# <a name="add-imagegroup"></a>Add-ImageGroup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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
## <a name="examples"></a>Ejemplos
Para agregar un grupo de imágenes, escriba uno de los siguientes:
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-get-allimagegroups-command.md) 
 Get-AllImageGroups [Usar el comando](using-the-get-imagegroup-command.md) 
 Get-ImageGroup [Usar el comando](using-the-remove-imagegroup-command.md) 
 Remove-ImageGroup [Subcomando: set-ImageGroup](subcommand-set-imagegroup.md)
