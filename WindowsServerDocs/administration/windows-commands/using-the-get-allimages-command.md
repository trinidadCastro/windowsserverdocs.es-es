---
title: Mediante el comando get-AllImages
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 57b81dd3dd3a24876c4401e80d08130ed5243888
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872566"
---
# <a name="using-the-get-allimages-command"></a>Mediante el comando get-AllImages

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información sobre todas las imágenes en un servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|/ Show: {arranque &#124; instalar &#124; LegacyRis &#124; todas}|-   **Arranque** devuelve solo las imágenes de arranque.<br />-   **Instalar** devuelve instala imágenes así como información acerca de los grupos de imágenes que los contienen.<br />-   **LegacyRis** devuelve solo las imágenes de servicios de instalación (RIS) remoto.<br />-   **Todos los** devuelve información de la imagen RIS, información de la imagen de instalación (incluida información acerca de los grupos de imagen) e información de la imagen de arranque.|
|[/ detallada]|Indica que se deben devolver todos los metadatos de imagen de cada imagen. Si no se utiliza esta opción, el comportamiento predeterminado es devolver el nombre de la imagen, descripción y nombre de archivo.|
## <a name="BKMK_examples"></a>Ejemplos
Para ver información sobre las imágenes, escriba uno de los siguientes:
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[mediante el comando add-Image](using-the-add-image-command.md)
[mediante el comando de Copiar imagen](using-the-copy-image-command.md)
[mediante el Comando de Export-Image](using-the-export-image-command.md)
[mediante el comando remove-Image](using-the-remove-image-command.md)
[mediante el comando replace-Image](using-the-replace-image-command.md) 
 [Subcomando: Establecer imagen](subcommand-set-image.md)
