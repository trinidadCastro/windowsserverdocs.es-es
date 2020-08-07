---
title: Add-DriverPackage
description: Artículo de referencia de Add-DriverPackage, que agrega un paquete de controladores al servidor.
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd7ba7897006a4db144fc8bd92317fb07a34b55f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881269"
---
# <a name="add-driverpackage"></a>Add-DriverPackage

Agrega un paquete de controladores al servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

### <a name="parameters"></a>Parámetros

|          Parámetro           |                                                              Descripción                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|   InfFile:\<Inf File path>   |                                           Especifica la ruta de acceso completa del archivo. inf que se va a agregar.                                            |
|    Servidor\<Server name>    | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
|      /Architecture: {x86      |                                                                 64                                                                  |
| [/DriverGroup: \<Group Name> ] |                             Especifica el nombre del grupo de controladores al que se debe agregar el paquete.                              |
|   [/Name: \<Friendly Name> ]   |                                           Indica el nombre descriptivo para el paquete de controladores.                                            |

## <a name="examples"></a>Ejemplos

Para agregar un paquete de controladores, escriba uno de los siguientes:
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:C:\Temp\Display.inf
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:C:\Temp\Display.inf /Architecture:x86 /DriverGroup:x86Drivers /Name:Display Driver
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

