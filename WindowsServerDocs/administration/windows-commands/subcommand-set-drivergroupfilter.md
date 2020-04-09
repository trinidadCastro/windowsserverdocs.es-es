---
title: Subcomando set-DriverGroupFilter
description: Tema de comandos de Windows para el subcomando set-DriverGroupFilter, que agrega o quita un filtro de grupo de controladores existente de un grupo de controladores.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a808bfd893e1f149a745db865fdc8bd28a618699
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834027"
---
# <a name="subcommand-set-drivergroupfilter"></a>Subcomando: set-DriverGroupFilter

agrega o quita un filtro de grupo de controladores existente de un grupo de controladores.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

### <a name="parameters"></a>Parámetros

|         Parámetro          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup: nombre del grupo de\<> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                 Especifica el nombre del grupo de controladores.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|  [/Server:\<nombre de servidor >]  |                                                                                                                                                                                                                                                                                                                                                                                                                Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| /FilterType:\<FilterType >  |                                                                                                                                                                                                                                                                       Especifica el tipo de filtro de grupo de controladores que se va a agregar o quitar. Puede especificar varios filtros en un solo comando. Para cada **/FilterType**, puede Agregar o quitar varios valores mediante **/RemoveValue** y **/AddValue**. \<FilterType > puede ser uno de los siguientes:</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Le**</br>**UUID**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**                                                                                                                                                                                                                                                                        |
|     [/Policy: {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                Exclude}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|    [/AddValue:\<valor >]    | Especifica el nuevo valor de cliente que se va a agregar al filtro. Puede especificar varios valores para un único tipo de filtro. Consulte la lista siguiente para ver los valores de atributo válidos para **ChassisType**. Para obtener información acerca de cómo obtener los valores para todos los demás tipos de filtro, vea [filtros de grupo de controladores](https://go.microsoft.com/fwlink/?LinkID=155158) (<https://go.microsoft.com/fwlink/?LinkID=155158>).</br>**Distinta**</br>**UnknownChassis**</br>**Equipo de escritorio**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**Minitorre**</br>**Torre**</br>**Portable**</br>**Portátil**</br>**Inspiron**</br>**X30**</br>**DockingStation**</br>**AllInOne**</br>**Subcuaderno**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Subchasis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |
|  [/RemoveValue:\<valor >]   |                                                                                                                                                                                                                                                                                                                                                                                                                                     Especifica el valor de cliente existente que se va a quitar del filtro, tal y como se especifica con **/AddValue**.                                                                                                                                                                                                                                                                                                                                                                                                                                      |

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para quitar un filtro, escriba uno de los siguientes:
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)