---
title: Set-ImageGroup subcomando
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0c7ba47148ba6f8295ab720dd0118759ac9346c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822986"
---
# <a name="subcommand-set-imagegroup"></a>Subcomando: set-ImageGroup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia los atributos de un grupo de imágenes.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
mediaGroup:<Image group name>|Especifica el nombre del grupo de imágenes.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica, se usará el servidor local.|
|[/ Name:<New image group name>]|Especifica el nuevo nombre del grupo de imágenes.|
|[/ Seguridad:<SDDL>]|Especifica el nuevo Descriptor de seguridad del grupo de imágenes, en formato de lenguaje (SDDL) de definición de descriptor de seguridad.|
## <a name="BKMK_examples"></a>Ejemplos
Para establecer el nombre de un grupo de imágenes, escriba:
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:"New Image Group Name"
```
Para especificar varios valores para un grupo de imágenes, escriba:
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:"New Image Group Name" 
/Security:"O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)"
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando add-ImageGroup](using-the-add-imagegroup-command.md)
[con el comando get-AllImageGroups](using-the-get-allimagegroups-command.md) 
 [ Mediante el comando get-ImageGroup](using-the-get-imagegroup-command.md)
[con el comando remove-ImageGroup](using-the-remove-imagegroup-command.md)
