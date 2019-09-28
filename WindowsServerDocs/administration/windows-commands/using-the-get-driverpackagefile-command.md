---
title: Usar el comando Get-DriverPackageFile
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21bbe17e56177da5cd2c1bf83c712d256cc794c8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363151"
---
# <a name="using-the-get-driverpackagefile-command"></a>Usar el comando Get-DriverPackageFile



Muestra información acerca de un paquete de controladores, incluidos los controladores y los archivos que contiene.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parámetros

|         Parámetro         |                              Descripción                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile: ruta de acceso del archivo \<Inf > | Especifica la ruta de acceso completa y el nombre de archivo del archivo. inf del paquete de controladores. |
|    [/Architecture: {x86    |                                  64                                  |
|     [/Show: {drivers      |                                 Archivos                                  |

## <a name="BKMK_examples"></a>Example

Para ver información acerca de un archivo de controlador, escriba:
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)