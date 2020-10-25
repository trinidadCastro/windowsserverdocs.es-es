---
title: WDSUtil Add-alldriverpackages
description: Artículo de referencia del comando WDSUtil Add-alldriverpackages, que agrega todos los paquetes de controladores que se almacenan en una carpeta a un servidor.
ms.topic: reference
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 07d958fb382f040db34f486a28fed4b924ce45d8
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524660"
---
# <a name="wdsutil-add-alldriverpackages"></a>WDSUtil Add-alldriverpackages

Agrega todos los paquetes de controladores que se almacenan en una carpeta a un servidor.

## <a name="syntax"></a>Sintaxis

```
wdsutil /Add-AllDriverPackages /FolderPath:<folderpath> [/Server:<servername>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<groupname>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| FolderPath`<folderpath>` | Especifica la ruta de acceso completa a la carpeta que contiene los archivos. inf para los paquetes de controladores. |
| [/Server:`<servername>`] | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
| [/Architecture: `{x86|ia64|x64}` ] | Especifica el tipo de arquitectura para el paquete de controladores. |
| [/DriverGroup: `<groupname>` ] | Especifica el nombre del grupo de controladores al que se deben agregar los paquetes. |

## <a name="examples"></a>Ejemplos

Para agregar paquetes de controladores, escriba:

```
wdsutil /verbose /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers /Architecture:x86
```

```
wdsutil /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers\Printers /DriverGroup:Printer Drivers
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)

- [Add-WdsDriverPackage](/powershell/module/wds/add-wdsdriverpackage)
