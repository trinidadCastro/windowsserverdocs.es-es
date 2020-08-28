---
title: Usar el subcomando add-AllDriverPackages
description: Artículo de referencia de Add-AllDriverPackages, que agrega todos los paquetes de controladores que se almacenan en una carpeta a un servidor.
ms.topic: reference
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ccdd132687fdc65336aeb49d317f9ccf5fbcd968
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032190"
---
# <a name="add-alldriverpackages"></a>Add-AllDriverPackages

Agrega todos los paquetes de controladores que se almacenan en una carpeta a un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

### <a name="parameters"></a>Parámetros

|          Parámetro           |                                                              Descripción                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  FolderPath\<Folder Path>  |                      Especifica la ruta de acceso completa a la carpeta que contiene los archivos. inf para los paquetes de controladores.                      |
|   [/Server:\<Server name>]   | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
|     [/Architecture: {x86      |                                                                 64                                                                  |
| [/DriverGroup: \<Group Name> ] |                             Especifica el nombre del grupo de controladores al que se deben agregar los paquetes.                             |

## <a name="examples"></a>Ejemplos

Para agregar paquetes de controladores, escriba uno de los siguientes:
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers\Printers /DriverGroup:Printer Drivers
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Add-WdsDriverPackage](/previous-versions/windows/powershell-scripting/dn283440(v=wps.630))
