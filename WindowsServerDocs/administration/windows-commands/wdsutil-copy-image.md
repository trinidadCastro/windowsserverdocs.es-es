---
title: WDSUtil-copiar imagen
description: Artículo de referencia del comando WDSUtil Copy-Image, que copia las imágenes que se encuentran dentro del mismo grupo de imágenes.
ms.topic: reference
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 690040a5f6624e33e01969a77806afb01c815ea9
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524910"
---
# <a name="wdsutil-copy-image"></a>WDSUtil-copiar imagen

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia las imágenes que se encuentran dentro del mismo grupo de imágenes. Para copiar imágenes entre grupos de imágenes, use el comando de [comando wdsutil Export-Image](wdsutil-export-image.md) y, a continuación, el comando de [comando Add-image de WDSUtil](wdsutil-add-image.md) .

## <a name="syntax"></a>Sintaxis

```
wdsutil [Options] /copy-Image image:<Image name> [/Server:<Server name>] imagetype:Install imageGroup:<Image group name>] [/Filename:<File name>] /DestinationImage /Name:<Name> /Filename:<File name> [/Description:<Description>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| impresión`<Imagename>` | Especifica el nombre de la imagen que se va a copiar. |
| [/Server:`<Servername>`] | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
| ImageType: instalación | Especifica el tipo de imagen que se va a copiar. Esta opción debe estar configurada para **instalar**. |
| \imageGroup: `<Image groupname>` ] | Especifica el grupo de imágenes que contiene la imagen que se va a copiar. Si no se especifica ningún grupo de imágenes y solo existe un grupo en el servidor, se usa el grupo de imágenes de forma predeterminada. Si existen varios grupos de imágenes en el servidor, debe especificar el grupo de imágenes. |
| [/Filename:`<Filename>`] | Especifica el nombre de archivo de la imagen que se va a copiar. Si la imagen de origen no se puede identificar de forma única por nombre, debe especificar el nombre de archivo. |
| /DestinationImage | Especifica la configuración de la imagen de destino. Los valores válidos son:<ul><li>/Name: `<Name>` -establece el nombre para mostrar de la imagen que se va a copiar.</li><li>/Filename: `<Filename>` -establece el nombre del archivo de imagen de destino que contendrá la copia de la imagen.</li><li>[/Description: `<Description>` ] : Establece la descripción de la copia de la imagen.</li></ul> |

## <a name="examples"></a>Ejemplos

Para crear una copia de la imagen especificada y asignarle el nombre WindowsVista. Wim, escriba:

```
wdsutil /copy-Image image:Windows Vista with Office imagetype:Install /DestinationImage /Name:copy of Windows Vista with Office / Filename:WindowsVista.wim
```

Para crear una copia de la imagen especificada, aplique la configuración especificada y asigne a la copia el nombre WindowsVista. Wim, escriba:

```
wdsutil /verbose /Progress /copy-Image image:Windows Vista with Office /Server:MyWDSServe imagetype:Install imageGroup:ImageGroup1
/Filename:install.wim /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim /Description:This is a copy of the original Windows image with Office installed
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil-comando Add-image](wdsutil-add-image.md)

- [comando WDSUtil Export-Image](wdsutil-export-image.md)

- [WDSUtil comando Get-Image](wdsutil-get-image.md)

- [comando WDSUtil Remove-image](wdsutil-remove-image.md)

- [WDSUtil-comando Replace-Image](wdsutil-replace-image.md)

- [WDSUtil Set-Image (comando)](wdsutil-set-image.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
