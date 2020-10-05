---
title: WDSUtil Get-driverpackagefile
description: Artículo de referencia para WDSUtil Get-driverpackagefile, que muestra información acerca de un paquete de controladores, incluidos los controladores y los archivos que contiene.
ms.topic: reference
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f85d57a81aa63f6ecb94b4b09a1614403aa19664
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730837"
---
# <a name="wdsutil-get-driverpackagefile"></a>WDSUtil Get-driverpackagefile

Muestra información acerca de un paquete de controladores, incluidos los controladores y los archivos que contiene.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
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
WDSUTIL /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)