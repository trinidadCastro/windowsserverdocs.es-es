---
title: Add-DriverGroupFilter
description: Artículo de referencia del comando Add-DriverGroupFilter, que agrega un filtro a un grupo de controladores en un servidor.
ms.topic: reference
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bd9e26e7bdf6b1a03d01b2993c969bbbcf5b8754
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524610"
---
# <a name="add-drivergroupfilter"></a>Add-DriverGroupFilter

Agrega un filtro a un grupo de controladores en un servidor.

## <a name="syntax"></a>Sintaxis

```
wdsutil /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /DriverGroup:`<Groupname>` | Especifica el nombre del nuevo grupo de controladores. |
| Servidor`<Servername>` | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
| FilterType`<Filtertype>` | Especifica el tipo de filtro que se va a agregar al grupo. Puede especificar varios tipos de filtro en un solo comando. Cada tipo de filtro debe ir seguido de **/Policy** y al menos un **/Value**. Los valores válidos son:<ul><li>BiosVendor</li><li>Biosversion</li><li>Chassistype</li><li>Fabricante</li><li>Uuid</li><li>OSVersion</li><li>Osedition</li><li>OsLanguage</li></ul> Para obtener información acerca de cómo obtener valores para todos los demás tipos de filtro, vea [filtros de grupo de controladores](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759191(v=ws.11)). |
| [/Policy: `{Include|Exclude}` ] | Especifica la Directiva que se va a establecer en el filtro. Si **/Policy** se establece en **include**, los equipos cliente que coincidan con el filtro podrán instalar los controladores en este grupo. Si **/Policy** está establecido en **excluir**, los equipos cliente que coincidan con el filtro no podrán instalar los controladores de este grupo. |
| [/Value: `<Value>` ] | Especifica el valor de cliente que corresponde a **/FilterType**. Puede especificar varios valores para un único tipo. Para obtener información acerca de los valores de tipo de filtro aceptables, consulte [filtros de grupo de controladores](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759191(v=ws.11)). |

## <a name="examples"></a>Ejemplos

Para agregar un filtro a un grupo de controladores, escriba:

```
wdsutil /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```

```
wdsutil /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil Add-drivergrouppackage (comando)](wdsutil-add-drivergrouppackage.md)

- [WDSUtil Add-drivergrouppackages (comando)](wdsutil-add-drivergrouppackages.md)

- [WDSUtil Add-drivergroup (comando)](wdsutil-add-drivergroup.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
