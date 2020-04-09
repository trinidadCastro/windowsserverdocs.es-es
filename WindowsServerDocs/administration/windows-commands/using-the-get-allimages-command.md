---
title: Get-AllImages
description: Temas de comandos de Windows para Get-AllImages, que recupera información acerca de todas las imágenes de un servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1358d7ae4a86b6439b9a304e10e3aa569112d5a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831308"
---
# <a name="get-allimages"></a>Get-AllImages

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información acerca de todas las imágenes de un servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
|/Show: {boot &#124; install &#124; LegacyRis &#124; All}|-   **arranque** solo devuelve imágenes de arranque.<br />-   **instalación** devuelve imágenes de instalación, así como información sobre los grupos de imágenes que las contienen.<br />-   **LegacyRis** devuelve solo imágenes de servicios de instalación remota (RIS).<br />-   **All** devuelve la información de la imagen de arranque, la información de imagen de instalación (incluida la información acerca de los grupos de imágenes) y la información de imagen de RIS.|
|[/detailed]|Indica que se deben devolver todos los metadatos de imagen de cada imagen. Si no se usa esta opción, el comportamiento predeterminado es devolver solo el nombre de la imagen, la descripción y el nombre de archivo.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver información acerca de las imágenes, escriba una de las siguientes opciones:
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Referencias adicionales
- La [clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[usar el comando add-Image](using-the-add-image-command.md)
[con el comando copy-](using-the-copy-image-command.md) Image
mediante el comando [Export-Image](using-the-export-image-command.md)
[mediante el comando Remove-image](using-the-remove-image-command.md)
[con el comando Replace-Image](using-the-replace-image-command.md)
[Subcommand: set-Image](subcommand-set-image.md)
