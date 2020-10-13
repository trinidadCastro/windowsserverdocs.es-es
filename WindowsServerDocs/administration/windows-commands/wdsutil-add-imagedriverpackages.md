---
title: WDSUtil Add-imagedriverpackages
description: Artículo de referencia de WDSUtil Add-imagedriverpackages, que agrega paquetes de controladores del almacén de controladores a una imagen de arranque.
ms.topic: reference
ms.assetid: 9dc78909-a4d1-42a2-af8f-21ebcbfe8302
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2b8abcf119d43bd0f524f92c29219ad7e6027941
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731062"
---
# <a name="wdsutil-add-imagedriverpackages"></a>WDSUtil Add-imagedriverpackages

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega paquetes de controladores del almacén de controladores a una imagen de arranque. La versión de la imagen debe ser Windows 7 o Windows Server 2008 R2 o posterior.

## <a name="syntax"></a>Sintaxis
```
wdsutil /add-ImageDriverPackages [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64}
[/Filename:<File name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```
### <a name="parameters"></a>Parámetros

|                                         Parámetro                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                   Servidor<Server name>                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|                                     soporte<Image name>                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         Especifica el nombre de la imagen a la que se va a agregar el controlador.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                                       mediatype: boot                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  Especifica el tipo de imagen a la que se va a agregar el controlador. Los paquetes de controladores solo se pueden agregar a imágenes de arranque.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                         /Architecture: {x86 &#124; ia64 &#124; x64}                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         Especifica la arquitectura de la imagen de arranque. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en diferentes arquitecturas, debe especificar la arquitectura para asegurarse de que se utiliza la imagen correcta.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                                   Extensión<File name>                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             especifica el nombre de archivo. Si la imagen no se puede identificar de forma única por nombre, se debe especificar el nombre de archivo.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                                 FilterType<Filter type>                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     Especifica el atributo del paquete de controladores que se va a buscar. Puede especificar varios atributos en un solo comando. También debe especificar **/Operator** y **/Value** con esta opción.<p><Filter type> puede ser uno de los siguientes:<p>**PackageId**<p>**PackageName**<p>**PackageEnabled**<p>**Packagedateadded**<p>**PackageInfFilename**<p>**PackageClass**<p>**PackageProvider**<p>**PackageArchitecture**<p>**PackageLocale**<p>**PackageSigned**<p>**PackagedatePublished**<p>**Packageversion**<p>**Driverdescription**<p>**DriverManufacturer**<p>**DriverHardwareId**<p>**DrivercompatibleId**<p>**DriverExcludeId**<p>**DriverGroupId**<p>**DriverGroupName**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| /Operator: {Equal &#124; NotEqual &#124; GreaterOrEqual &#124; LessOrEqual &#124; contiene} |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             La relación entre el atributo y los valores. Solo se puede especificar **Contains** con atributos de cadena. Solo puede especificar **GreaterOrEqual** y **LessOrEqual** con atributos de fecha y versión.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|                                       Valor<Value>                                       | Especifica el valor que se va a buscar en relación con el especificado <attribute> . Puede especificar varios valores para un único **/FilterType**. En la lista siguiente se describen los atributos que se pueden especificar para cada filtro. Para obtener más información acerca de estos atributos, vea [controladores y atributos de paquete](https://go.microsoft.com/fwlink/?LinkId=166895) ( <https://go.microsoft.com/fwlink/?LinkId=166895> ).<p>-PackageId: especifique un GUID válido. Por ejemplo: {4d36e972-E325-11ce-bfc1-08002be10318}.<br />-PackageName especifique cualquier valor de cadena.<br />-PackageEnabled: especifique **sí** o **no**.<br />-Packagedateadded: especifique la fecha con el formato siguiente: AAAA/MM/DD<br />-PackageInfFilename especifique cualquier valor de cadena.<br />-PackageClass: especifique un nombre de clase o un GUID de clase válidos. Por ejemplo: **DiskDrive**, **net**o {4d36e972-E325-11ce-bfc1-08002be10318}.<br />-PackageProvider especifique cualquier valor de cadena.<br />-PackageArchitecture: especifique **x86**,  **x64**o **ia64**. <b /> -PackageLocale: especifique un identificador de idioma válido. Por ejemplo: **en-US** o **es-es**.<br />-PackageSigned: especifique **sí** o **no**.<br />-PackagedatePublished: especifique la fecha con el formato siguiente: AAAA/MM/DD<br />-Packageversion: especifique la versión con el siguiente formato: a.b.x.y. Por ejemplo: 6.1.0.0<br />-Driverdescription especifique cualquier valor de cadena.<br />-DriverManufacturer especifique cualquier valor de cadena.<br />-DriverHardwareId: especifique cualquier valor de cadena.<br />-DrivercompatibleId: especifique cualquier valor de cadena.<br />-DriverExcludeId: especifique cualquier valor de cadena.<br />-DriverGroupId: especifique un GUID válido. Por ejemplo: {4d36e972-E325-11ce-bfc1-08002be10318}.<br />-DriverGroupName especifique cualquier valor de cadena. |

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
- [WDSUtil Add-imagedriverpackage (comando)](wdsutil-add-imagedriverpackage.md)
- [WDSUtil Add-alldriverpackages (comando)](wdsutil-add-alldriverpackages.md)