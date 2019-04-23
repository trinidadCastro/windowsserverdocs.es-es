---
title: Con el comando remove-DriverGroupPackages
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b499635-6285-491c-8854-5665489f4364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c00251dd07fca2491fedf6aed0c6989ee65a0dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866016"
---
# <a name="using-the-remove-drivergrouppackages-command"></a>Con el comando remove-DriverGroupPackages

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita los paquetes de controladores de un grupo de controladores en un servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /remove-DriverGroupPackages /DriverGroup:<Group Name> [/Server:<Server Name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/DriverGroup:<Group Name>|Especifica el nombre del grupo de controladores.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se usa el servidor local.|
|/Filtertype:<Filter type>|Especifica el atributo del paquete de controladores que se buscará. Puede especificar varios atributos en un solo comando. También debe especificar **/Operator** y **/valor** con esta opción.<br /><br /><Filter type> Puede ser uno de los siguientes:<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/ Operador: {igual &#124; NotEqual &#124; GreaterOrEqual &#124; LessOrEqual &#124; contiene}|Especifica la relación entre el atributo y los valores. Solo se puede especificar **Contains** con atributos de cadena. Solo se puede especificar **GreaterOrEqual** y **LessOrEqual** con atributos de fecha y la versión.|
|/ Valor:<Value>|Especifica el valor de búsqueda especificado <attribute>. Puede especificar varios valores para un único **/Filtertype**. En la lista siguiente se describe los atributos que se pueden especificar para cada filtro. Para obtener más información acerca de estos atributos, vea [atributos del controlador y el paquete](https://go.microsoft.com/fwlink/?LinkId=166895) (https://go.microsoft.com/fwlink/?LinkId=166895).<br /><br />-PackageId - especifique un GUID válido. Por ejemplo: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-Especifique PackageName cualquier valor de cadena.<br />-Especifique PackageEnabled - **Sí** o **No**.<br />-Packagedateadded - especificar la fecha en el formato siguiente: AAAA/MM/DD<br />-PackageInfFilename especificar cualquier valor de cadena.<br />-PackageClass - especifique un nombre de clase válido o el GUID de clase. Por ejemplo: **Unidad de disco**, **Net**, o {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageProvider especificar cualquier valor de cadena.<br />-Especifique PackageArchitecture - **x86**, **x64**, o **ia64**.<br />-PckageLocale - especificar un identificador de idioma válido. Por ejemplo: **en-US** o **es-es**.<br />-Especifique PackageSigned - **Sí** o **No**.<br />-PackagedatePublished - especificar la fecha en el formato siguiente: AAAA/MM/DD<br />-Packageversion - especificar la versión en el siguiente formato: a.b.x.y. Por ejemplo: 6.1.0.0<br />-Driverdescription especificar cualquier valor de cadena.<br />-DriverManufacturer especificar cualquier valor de cadena.<br />-DriverHardwareId - especificar cualquier valor de cadena.<br />-DrivercompatibleId - especificar cualquier valor de cadena.<br />-DriverExcludeId - especificar cualquier valor de cadena.<br />-DriverGroupId - especifique un GUID válido. Por ejemplo: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-DriverGroupName especificar cualquier valor de cadena.|
## <a name="BKMK_examples"></a>Ejemplos
Para quitar paquetes de controladores de un grupo de controladores, escriba uno de los siguientes:
```
wdsutil /verbose /remove-DriverGroupPackages /DriverGroup:printerdrivers
/Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2
```
```
wdsutil /verbose /remove-DriverGroupPackages /DriverGroup:DisplayDrivers
/Filtertype:PackageArchitecture /Operator:Equal /Value:x86
/Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando remove-DriverGroupPackage](using-the-remove-drivergrouppackage-command.md)
