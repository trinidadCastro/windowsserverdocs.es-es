---
title: Subcomando set-DriverGroupFilter
description: Tema de referencia sobre el subcomando set-DriverGroupFilter, que agrega o quita un filtro de grupo de controladores existente de un grupo de controladores.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8a8d3567785b54a722b1787c519de7eb0b7a229
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721732"
---
# <a name="subcommand-set-drivergroupfilter"></a>Subcomando: set-DriverGroupFilter

Agrega o quita un filtro de grupo de controladores existente de un grupo de controladores.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

### <a name="parameters"></a>Parámetros

|         Parámetro          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:\<nombre de grupo> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                 Especifica el nombre del grupo de controladores.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|  [/Server:\<nombre del servidor>]  |                                                                                                                                                                                                                                                                                                                                                                                                                Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| /FilterType:\<FilterType>  |                                                                                                                                                                                                                                                                       Especifica el tipo de filtro de grupo de controladores que se va a agregar o quitar. Puede especificar varios filtros en un solo comando. Para cada **/FilterType**, puede Agregar o quitar varios valores mediante **/RemoveValue** y **/AddValue**. \<La> FilterType puede ser una de las siguientes:</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Fabricante**</br>**UUID**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**                                                                                                                                                                                                                                                                        |
|     [/Policy: {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                Exclude}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|    [/AddValue:\<valor>]    | Especifica el nuevo valor de cliente que se va a agregar al filtro. Puede especificar varios valores para un único tipo de filtro. Consulte la lista siguiente para ver los valores de atributo válidos para **ChassisType**. Para obtener información acerca de cómo obtener los valores para todos los demás tipos de filtro,<https://go.microsoft.com/fwlink/?LinkID=155158>vea filtros de grupo de [Controladores](https://go.microsoft.com/fwlink/?LinkID=155158) ().</br>**Otros**</br>**UnknownChassis**</br>**Escritorio**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**Minitorre**</br>**Torre**</br>**Portátil**</br>**Equipo portátil**</br>**Cuaderno**</br>**X30**</br>**DockingStation**</br>**AllInOne**</br>**Subcuaderno**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Subchasis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |
|  [/RemoveValue:\<valor>]   |                                                                                                                                                                                                                                                                                                                                                                                                                                     Especifica el valor de cliente existente que se va a quitar del filtro, tal y como se especifica con **/AddValue**.                                                                                                                                                                                                                                                                                                                                                                                                                                      |

## <a name="examples"></a>Ejemplos

Para quitar un filtro, escriba uno de los siguientes:
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)