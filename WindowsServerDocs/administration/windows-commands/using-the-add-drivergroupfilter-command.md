---
title: Uso del comando Add-DriverGroupFilter
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a332c804fd3c78598eb9b1a6ba8cb5bdae0b3f1c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363830"
---
# <a name="using-the-add-drivergroupfilter-command"></a>Uso del comando Add-DriverGroupFilter



agrega un filtro a un grupo de controladores en un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

## <a name="parameters"></a>Parámetros

|         Parámetro          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup: \<Group nombre > |                                                                                                                                                                                                                                                                                                                                                                                                                                                              Especifica el nombre del grupo de controladores.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|  [/Server: \<Server nombre >]  |                                                                                                                                                                                                                                                                                                                                                                                                               Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local.                                                                                                                                                                                                                                                                                                                                                                                                               |
| /FilterType: @no__t 0FilterType >  |                                                                                                                                                                                                   Especifica el tipo de filtro que se va a agregar al grupo. Puede especificar varios tipos de filtro en un solo comando. Cada tipo de filtro debe ir seguido de **/Policy** e incluir al menos un **/Value**. \<FilterType > puede ser **BiosVendor**, **BiosVersion**, **ChassisType**, **manufacturer**, **UUID**, **OsVersion**, **OsEdition**o **OsLanguage**. Para obtener información acerca de cómo obtener los valores para todos los demás tipos de filtro, consulte [filtros de grupo de controladores](https://go.microsoft.com/fwlink/?LinkID=155158) (<https://go.microsoft.com/fwlink/?LinkID=155158>).                                                                                                                                                                                                    |
|     [/Policy: {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Exclude}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/Value: \<Value >]      | Especifica el valor de cliente que corresponde a **/FilterType**. Puede especificar varios valores para un único tipo. Consulte la siguiente lista para ver los valores válidos de **ChassisType**. Para obtener información acerca de cómo obtener los valores para todos los demás tipos de filtro, consulte [filtros de grupo de controladores](https://go.microsoft.com/fwlink/?LinkID=155158) (<https://go.microsoft.com/fwlink/?LinkID=155158>).</br>**Distinta**</br>**UnknownChassis**</br>**Equipo de escritorio**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**Minitorre**</br>**Torre**</br>**Portable**</br>**Portátil**</br>**Inspiron**</br>**X30**</br>**DockingStation**</br>**AllInOne**</br>**Subcuaderno**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Subchasis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

## <a name="BKMK_examples"></a>Example

Para agregar un filtro a un grupo de controladores, escriba uno de los siguientes:
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

