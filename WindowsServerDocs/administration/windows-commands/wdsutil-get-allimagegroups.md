---
title: WDSUtil Get-allimagegroups
description: Artículo de referencia para WDSUtil Get-allimagegroups, que recupera información sobre todos los grupos de imágenes de un servidor y todas las imágenes de esos grupos de imágenes.
ms.topic: reference
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 894c9ede958b4583c03d4b0e3b5ac4cca1f39395
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730894"
---
# <a name="wdsutil-get-allimagegroups"></a>WDSUtil Get-allimagegroups

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera información sobre todos los grupos de imágenes de un servidor y todas las imágenes de esos grupos de imágenes.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|[/detailed]|Devuelve los metadatos de la imagen de cada imagen. Si no se utiliza este parámetro, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo de cada imagen.|
## <a name="examples"></a>Ejemplos
Para ver información acerca de los grupos de imágenes, escriba uno de los siguientes:
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Add-imagegroup (comando)](wdsutil-add-imagegroup.md)
- [WDSUtil Get-imagegroup (comando)](wdsutil-get-imagegroup.md)
- [WDSUtil Remove-imagegroup (comando)](wdsutil-remove-imagegroup.md)
- [WDSUtil Set-imagegroup (comando)](wdsutil-set-imagegroup.md)
