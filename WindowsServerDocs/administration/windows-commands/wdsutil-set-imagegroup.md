---
title: WDSUtil Set-imagegroup
description: Artículo de referencia para el subcomando set-ImageGroup, que cambia los atributos de un grupo de imágenes.
ms.topic: reference
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6f3b2b3790ecc126f8be48ade61d305d44011f68
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731322"
---
# <a name="wdsutil-set-imagegroup"></a>WDSUtil Set-imagegroup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia los atributos de un grupo de imágenes.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /set-imagegroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/set-imagegroup:<Image group name>|Especifica el nombre del grupo de imágenes.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica, se usará el servidor local.|
|[/Name: <New image group name> ]|Especifica el nuevo nombre del grupo de imágenes.|
|[/Security: <SDDL> ]|Especifica el nuevo descriptor de seguridad del grupo de imágenes, en formato de lenguaje de definición de descriptores de seguridad (SDDL).|
## <a name="examples"></a>Ejemplos
Para establecer el nombre de un grupo de imágenes, escriba:
```
wdsutil /Set-ImageGroup:ImageGroup1 /Name:New Image Group Name
```
Para especificar varios valores para un grupo de imágenes, escriba:
```
wdsutil /verbose /Set-ImageGroupGroup:ImageGroup1 /Server:MyWDSServer /Name:New Image Group Name
/Security:O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Add-imagegroup (comando)](wdsutil-add-imagegroup.md)
- [WDSUtil Get-allimagegroups (comando)](wdsutil-get-allimagegroups.md)
- [WDSUtil Get-imagegroup (comando)](wdsutil-get-imagegroup.md)
- [WDSUtil Remove-imagegroup (comando)](wdsutil-remove-imagegroup.md)
