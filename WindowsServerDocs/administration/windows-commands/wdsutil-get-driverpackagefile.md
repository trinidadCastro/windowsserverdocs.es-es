---
title: WDSUtil Get-driverpackagefile
description: Artículo de referencia para WDSUtil Get-driverpackagefile, que muestra información acerca de un paquete de controladores, incluidos los controladores y los archivos que contiene.
ms.topic: reference
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 900fc109c52908870733d4892e6d70f4a7b84c07
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524270"
---
# <a name="wdsutil-get-driverpackagefile"></a>WDSUtil Get-driverpackagefile

Muestra información acerca de un paquete de controladores, incluidos los controladores y los archivos que contiene.

## <a name="syntax"></a>Sintaxis

```
wdsutil /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>Parámetros

|         Parámetro         |                              Descripción                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile:\<Inf File path> | Especifica la ruta de acceso completa y el nombre de archivo del archivo. inf del paquete de controladores. |
|    [/Architecture: {x86    |                                  64                                  |
|     [/Show: {drivers      |                                 Archivos                                  |

## <a name="examples"></a>Ejemplos

Para ver información acerca de un archivo de controlador, escriba:
```
wdsutil /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)