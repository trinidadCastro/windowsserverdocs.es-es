---
title: versiones de Wbadmin get
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b986acc4-d083-4d32-9434-862314ed5e97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4ebbd0d78de0ffbff1ee8c658d6d9811b87df1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813536"
---
# <a name="wbadmin-get-versions"></a>versiones de Wbadmin get



Muestra detalles acerca de las copias de seguridad disponibles que se almacenan en el equipo local o en otro equipo. Cuando se usa este subcomando sin parámetros, se enumeran todas las copias de seguridad del equipo local, incluso si esas copias de seguridad no están disponibles. Los detalles proporcionados para una copia de seguridad incluyen el tiempo de copia de seguridad, la ubicación de almacenamiento de copia de seguridad, el identificador de versión (necesario para la **wbadmin obtener elementos** subcomando y llevar a cabo recuperaciones) y el tipo de recuperaciones puede llevar a cabo.

Para obtener información acerca de las copias de seguridad disponibles mediante este subcomando, debe ser miembro de la **operadores de copia de seguridad** grupo o la **administradores** grupo, o bien debe haber sido delegado adecuado permisos. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados **símbolo** y, a continuación, haga clic en **ejecutar como administrador**.)

Para obtener ejemplos de cómo usar este subcomando, consulte [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
wbadmin get versions
[-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}]
[-machine:BackupMachineName]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-backupTarget|Especifica la ubicación de almacenamiento que contiene las copias de seguridad que desea que los detalles de. Se utiliza para enumerar las copias de seguridad almacenadas en esa ubicación de destino. Ubicaciones de destino de copia de seguridad pueden ser unidades de disco conectadas localmente, volúmenes, carpetas compartidas remotas, medios extraíbles como unidades de DVD u otro medio óptico. Si **wbadmin obtener versiones** se ejecuta en el mismo equipo donde se creó la copia de seguridad, este parámetro no es necesario. Sin embargo, este parámetro es necesario para obtener información acerca de una copia de seguridad creado a partir de otro equipo.|
|-machine|Especifica el equipo que desea ver los detalles de copia de seguridad. Se utiliza cuando se almacenan las copias de seguridad de varios equipos en la misma ubicación. Debe usarse cuando **- backupTarget** se especifica.|

## <a name="remarks"></a>Comentarios

Para enumerar los elementos disponibles para la recuperación de una copia de seguridad específica, use **wbadmin obtener elementos**.

## <a name="BKMK_examples"></a>Ejemplos

Para ver una lista de copias de seguridad disponibles que están almacenados en el volumen h, escriba:
```
wbadmin get versions -backupTarget:h:
```
Para ver una lista de copias de seguridad disponibles que se almacenan en la carpeta compartida remota \\ \\servername\share para el equipo server01, escriba:
```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBackupTarget](https://technet.microsoft.com/library/jj902447.aspx) cmdlet