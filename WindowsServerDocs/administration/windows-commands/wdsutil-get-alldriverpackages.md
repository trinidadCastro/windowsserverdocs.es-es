---
title: WDSUtil Get-alldriverpackages
description: Artículo de referencia del comando WDSUtil Get-alldriverpackages, que muestra información acerca de todos los paquetes de controladores de un servidor que coinciden con los criterios de búsqueda especificados.
ms.topic: reference
ms.assetid: 9eb8fcb7-ef46-4c8d-9623-8969a3c8b8a4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dd144d9abc2776f7e56913efd40775c1eef4d3ac
ms.sourcegitcommit: 28b5ab74cb0b40539ccc1a83998d6391e87fe51f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96614927"
---
# <a name="wdsutil-get-alldriverpackages"></a>WDSUtil Get-alldriverpackages

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de todos los paquetes de controladores de un servidor que coinciden con los criterios de búsqueda especificados.

## <a name="syntax"></a>Sintaxis

```
wdsutil /get-alldriverpackages [/server:<servername>] [/show:{drivers | files | all}] [/filtertype:<filtertype> /operator:{equal | notequal | greaterorequal | lessorequal | contains} /value:<value> [/value:<value> ...]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[/server:<servername>] `| Nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local. |
| `[/show:{drivers | files | all}]` | Indica la información de paquete que se va a mostrar. Si no se especifica **/Show** , el valor predeterminado es devolver solo los metadatos del paquete de controladores. **Controladores** muestra la lista de controladores del paquete, **archivos** muestra la lista de archivos del paquete, y **todos los** controladores y archivos. |
| `/filtertype:<filtertype>` | Especifica el atributo del paquete de controladores que se va a buscar. Puede especificar varios atributos en un solo comando. También debe especificar **/Operator** y **/Value** con esta opción.<p>`<filtertype>`Puede ser uno de los siguientes:<ul><li>PackageId</li><li>PackageName</li><li>PackageEnabled</li><li>Packagedateadded</li><li>PackageInfFilename</li><li>PackageClass</li><li>PackageProvider</li><li>PackageArchitecture</li><li>PackageLocale</li><li>PackageSigned</li><li>PackagedatePublished</li><li>Packageversion</li><li>Driverdescription</li><li>DriverManufacturer</li><li>DriverHardwareId</li><li>DrivercompatibleId</li><li>DriverGroupId</li><li>DriverGroupName</li></ul> |
| `/operator:{equal | notequal | greaterorequal | lessorequal | contains}` | Especifica la relación entre el atributo y los valores. Solo puede especificar **Contains** con atributos de cadena. Solo puede especificar **greaterorequal** y **lessorequal** con atributos de fecha y versión. |
| `/value:<value>` | Especifica el valor que se va a buscar para el especificado `<attribute>` . Puede especificar varios valores para un único **/FilterType**. En la lista siguiente se describen los atributos que se pueden especificar para cada filtro. Para obtener más información sobre estos atributos, vea [controladores y atributos de paquete](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759262(v=ws.11)). Los atributos pueden incluir:<ul><li>**PackageId.** Especifica un GUID válido. Por ejemplo: {4d36e972-E325-11ce-bfc1-08002be10318}.</li><li>**Nombredepaquete.** Especifica cualquier valor de cadena.</li><li>**PackageEnabled.** Especifica *yes* o *no*.</li><li>**Packagedateadded.** Especifica la fecha en el siguiente formato: AAAA/MM/DD</li><li>**PackageInfFilename.** Especifica cualquier valor de cadena.</li><li>**PackageClass.** Especifica un nombre de clase o un GUID de clase válidos. Por ejemplo: *DiskDrive*, *net* o *{4d36e972-E325-11ce-bfc1-08002be10318}*.</li><li>**PackageProvider.** Especifica cualquier valor de cadena.</li><li>**PackageArchitecture.** Especifica *x86*, *x64* o *ia64*.</li><li>**PackagLocale.** Especifica un identificador de idioma válido. Por ejemplo: *en-US* o *es-es*.</li><li>**PackageSigned.** Especifica **yes** o **no**.</li><li>**PackagedatePublished.** Especifica la fecha en el siguiente formato: AAAA/MM/DD.</li><li>**Packageversion.** Especifica la versión en el formato siguiente: a.b.x.y. Por ejemplo: 6.1.0.0.</li><li>**Driverdescription.** Especifica cualquier valor de cadena.</li><li>**DriverManufacturer.** Especifica cualquier valor de cadena.</li><li>**DriverHardwareId.** Especifica cualquier valor de cadena.</li><li>**DrivercompatibleId.** Especifica cualquier valor de cadena.</li><li>**DriverExcludeId.** Especifica cualquier valor de cadena.</li><li>**DriverGroupId.** Especifica un GUID válido. Por ejemplo: {4d36e972-E325-11ce-bfc1-08002be10318}.</li><li>**DriverGroupName.** Especifica cualquier valor de cadena.</li></ul> |

## <a name="examples"></a>Ejemplos

Para mostrar información, escriba:

```
wdsutil /get-alldriverpackages /server:MyWdsServer /show:all /filtertype:drivergroupname /operator:contains /value:printer /filtertype:packagearchitecture /operator:equal /value:x64 /value:x86
```

```
wdsutil /get-alldriverpackages /show:drivers /filtertype:packagedateadded /operator:greaterorequal /value:2008/01/01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil Get-driverpackage (comando)](wdsutil-get-driverpackage.md)

- [WDSUtil Get-driverpackagefile (comando)](wdsutil-get-driverpackagefile.md)
