---
title: WDSUtil Export-Image
description: Artículo de referencia del comando WDSUtil Export-Image, que exporta una imagen existente del almacén de imágenes a otro archivo de imagen de Windows (. wim).
ms.topic: reference
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8c3c390e43cc8707d8d94ade757130276b5f2753
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071047"
---
# <a name="wdsutil-export-image"></a>WDSUtil Export-Image

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Exporta una imagen existente del almacén de imágenes a otro archivo de imagen de Windows (. wim).

## <a name="syntax"></a>Syntax

Para imágenes de arranque:

```
wdsutil [options] /Export-Image image:<Image name> [/Server:<Servername>]
    imagetype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
     /DestinationImage
         /Filepath:<Filepath and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No}]
```

Para imágenes de instalación:

```
wdsutil [options] /Export-Image image:<Image name> [/Server:<Servername>]
    imagetype:Install imageGroup:<Image group name>]
     [/Filename:<Filename>]
     /DestinationImage
         /Filepath:<Filepath and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No | append}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| impresión`<Imagename>` | Especifica el nombre de la imagen que se va a exportar. |
| [/Server:`<Servername>`] | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local. |
| ImageType`{Boot|Install}` | Especifica el tipo de imagen que se va a exportar. |
| \imageGroup: `<Image group name>` ] | Especifica el grupo de imágenes que contiene la imagen que se va a exportar. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utilizará ese grupo de imágenes de forma predeterminada. Si existe más de un grupo de imágenes en el servidor, se debe especificar el grupo de imágenes. |
| Arquitectura`{x86|ia64|x64}` | Especifica la arquitectura de la imagen que se va a exportar. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en distintas arquitecturas, la especificación del valor de arquitectura garantiza que se devolverá la imagen correcta. |
| [/Filename:`<Filename>`] | Si la imagen no se puede identificar de forma única por nombre, se debe especificar el nombre de archivo. |
| /DestinationImage | Especifica la configuración de la imagen de destino. Puede especificar esta configuración con las siguientes opciones:<ul><li>`/Filepath:<Filepath and name>` : Especifica la ruta de acceso completa del archivo para la nueva imagen.</li><li>`[/Name:<Name>]` : Establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre, se usará el nombre para mostrar de la imagen de origen.</li><li>`[/Description: <Description>]` : Establece la descripción de la imagen.</li></ul> |
| [/Overwrite: `{Yes|No|append}` ] | Determina si el archivo especificado en la opción **/DestinationImage** se sobrescribirá si ya existe un archivo con ese nombre en el/FilePath. La opción **sí** hace que se sobrescriba el archivo existente, la opción **no** (valor predeterminado) hace que se produzca un error si ya existe un archivo con el mismo nombre, y la opción **anexar** hace que la imagen generada se anexe como una nueva imagen dentro del archivo. Wim existente. |

## <a name="examples"></a>Ejemplos

Para exportar una imagen de arranque, escriba:

```
wdsutil /Export-Image image:WinPE boot image imagetype:Boot /Architecture:x86 /DestinationImage /Filepath:C:\temp\boot.wim
```

```
wdsutil /verbose /Progress /Export-Image image:WinPE boot image /Server:MyWDSServer imagetype:Boot /Architecture:x64 /Filename:boot.wim /DestinationImage /Filepath:\\Server\Share\ExportImage.wim /Name:Exported WinPE image /Description:WinPE Image from WDS server /Overwrite:Yes
```

Para exportar una imagen de instalación, escriba:

```
wdsutil /Export-Image image:Windows Vista with Office imagetype:Install /DestinationImage /Filepath:C:\Temp\Install.wim
```

```
wdsutil /verbose /Progress /Export-Image image:Windows Vista with Office /Server:MyWDSServer imagetype:Instal imageGroup:ImageGroup1 /Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:Exported Windows image /Description:Windows Vista image from WDS server /Overwrite:append
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil-comando Add-image](wdsutil-add-image.md)

- [comando de copia de imagen de WDSUtil](wdsutil-copy-image.md)

- [WDSUtil comando Get-Image](wdsutil-get-image.md)

- [comando WDSUtil Remove-image](wdsutil-remove-image.md)

- [WDSUtil-comando Replace-Image](wdsutil-replace-image.md)

- [WDSUtil Set-Image (comando)](wdsutil-set-image.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)