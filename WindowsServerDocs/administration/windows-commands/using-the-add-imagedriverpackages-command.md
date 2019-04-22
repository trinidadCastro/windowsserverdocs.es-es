---
title: Con el comando add-ImageDriverPackages
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9dc78909-a4d1-42a2-af8f-21ebcbfe8302
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55b26acf9c4006db3d64e27be8a10e158876ddc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817366"
---
# <a name="using-the-add-imagedriverpackages-command"></a>Con el comando add-ImageDriverPackages

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agrega paquetes de controladores del almacén de controladores a una imagen de arranque. La versión de la imagen debe ser Windows 7 o Windows Server 2008 R2 o posterior.
Para obtener ejemplos de cómo se puede utilizar este comando, consulte [ejemplos](#BKMK_examples).
## <a name="syntax"></a>Sintaxis
```
wdsutil /add-ImageDriverPackages [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
[/Filename:<File name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/ Server:<Server name>|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se usa el servidor local.|
Medio:<Image name>|Especifica el nombre de la imagen para agregar el controlador.|
mediatype:Boot|Especifica el tipo de imagen para agregar el controlador. Paquetes de controladores pueden agregarse únicamente a las imágenes de arranque.|
|/ Arquitectura: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen de arranque. Dado que es posible tener el mismo nombre de imagen para las imágenes de arranque en arquitecturas diferentes, debe especificar la arquitectura para asegurarse de que se usa la imagen correcta.|
|/Filename:<File name>|Especifica el nombre de archivo. Si la imagen no se identifica por nombre, debe especificarse el nombre de archivo.|
|/Filtertype:<Filter type>|Especifica el atributo del paquete de controladores que se buscará. Puede especificar varios atributos en un solo comando. También debe especificar **/Operator** y **/valor** con esta opción.<br /><br /><Filter type> Puede ser uno de los siguientes:<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/ Operador: {igual &#124; NotEqual &#124; GreaterOrEqual &#124; LessOrEqual &#124; contiene}|La relación entre el atributo y los valores. Solo se puede especificar **Contains** con atributos de cadena. Solo se puede especificar **GreaterOrEqual** y **LessOrEqual** con atributos de fecha y la versión.|
|/ Valor:<Value>|Especifica el valor de búsqueda en relación con el especificado <attribute>. Puede especificar varios valores para un único **/Filtertype**. La lista siguiente se describen los atributos que se pueden especificar para cada filtro. Para obtener más información acerca de estos atributos, vea [atributos del controlador y el paquete](https://go.microsoft.com/fwlink/?LinkId=166895) (https://go.microsoft.com/fwlink/?LinkId=166895).<br /><br />-PackageId - especifique un GUID válido. Por ejemplo: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-Especifique PackageName cualquier valor de cadena.<br />-Especifique PackageEnabled - **Sí** o **No**.<br />-Packagedateadded - especificar la fecha en el formato siguiente: AAAA/MM/DD<br />-PackageInfFilename especificar cualquier valor de cadena.<br />-PackageClass - especifique un nombre de clase válido o el GUID de clase. Por ejemplo: **Unidad de disco**, **Net**, o {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageProvider especificar cualquier valor de cadena.<br />-Especifique PackageArchitecture - **x86**, **x64**, o **ia64**.<b /> -PackageLocale - especificar un identificador de idioma válido. Por ejemplo: **en-US** o **es-es**.<br />-Especifique PackageSigned - **Sí** o **No**.<br />-PackagedatePublished - especificar la fecha en el formato siguiente: AAAA/MM/DD<br />-Packageversion - especificar la versión en el siguiente formato: a.b.x.y. Por ejemplo: 6.1.0.0<br />-Driverdescription especificar cualquier valor de cadena.<br />-DriverManufacturer especificar cualquier valor de cadena.<br />-DriverHardwareId - especificar cualquier valor de cadena.<br />-DrivercompatibleId - especificar cualquier valor de cadena.<br />-DriverExcludeId - especificar cualquier valor de cadena.<br />-DriverGroupId - especifique un GUID válido. Por ejemplo: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-DriverGroupName especificar cualquier valor de cadena.|
## <a name="BKMK_examples"></a>Ejemplos
Para agregar paquetes de controladores a una imagen de arranque, escriba uno de los siguientes:
```
wdsutil /add-ImageDriverPackagemedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /Filtertype:DriverGroupName /Operator:Equal /Value:x86Bus /Filtertype:PackageProvider /Operator:Contains /Value:Provider1 /Filtertype:Packageversion /Operator:GreaterOrEqual /Value:6.1.0.0
```
```
wdsutil /verbose /add-ImageDriverPackagemedia: "WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
```
wdsutil /verbose /add-ImageDriverPackagemedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Value:System /Value:DiskDrive /Value:HDC /Value:SCSIAdapter
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando add-ImageDriverPackage](using-the-add-imagedriverpackage-command.md)
[mediante el subcomando add AllDriverPackages](using-the-add-alldriverpackages-subcommand.md)
