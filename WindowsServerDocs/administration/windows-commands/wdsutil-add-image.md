---
title: WDSUtil-agregar imagen
description: Artículo de referencia del comando WDSUtil Add-image, que agrega imágenes a un servidor de servicios de implementación de Windows.
ms.topic: reference
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e0d80c0a3aa9cfc6a5b9e05f11542dfcc8621946
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524529"
---
# <a name="wdsutil-add-image"></a>WDSUtil-agregar imagen

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega imágenes a un servidor de servicios de implementación de Windows.

## <a name="syntax"></a>Sintaxis

En el caso de las imágenes de arranque, use la siguiente sintaxis:

```
wdsutil /add-Image imageFile:<wim file path> [/Server:<Server name> imagetype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>] [/Filename:<New wim file name>]
```

En el caso de las imágenes de instalación, use la siguiente sintaxis:

```
wdsutil /add-Image imageFile:<wim filepath> [/Server:<Servername>] imagetype:Install [/Skipverify] imageGroup:<Image group name>] [/SingleImage:<Single image name>] [/Name:<Name>] [/Description:<Description>] [/Filename:<File name>] [/UnattendFile:<Unattend file path>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| ImageFile`<.wim filepath>` | Especifica la ruta de acceso completa y el nombre de archivo del archivo de imagen de Windows (. wim) que contiene las imágenes que se van a agregar. |
| [/Server:`<Servername>`] | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica un nombre de servidor, se utiliza el servidor local. |
| ImageType`{Boot|Install}` | Especifica el tipo de imágenes que se van a agregar. |
| [/Skipverify] | Especifica que no se realizará la comprobación de integridad en el archivo de imagen de origen antes de que se agregue la imagen. |
| [/Name: `<Name>` ] | Establece el nombre para mostrar de la imagen. |
| [/Description: `<Description>` ] | Establece la descripción de la imagen. |
| [/Filename:`<Filename>`] | Especifica el nuevo nombre de archivo para el archivo. Wim. Esto le permite cambiar el nombre de archivo del archivo. Wim al agregar la imagen. Si no especifica un nombre de archivo, se usa el nombre de archivo de la imagen de origen. En todos los casos, servicios de implementación de Windows comprueba si el nombre de archivo es único en el almacén de imágenes de arranque del equipo de destino. |
| \imageGroup: `<Imagegroupname>` ] | Especifica el nombre del grupo de imágenes en el que se van a agregar las imágenes. Si existe más de un grupo de imágenes en el servidor, se debe especificar el grupo de imágenes. Si no especifica el grupo de imágenes y ya no existe un grupo de imágenes, se creará un nuevo grupo de imágenes. De lo contrario, se usa el grupo de imágenes existente. |
| [/SingleImage: `<Singleimagename>` ] [/Name: `<Name>` ] [/Description: `<Description>` ] | Copia la imagen única especificada de un archivo. Wim y establece el nombre para mostrar y la descripción de la imagen. |
| [/UnattendFile:`<Unattendfilepath>`] | Especifica la ruta de acceso completa al archivo de instalación desatendida que se va a asociar a las imágenes que se van a agregar. Si no se especifica **/SingleImage** , el mismo archivo de instalación desatendida está asociado a todas las imágenes del archivo. Wim. |

## <a name="examples"></a>Ejemplos

Para agregar una imagen de arranque, escriba:

```
wdsutil /add-Image imageFile:C:\MyFolder\Boot.wim imagetype:Boot
wdsutil /verbose /Progress /add-Image imageFile:\\MyServer\Share\Boot.wim /Server:MyWDSServer imagetype:Boot /Name:My WinPE Image /Description:WinPE Image containing the WDS Client /Filename:WDSBoot.wim
```

Para agregar una imagen de instalación, escriba una de las siguientes opciones:

```
wdsutil /add-Image imageFile:C:\MyFolder\Install.wim imagetype:Install
wdsutil /verbose /Progress /add-Image imageFile:\\MyServer\Share \Install.wim /Server:MyWDSServer imagetype:Instal imageGroup:ImageGroup1
/SingleImage:Windows Pro /Name:My WDS Image /Description:Windows Pro image with Microsoft Office /Filename:Win Pro.wim /UnattendFile:\\server\share\unattend.xml
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de copia de imagen de WDSUtil](wdsutil-copy-image.md)

- [comando WDSUtil Export-Image](wdsutil-export-image.md)

- [WDSUtil comando Get-Image](wdsutil-get-image.md)

- [comando WDSUtil Remove-image](wdsutil-remove-image.md)

- [WDSUtil-comando Replace-Image](wdsutil-replace-image.md)

- [WDSUtil Set-Image (comando)](wdsutil-set-image.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
