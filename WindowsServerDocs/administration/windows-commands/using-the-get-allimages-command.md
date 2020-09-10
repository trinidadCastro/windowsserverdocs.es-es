---
title: Get-AllImages
description: Artículo de referencia de Get-AllImages, que recupera información acerca de todas las imágenes de un servidor.
ms.topic: reference
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 79b28d3649e18b192504bf6eb012984cb43218e2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626388"
---
# <a name="get-allimages"></a>Get-AllImages

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera información acerca de todas las imágenes de un servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/Show: {boot &#124; instalar &#124; LegacyRis &#124; All}|-   El **arranque** solo devuelve imágenes de arranque.<br />-   **Install** devuelve las imágenes de instalación, así como información sobre los grupos de imágenes que las contienen.<br />-   **LegacyRis** solo devuelve imágenes de servicios de instalación remota (RIS).<br />-   **All** devuelve la información de la imagen de arranque, la información de imagen de instalación (incluida la información acerca de los grupos de imágenes) y la información de imagen de RIS.|
|[/detailed]|Indica que se deben devolver todos los metadatos de imagen de cada imagen. Si no se usa esta opción, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo.|
## <a name="examples"></a>Ejemplos
Para ver información acerca de las imágenes, escriba una de las siguientes opciones:
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando](using-the-add-image-command.md) 
 Add-image [Usar el comando](using-the-copy-image-command.md) 
 Copy-Image [Usar el comando](using-the-export-image-command.md) 
 Export-Image [Usar el comando](using-the-remove-image-command.md) 
 Remove-image [Usar el comando](using-the-replace-image-command.md) 
 Replace-Image [Subcomando: set-Image](subcommand-set-image.md)
