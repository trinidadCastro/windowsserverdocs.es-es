---
title: Con el comando add-DriverGroupFilter
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 16962bd87fabd1ee689b962547c2b1aa470283c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817386"
---
# <a name="using-the-add-drivergroupfilter-command"></a>Con el comando add-DriverGroupFilter



Agrega un filtro a un grupo de controladores en un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ DriverGroup:\<nombre de grupo >|Especifica el nombre del grupo de controladores.|
|[/ Server:\<nombre del servidor >]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se usa el servidor local.|
|/FilterType:\<FilterType>|Especifica el tipo de filtro va a agregar al grupo. Puede especificar varios tipos de filtro en un solo comando. Cada tipo de filtro debe ir seguido por **/Policy** e incluir al menos una **/valor**. \<FilterType > puede ser **BiosVendor**, **BiosVersion**, **ChassisType**, **fabricante**, **Uuid**, **OsVersion**, **OsEdition**, o **OsLanguage**. Para obtener información acerca de cómo obtener los valores para todos los demás tipos de filtro, vea [los filtros de grupo de controladores](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).|
|[/ Directiva: {incluyen | Excluir}]|Si **/Policy** está establecido en **Include**, pueden instalar los controladores de este grupo de equipos cliente que coinciden con el filtro. Si **/Policy** está establecido en **excluir**, los equipos cliente que coinciden con el filtro no se permiten para instalar los controladores de este grupo.|
|[/ Valor:\<valor >]|Especifica el valor de cliente que se corresponde con **/FilterType**. Puede especificar varios valores para un tipo único. Consulte la siguiente lista de valores válidos para **ChassisType**. Para obtener información acerca de cómo obtener los valores para todos los demás tipos de filtro, vea [los filtros de grupo de controladores](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).</br>**Otros**</br>**UnknownChassis**</br>**Equipo de escritorio**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Tower**</br>**Portable**</br>**Laptop**</br>**Notebook**</br>**Handheld**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca**|

## <a name="BKMK_examples"></a>Ejemplos

Para agregar un filtro a un grupo de controladores, escriba uno de los siguientes:
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

