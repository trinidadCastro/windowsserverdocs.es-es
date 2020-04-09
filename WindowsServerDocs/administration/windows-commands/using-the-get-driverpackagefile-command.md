---
title: Get-DriverPackageFile
description: Temas de comandos de Windows para Get-DriverPackageFile, que muestra información acerca de un paquete de controladores, incluidos los controladores y los archivos que contiene.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d485a24479aa857270968a1bff7bd55a014347a3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831038"
---
# <a name="get-driverpackagefile"></a>Get-DriverPackageFile

Muestra información acerca de un paquete de controladores, incluidos los controladores y los archivos que contiene.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>Parámetros

|         Parámetro         |                              Descripción                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile:\<ruta de acceso del archivo INF > | Especifica la ruta de acceso completa y el nombre de archivo del archivo. inf del paquete de controladores. |
|    [/Architecture: {x86    |                                  64                                  |
|     [/Show: {drivers      |                                 Files                                  |

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para ver información acerca de un archivo de controlador, escriba:
```
WDSUTIL /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)