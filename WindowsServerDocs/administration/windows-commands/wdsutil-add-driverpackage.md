---
title: Add-DriverPackage
description: Artículo de referencia del comando Add-DriverPackage, que agrega un paquete de controladores al servidor.
ms.topic: reference
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c85279cc160ca12b52f4cf7225d0ac41700973bc
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524530"
---
# <a name="add-driverpackage"></a>Add-DriverPackage

Agrega un paquete de controladores al servidor.

## <a name="syntax"></a>Sintaxis

```
wdsutil /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /InfFile:`<InfFilepath>` | Especifica la ruta de acceso completa del archivo. inf que se va a agregar. |
| [/Server:`<Servername>`] | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local. |
| [/Architecture: `{x86 | ia64 | x64}` ] | Especifica el tipo de arquitectura para el paquete de controladores. |
| [/DriverGroup: `<groupname>` ] | Especifica el nombre del grupo de controladores al que se deben agregar los paquetes. |
| [/Name: `<friendlyname>` ] | Especifica el nombre descriptivo para el paquete de controladores. |

## <a name="examples"></a>Ejemplos

Para agregar un paquete de controladores, escriba:

```
wdsutil /verbose /Add-DriverPackage /InfFile:C:\Temp\Display.inf
```

```
wdsutil /Add-DriverPackage /Server:MyWDSServer /InfFile:C:\Temp\Display.inf /Architecture:x86 /DriverGroup:x86Drivers /Name:Display Driver
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil Add-drivergrouppackage (comando)](wdsutil-add-drivergrouppackage.md)

- [WDSUtil Add-alldriverpackages (comando)](wdsutil-add-alldriverpackages.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
