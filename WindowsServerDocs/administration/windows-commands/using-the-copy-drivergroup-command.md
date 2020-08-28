---
title: Copy-DriverGroup
description: Artículo de referencia de Copy-DriverGroup, que duplica un grupo de controladores existente en el servidor, incluidos los filtros, los paquetes de controladores y el estado habilitado o deshabilitado.
ms.topic: reference
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ccd6676ff0d925ee83eea5a4b159fc3e4bf72ba3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038223"
---
# <a name="copy-drivergroup"></a>Copy-DriverGroup

Duplica un grupo de controladores existente en el servidor, incluidos los filtros, los paquetes de controladores y el estado habilitado o deshabilitado.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[/Server:\<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
|/DriverGroup:\<Source Group Name>|Especifica el nombre del grupo de controladores de origen.|
|GroupName\<New Group Name>|Especifica el nombre del nuevo grupo de controladores.|

## <a name="examples"></a>Ejemplos

Para copiar un grupo de controladores, escriba uno de los siguientes:
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)