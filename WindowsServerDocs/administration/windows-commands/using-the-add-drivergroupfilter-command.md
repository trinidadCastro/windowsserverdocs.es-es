---
title: Add-DriverGroupFilter
description: Artículo de referencia de Add-DriverGroupFilter, que agrega un filtro a un grupo de controladores en un servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8061bbde188fca5dbdd5a97ddfd708aedf1c9be
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937225"
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
| FilterType\<FilterType>  |                                                                                                                                                                                                   Especifica el tipo de filtro que se va a agregar al grupo. Puede especificar varios tipos de filtro en un solo comando. Cada tipo de filtro debe ir seguido de **/Policy** e incluir al menos un **/Value**. \<FilterType>puede ser **BiosVendor**, **BiosVersion**, **ChassisType**, **manufacturer**, **UUID**, **OsVersion**, **OsEdition**o **OsLanguage**. Para obtener información acerca de cómo obtener los valores para todos los demás tipos de filtro, vea [filtros de grupo de controladores](https://go.microsoft.com/fwlink/?LinkID=155158) ( <https://go.microsoft.com/fwlink/?LinkID=155158> ).                                                                                                                                                                                                    |
|     [/Policy: {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Exclude}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/Value: \<Value> ]      | Especifica el valor de cliente que corresponde a **/FilterType**. Puede especificar varios valores para un único tipo. Consulte la siguiente lista para ver los valores válidos de **ChassisType**. Para obtener información acerca de cómo obtener los valores para todos los demás tipos de filtro, vea [filtros de grupo de controladores](https://go.microsoft.com/fwlink/?LinkID=155158) ( <https://go.microsoft.com/fwlink/?LinkID=155158> ).</br>**Otros**</br>**UnknownChassis**</br>**Escritorio**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**Minitorre**</br>**Torre**</br>**Portátil**</br>**Equipo portátil**</br>**Cuaderno**</br>**X30**</br>**DockingStation**</br>**AllInOne**</br>**Subcuaderno**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Subchasis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

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

