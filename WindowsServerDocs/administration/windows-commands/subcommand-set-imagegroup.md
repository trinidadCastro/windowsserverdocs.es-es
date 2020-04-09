---
title: Subcomando set-ImageGroup
description: Tema de comandos de Windows para el subcomando set-ImageGroup, que cambia los atributos de un grupo de imágenes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 966e3c10d9bcc13568f5fec4e1efd1ad46a121b0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833918"
---
# <a name="subcommand-set-imagegroup"></a>Subcomando: set-ImageGroup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|[/Name:<New image group name>]|Especifica el nuevo nombre del grupo de imágenes.|
|[/Security:<SDDL>]|Especifica el nuevo descriptor de seguridad del grupo de imágenes, en formato de lenguaje de definición de descriptores de seguridad (SDDL).|
## <a name="examples"></a><a name=BKMK_examples></a>Example
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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando add-ImageGroup](using-the-add-imagegroup-command.md)
[usar el comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)
mediante el comando [Get-ImageGroup](using-the-get-imagegroup-command.md)
[mediante el comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
