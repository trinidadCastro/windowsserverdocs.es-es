---
title: WDSUtil Add-imagedriverpackages
description: Artículo de referencia del comando WDSUtil Add-imagedriverpackages, que agrega paquetes de controladores desde el almacén de controladores a una imagen de arranque.
ms.topic: reference
ms.assetid: 9dc78909-a4d1-42a2-af8f-21ebcbfe8302
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 74029ca7b2243603c832b74f59a9248353cea911
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524480"
---
# <a name="wdsutil-add-imagedriverpackages"></a>WDSUtil Add-imagedriverpackages

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega paquetes de controladores del almacén de controladores a una imagen de arranque.

## <a name="syntax"></a>Sintaxis

```
wdsutil /add-ImageDriverPackages [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| [/Server:`<Servername>`] | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica un nombre de servidor, se utiliza el servidor local. |
| [medios: `<Imagename>` ] | Especifica el nombre de la imagen a la que se va a agregar el controlador. |
| [mediatype: boot] | Especifica el tipo de imagen a la que se va a agregar el controlador. Los paquetes de controladores solo se pueden agregar a imágenes de arranque. |
| [/Architecture: `{x86 | ia64 | x64}` ] | Especifica la arquitectura de la imagen de arranque. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en diferentes arquitecturas, debe especificar la arquitectura para asegurarse de que se usa la imagen correcta. |
| [/Filename:`<Filename>`] | Especifica el nombre del archivo. Si la imagen no se puede identificar de forma única por nombre, se debe especificar el nombre de archivo. |
| FilterType`<Filtertype>` | Especifica el atributo del paquete de controladores que se va a buscar. Puede especificar varios atributos en un solo comando. También debe especificar **/Operator** y **/Value** con esta opción. Los valores válidos son:<ul><li>PackageId</li><li>PackageName</li><li>PackageEnabled</li><li>Packagedateadded</li><li>PackageInfFilename</li><li>PackageClass<p>PackageProvider</li><li>PackageArchitecture</li><li>PackageLocale</li><li>PackageSigned</li><li>PackagedatePublished</li><li>Packageversion</li><li>Driverdescription</li><li>DriverManufacturer</li><li>DriverHardwareId</li><li>DrivercompatibleId</li><li>DriverExcludeId</li><li>DriverGroupId</li><li>DriverGroupName**</li></ul>. |
| Operator`{Equal|NotEqual|GreaterOrEqual|LessOrEqual|Contains}` | Especifica la relación entre el atributo y los valores. Solo se puede especificar **Contains** con atributos de cadena. Solo puede especificar **GreaterOrEqual** y **LessOrEqual** con atributos de fecha y versión. |
| Valor`<Value>` | Especifica el valor que se va a buscar en relación con el especificado `<attribute>` . Puede especificar varios valores para un único **/FilterType**. Los valores disponibles para cada filtro son:<ul><li>**PackageId** : especifique un GUID válido. Por ejemplo: {4d36e972-E325-11ce-bfc1-08002be10318}</li><li>**Packagename** -especifique cualquier valor de cadena</li><li>**PackageEnabled** : especifique **sí** o **no** .</li><li>**Packagedateadded** : especifique la fecha en el siguiente formato: AAAA/MM/DD</li><li>**PackageInfFilename** : especifique cualquier valor de cadena.</li><li>**PackageClass** : especifique un nombre de clase o un GUID de clase válidos. Por ejemplo: **DiskDrive**, **net**o {4d36e972-E325-11ce-bfc1-08002be10318}</li><li>**PackageProvider** : especifique cualquier valor de cadena.</li><li>**PackageArchitecture** : especificar **x86**, **x64**o **ia64**</li><li>**PackageLocale** : especifique un identificador de idioma válido. Por ejemplo: **en-US** o **es-es**</li><li>**PackageSigned** : especifique **sí** o **no** .</li><li>**PackagedatePublished** : especifique la fecha en el siguiente formato: AAAA/MM/DD</li><li>**Packageversion** : especifique la versión con el siguiente formato: a.b.x.y. Por ejemplo: 6.1.0.0</li><li>**Driverdescription** : especifique cualquier valor de cadena.</li><li>**DriverManufacturer** : especifique cualquier valor de cadena.</li><li>**DriverHardwareId** : especifique cualquier valor de cadena.</li><li>**DrivercompatibleId** : especifique cualquier valor de cadena.</li><li>**DriverExcludeId** : especifique cualquier valor de cadena.</li><li>**DriverGroupId** : especifique un GUID válido. Por ejemplo: {4d36e972-E325-11ce-bfc1-08002be10318}</li><li>**DriverGroupName** : especifique cualquier valor de cadena.</li></ul> Para obtener más información sobre estos valores, vea [controladores y atributos de paquete](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759262(v=ws.11)). |

## <a name="examples"></a>Ejemplos

Para agregar paquetes de controladores a una imagen de arranque, escriba uno de los siguientes:

```
wdsutil /add-ImageDriverPackagemedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /Filtertype:DriverGroupName /Operator:Equal /Value:x86Bus /Filtertype:PackageProvider /Operator:Contains /Value:Provider1 /Filtertype:Packageversion /Operator:GreaterOrEqual /Value:6.1.0.0
```

```
wdsutil /verbose /add-ImageDriverPackagemedia: WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```

```
wdsutil /verbose /add-ImageDriverPackagemedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Value:System /Value:DiskDrive /Value:HDC /Value:SCSIAdapter
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-
- [WDSUtil Add-imagedriverpackage (comando)](wdsutil-add-imagedriverpackage.md)
-
- [WDSUtil Add-alldriverpackages (comando)](wdsutil-add-alldriverpackages.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
