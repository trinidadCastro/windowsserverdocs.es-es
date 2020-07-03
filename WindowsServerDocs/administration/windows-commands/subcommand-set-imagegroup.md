---
title: Subcomando set-ImageGroup
description: Artículo de referencia para el subcomando set-ImageGroup, que cambia los atributos de un grupo de imágenes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f25acaddb08f829054ad9270ab171ab04d6ee156
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937156"
---
# <a name="subcommand-set-imagegroup"></a>Subcomando: set-ImageGroup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia los atributos de un grupo de imágenes.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
mediaGroup:<Image group name>|Especifica el nombre del grupo de imágenes.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica, se usará el servidor local.|
|[/Name: <New image group name> ]|Especifica el nuevo nombre del grupo de imágenes.|
|[/Security: <SDDL> ]|Especifica el nuevo descriptor de seguridad del grupo de imágenes, en formato de lenguaje de definición de descriptores de seguridad (SDDL).|
## <a name="examples"></a>Ejemplos
Para establecer el nombre de un grupo de imágenes, escriba:
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:New Image Group Name
```
Para especificar varios valores para un grupo de imágenes, escriba:
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:New Image Group Name
/Security:O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Uso del comando](using-the-add-imagegroup-command.md) 
 Add-ImageGroup [Usar el comando](using-the-get-allimagegroups-command.md) 
 Get-AllImageGroups [Usar el comando](using-the-get-imagegroup-command.md) 
 Get-ImageGroup [Usar el comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
