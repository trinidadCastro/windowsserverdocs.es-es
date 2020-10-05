---
title: WDSUtil Get-imagegroup
description: Artículo de referencia para WDSUtil Get-imagegroup, que recupera información sobre un grupo de imágenes y las imágenes que hay en él.
ms.topic: reference
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d40fef643ebbb8fd48f36fc048ddefc0b4b3229f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730821"
---
# <a name="wdsutil-get-imagegroup"></a>WDSUtil Get-imagegroup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera información sobre un grupo de imágenes y las imágenes que contiene.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-ImageGroup ImageGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/ImageGroup:<Image group name>|Especifica el nombre del grupo de imágenes.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|[/detailed]|Devuelve los metadatos de la imagen de cada imagen. Si no se usa este parámetro, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo.|
## <a name="examples"></a>Ejemplos
Para ver información acerca de un grupo de imágenes, escriba:
```
wdsutil /Get-ImageGroup ImageGroup:ImageGroup1
```
Para ver información, incluidos los metadatos, escriba:
```
wdsutil /verbose /Get-ImageGroup ImageGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Add-imagegroup (comando)](wdsutil-add-imagegroup.md)
- [WDSUtil Get-allimagegroups (comando)](wdsutil-get-allimagegroups.md)
- [WDSUtil Remove-imagegroup (comando)](wdsutil-remove-imagegroup.md)
- [WDSUtil Set-imagegroup (comando)](wdsutil-set-imagegroup.md)
