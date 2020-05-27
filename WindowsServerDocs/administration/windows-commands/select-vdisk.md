---
title: seleccionar vDisk
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 23741d9211ebdf98ac198af2ae1c562724a1955b
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821085"
---
# <a name="select-vdisk"></a>seleccionar vDisk

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

selecciona el VHD del disco duro virtual especificado \( \) y desplaza el foco a él.

> [!NOTE]
> Este comando solo es aplicable a Windows 7 y Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxis

```
select vdisk file=<full path> [noerr]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
|filesystem\=<full path>|Especifica la ruta de acceso completa y el nombre de un archivo VHD existente.|
|noerr|Se usa solo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error.|

## <a name="examples"></a>Ejemplos
Para desplazar el foco al VHD denominado test. vhd, escriba:

```
select vdisk file=c:\test\test.vhd
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

-   [attach vdisk](attach-vdisk.md)

-   [Compact vDisk](compact-vdisk.md)



-   [Detach vDisk](detach-vdisk.md)

-   [detalles del vDisk](detail-vdisk.md)

-   [expandir vDisk](expand-vdisk.md)

-   [Merge vDisk](merge-vdisk.md)

-   [list_1](list_1.md)


