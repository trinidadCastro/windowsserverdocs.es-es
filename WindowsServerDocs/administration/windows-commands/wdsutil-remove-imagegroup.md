---
title: WDSUtil Remove-imagegroup
description: Artículo de referencia de WDSUtil Remove-imagegroup, que quita un grupo de imágenes de un servidor.
ms.topic: reference
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c9691281a706a2929c7406bfcca2c3f3410112e0
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731242"
---
# <a name="wdsutil-remove-imagegroup"></a>WDSUtil Remove-imagegroup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Quita un grupo de imágenes de un servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /remove-ImageGroup Group:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|imagegroup:<Image group name>|Especifica el nombre del grupo de imágenes que se va a quitar.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
## <a name="examples"></a>Ejemplos
Para quitar el grupo de imágenes, escriba uno de los siguientes:
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Add-imagegroup (comando)](wdsutil-add-imagegroup.md)
- [WDSUtil Get-allimagegroups (comando)](wdsutil-get-allimagegroups.md)
- [WDSUtil Get-imagegroup (comando)](wdsutil-get-imagegroup.md)
- [WDSUtil Set-imagegroup (comando)](wdsutil-set-imagegroup.md)
