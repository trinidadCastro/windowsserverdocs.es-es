---
title: WDSUtil Add-drivergrouppackages
description: Artículo de referencia del comando WDSUtil Add-drivergrouppackages, que agrega paquetes de grupos de controladores.
ms.topic: reference
ms.assetid: 29022f53-ce14-4b2d-a81a-679c18e022b2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 67e211207f2c715053d734d659d511c61337e393
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524540"
---
# <a name="wdsutil-add-drivergrouppackages"></a>WDSUtil Add-drivergrouppackages

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega paquetes de grupos de controladores.

## <a name="syntax"></a>Sintaxis

```
wdsutil /add-DriverGroupPackages /DriverGroup:<Group Name> [/Server:<Server Name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /DriverGroup:`<Groupname>` | Especifica el nombre del nuevo grupo de controladores. |
| Servidor`<Servername>` | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
| FilterType`<Filtertype>` | Especifica el tipo de paquete de controladores que se va a buscar. Puede especificar varios atributos en un solo comando. También debe especificar **/Operator** y **/Value** con esta opción. Los valores válidos son:<ul><li>PackageId</li><li>PackageName</li><li>PackageEnabled</li><li>Packagedateadded</li><li>PackageInfFilename</li><li>PackageClass<p>PackageProvider</li><li>PackageArchitecture</li><li>PackageLocale</li><li>PackageSigned</li><li>PackagedatePublished</li><li>Packageversion</li><li>Driverdescription</li><li>DriverManufacturer</li><li>DriverHardwareId</li><li>DrivercompatibleId</li><li>DriverExcludeId</li><li>DriverGroupId</li><li>DriverGroupName**</li></ul>. |
| Operator`{Equal|NotEqual|GreaterOrEqual|LessOrEqual|Contains}` | Especifica la relación entre el atributo y los valores. Solo se puede especificar **Contains** con atributos de cadena. Solo puede especificar **Equal**, **NotEqual**, **GreaterOrEqual** y **LessOrEqual** con los atributos Date y version. |
| Valor`<Value>` | Especifica el valor de cliente correspondiente a **/FilterType**. Puede especificar varios valores para un único **/FilterType**. Los valores disponibles para cada filtro son:<ul><li>**PackageId** : especifique un GUID válido. Por ejemplo: {4d36e972-E325-11ce-bfc1-08002be10318}</li><li>**Packagename** -especifique cualquier valor de cadena</li><li>**PackageEnabled** : especifique **sí** o **no** .</li><li>**Packagedateadded** : especifique la fecha en el siguiente formato: AAAA/MM/DD</li><li>**PackageInfFilename** : especifique cualquier valor de cadena.</li><li>**PackageClass** : especifique un nombre de clase o un GUID de clase válidos. Por ejemplo: **DiskDrive**, **net**o {4d36e972-E325-11ce-bfc1-08002be10318}</li><li>**PackageProvider** : especifique cualquier valor de cadena.</li><li>**PackageArchitecture** : especificar **x86**, **x64**o **ia64**</li><li>**PackageLocale** : especifique un identificador de idioma válido. Por ejemplo: **en-US** o **es-es**</li><li>**PackageSigned** : especifique **sí** o **no** .</li><li>**PackagedatePublished** : especifique la fecha en el siguiente formato: AAAA/MM/DD</li><li>**Packageversion** : especifique la versión con el siguiente formato: a.b.x.y. Por ejemplo: 6.1.0.0</li><li>**Driverdescription** : especifique cualquier valor de cadena.</li><li>**DriverManufacturer** : especifique cualquier valor de cadena.</li><li>**DriverHardwareId** : especifique cualquier valor de cadena.</li><li>**DrivercompatibleId** : especifique cualquier valor de cadena.</li><li>**DriverExcludeId** : especifique cualquier valor de cadena.</li><li>**DriverGroupId** : especifique un GUID válido. Por ejemplo: {4d36e972-E325-11ce-bfc1-08002be10318}</li><li>**DriverGroupName** : especifique cualquier valor de cadena.</li></ul> Para obtener más información sobre estos valores, vea [controladores y atributos de paquete](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759262(v=ws.11)). |

## <a name="examples"></a>Ejemplos

Para agregar un paquete de grupo de controladores, escriba:

```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:printerdrivers /Filtertype:PackageClass /Operator:Equal /Value:printer /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2
```

```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:DisplayDriversX86 /Filtertype:PackageClass /Operator:Equal /Value:Display /Filtertype:PackageArchitecture /Operator:Equal /Value:x86 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil Add-driverpackage (comando)](wdsutil-add-driverpackage.md)

- [WDSUtil Add-drivergrouppackage (comando)](wdsutil-add-drivergrouppackage.md)

- [WDSUtil Add-alldriverpackages (comando)](wdsutil-add-alldriverpackages.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
