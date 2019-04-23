---
title: Con el comando remove-DriverGroupFilter
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 837bd5d4-c79d-4714-942d-9875bd8e61dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a546ead7220273955368c582ac1e3f9b3f61c191
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883436"
---
# <a name="using-the-remove-drivergroupfilter-command"></a>Con el comando remove-DriverGroupFilter



Quita una regla de filtro de un grupo de controladores en un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ DriverGroup:\<nombre de grupo >|Especifica el nombre del grupo de controladores.|
|[/ Server:\<nombre del servidor >]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se usa el servidor local.|
|[/FilterType:\<FilterType>]|Especifica el tipo del filtro que se quite del grupo. \<FilterType > puede ser uno de los siguientes:</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Fabricante**</br>**Uuid**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|

## <a name="BKMK_examples"></a>Ejemplos

Para quitar un filtro, escriba uno de los siguientes:
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer
```
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /FilterType:OSLanguage
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)