---
title: WDSUtil Add-alldriverpackages
description: Artículo de referencia de WDSUtil Add-alldriverpackages, que agrega todos los paquetes de controladores que se almacenan en una carpeta a un servidor.
ms.topic: reference
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d3ec9b3d61c481eacd192ff74e149dbb56134b17
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731162"
---
# <a name="wdsutil-add-alldriverpackages"></a>WDSUtil Add-alldriverpackages

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

- [wsdutil Add-wdsdriverpackage](/previous-versions/windows/powershell-scripting/dn283440(v=wps.630))
