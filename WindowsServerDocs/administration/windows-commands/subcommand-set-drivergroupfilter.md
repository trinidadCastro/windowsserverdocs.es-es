---
title: Subcomando conjunto DriverGroupFilter
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 339beda0d6e92c7632cb16566c8db7be5f1079af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835066"
---
# <a name="subcommand-set-drivergroupfilter"></a>Subcomando: set-DriverGroupFilter



Agrega o quita un filtro de grupo de controladores existente de un grupo de controladores.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ DriverGroup:\<nombre de grupo >|Especifica el nombre del grupo de controladores.|
|[/ Server:\<nombre del servidor >]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se usa el servidor local.|
|/FilterType:\<FilterType>|Especifica el tipo de controlador de filtro de grupo para agregar o quitar. Puede especificar varios filtros en un solo comando. Para cada **/FilterType**, puede agregar o quitar varios valores mediante **/RemoveValue** y **/AddValue**. \<FilterType > puede ser uno de los siguientes:</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Fabricante**</br>**Uuid**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|
|[/ Directiva: {incluyen | Excluir}]|Especifica la nueva directiva debe establecerse en el filtro. Si **/Policy** está establecido en **Include**, pueden instalar los controladores de este grupo de equipos cliente que coinciden con el filtro. Si **/Policy** está establecido en **excluir**, a continuación, los equipos cliente que ajustar el filtro no se permiten para instalar los controladores de este grupo.|
|[/AddValue:\<Value>]|Especifica el nuevo valor de cliente para agregar al filtro. Puede especificar varios valores para un tipo de filtro único. Consulte la siguiente lista de valores de atributo válido para **ChassisType**. Para obtener información acerca de cómo obtener los valores para todos los demás tipos de filtro, vea [los filtros de grupo de controladores](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).</br>**Otros**</br>**UnknownChassis**</br>**Equipo de escritorio**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Tower**</br>**Portable**</br>**Laptop**</br>**Notebook**</br>**Handheld**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca**|
|[/ RemoveValue:\<valor >]|Especifica el valor de cliente existentes para quitar el filtro tal como se especifica con **/AddValue**.|

## <a name="BKMK_examples"></a>Ejemplos

Para quitar un filtro, escriba uno de los siguientes:
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)