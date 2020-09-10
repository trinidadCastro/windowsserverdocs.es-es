---
title: select vdisk
description: Artículo de referencia de * * * *-
ms.topic: reference
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: daa6d3047e28fb8f6084824fdf1557423daa52b8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639141"
---
# <a name="select-vdisk"></a>select vdisk

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

-   [compact vdisk](compact-vdisk.md)



-   [Detach vDisk](detach-vdisk.md)

-   [detail vdisk](detail-vdisk.md)

-   [expand vdisk](expand-vdisk.md)

-   [Merge vDisk](merge-vdisk.md)

-   [list_1](./list.md)