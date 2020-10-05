---
title: Add-DriverGroupFilter
description: Artículo de referencia de Add-DriverGroupFilter, que agrega un filtro a un grupo de controladores en un servidor.
ms.topic: reference
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bf11cbe86242a8051b173aa23f53748c20aa4ca6
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731102"
---
# <a name="add-drivergroupfilter"></a>Add-DriverGroupFilter

Agrega un filtro a un grupo de controladores en un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

### <a name="parameters"></a>Parámetros

|         Parámetro          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:\<Group Name> |                                                                                                                                                                                                                                                                                                                                                                                                                                                              Especifica el nombre del grupo de controladores.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|  [/Server:\<Server name>]  |                                                                                                                                                                                                                                                                                                                                                                                                               Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local.                                                                                                                                                                                                                                                                                                                                                                                                               |
| FilterType\<FilterType>  |                                                                                                                                                                                                   Especifica el tipo de filtro que se va a agregar al grupo. Puede especificar varios tipos de filtro en un solo comando. Cada tipo de filtro debe ir seguido de **/Policy** e incluir al menos un **/Value**. \<FilterType> puede ser **BiosVendor**, **BiosVersion**, **ChassisType**, **manufacturer**, **UUID**, **OsVersion**, **OsEdition**o **OsLanguage**. Para obtener información acerca de cómo obtener los valores para todos los demás tipos de filtro, vea [filtros de grupo de controladores](https://go.microsoft.com/fwlink/?LinkID=155158) ( <https://go.microsoft.com/fwlink/?LinkID=155158> ).                                                                                                                                                                                                    |
|     [/Policy: {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Exclude}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/Value: \<Value> ]      | Especifica el valor de cliente que corresponde a **/FilterType**. Puede especificar varios valores para un único tipo. Consulte la siguiente lista para ver los valores válidos de **ChassisType**. Para obtener información acerca de cómo obtener los valores para todos los demás tipos de filtro, vea [filtros de grupo de controladores](https://go.microsoft.com/fwlink/?LinkID=155158) ( <https://go.microsoft.com/fwlink/?LinkID=155158> ).</br>**Otros**</br>**UnknownChassis**</br>**Dispositivo de escritorio**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**Minitorre**</br>**Torre**</br>**Portátil**</br>**Equipo portátil**</br>**Cuaderno**</br>**X30**</br>**DockingStation**</br>**AllInOne**</br>**Subcuaderno**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Subchasis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

## <a name="examples"></a>Ejemplos

Para agregar un filtro a un grupo de controladores, escriba uno de los siguientes:
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

