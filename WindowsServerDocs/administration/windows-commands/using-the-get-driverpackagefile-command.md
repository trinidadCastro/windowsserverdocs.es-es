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
ms.openlocfilehash: ed9518fae07745502d01dc0084b7443a1332db83
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859806"
---
# <a name="using-the-get-driverpackagefile-command"></a>Mediante el comando get-DriverPackageFile



Muestra información sobre un paquete de controladores, incluidos los controladores y los archivos que contiene.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ ArchivoInf:\<ruta de acceso de archivo Inf >|Especifica la ruta de acceso y el nombre completo del archivo .inf del paquete de controladores.|
|[/ Arquitectura: {x86 | ia64 | x64}]|Especifica la arquitectura del paquete de controladores.|
|[/ Mostrar: {controladores | Archivos | All}]|Indica la información de paquete para mostrar. Si **/mostrar** no se especifica, el valor predeterminado es devolver solo el controlador de metadatos del paquete. **Controladores** muestra la lista de controladores en el paquete. **Archivos** muestra la lista de archivos en el paquete. **Todos los** muestra archivos y controladores.|

## <a name="BKMK_examples"></a>Ejemplos

Para ver información acerca de un archivo de controlador, escriba:
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)