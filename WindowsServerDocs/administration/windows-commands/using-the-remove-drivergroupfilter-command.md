---
title: Remove-DriverGroupFilter
description: Comando de comandos de Windows para Remove-DriverGroupFilter, que quita una regla de filtro de un grupo de controladores en un servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837bd5d4-c79d-4714-942d-9875bd8e61dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08914b66c37d327ddef2a50d2f98adcfdbb88ffe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830498"
---
# <a name="remove-drivergroupfilter"></a>Remove-DriverGroupFilter



Quita una regla de filtro de un grupo de controladores en un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/DriverGroup: nombre del grupo de\<>|Especifica el nombre del grupo de controladores.|
|[/Server:\<nombre de servidor >]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local.|
|[/FilterType:\<FilterType >]|Especifica el tipo de filtro que se va a quitar del grupo. \<FilterType > puede ser uno de los siguientes:</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Le**</br>**UUID**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para quitar un filtro, escriba uno de los siguientes:
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer
```
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /FilterType:OSLanguage
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)