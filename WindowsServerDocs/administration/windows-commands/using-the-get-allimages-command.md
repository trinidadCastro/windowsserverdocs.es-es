---
title: Usar el comando Get-AllImages
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5122a5660031d503795715c0005b404f910d6626
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363496"
---
# <a name="using-the-get-allimages-command"></a>Usar el comando Get-AllImages

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información acerca de todas las imágenes de un servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/Show: {boot &#124; install &#124; LegacyRis &#124; All}|el**arranque** -    solo devuelve imágenes de arranque.<br />la**instalación** de -    devuelve imágenes de instalación, así como información sobre los grupos de imágenes que las contienen.<br />-   **LegacyRis** devuelve solo imágenes de servicios de instalación remota (RIS).<br />@no__t **-0 devuelve** la información de la imagen de arranque, la información de la imagen de instalación (incluida la información acerca de los grupos de imágenes) y la información de la imagen de RIS.|
|[/detailed]|Indica que se deben devolver todos los metadatos de imagen de cada imagen. Si no se usa esta opción, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo.|
## <a name="BKMK_examples"></a>Example
Para ver información acerca de las imágenes, escriba una de las siguientes opciones:
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando add-Image](using-the-add-image-command.md)
[mediante el comando copy-Image](using-the-copy-image-command.md)
[mediante el comando export-Image](using-the-export-image-command.md)
[con el comando Remove-image](using-the-remove-image-command.md)
 mediante el comando[ Comando Replace-Image](using-the-replace-image-command.md)1[subcomando: set-Image](subcommand-set-image.md)
