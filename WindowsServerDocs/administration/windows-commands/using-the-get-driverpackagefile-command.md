---
title: Mediante el comando get-DriverPackageFile
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 264bdb6d51622e6323be00b44014b86cd9662e61
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440503"
---
# <a name="using-the-get-driverpackagefile-command"></a>Mediante el comando get-DriverPackageFile



Muestra información sobre un paquete de controladores, incluidos los controladores y los archivos que contiene.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parámetros

|         Parámetro         |                              Descripción                               |
|---------------------------|------------------------------------------------------------------------|
| / ArchivoInf:\<ruta de acceso de archivo Inf > | Especifica la ruta de acceso y el nombre completo del archivo .inf del paquete de controladores. |
|    [/ Arquitectura: {x86    |                                  ia64                                  |
|     [/ Mostrar: {controladores      |                                 Archivos                                  |

## <a name="BKMK_examples"></a>Ejemplos

Para ver información acerca de un archivo de controlador, escriba:
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)