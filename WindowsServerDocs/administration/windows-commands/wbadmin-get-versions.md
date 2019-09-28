---
title: versiones de Wbadmin get
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0b7ba0749c8ef347e27590bde4eed7bbcf25af7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362359"
---
# <a name="wbadmin-get-versions"></a>versiones de Wbadmin get



Muestra detalles acerca de las copias de seguridad disponibles almacenadas en el equipo local o en otro equipo. Cuando este subcomando se usa sin parámetros, enumera todas las copias de seguridad del equipo local, aunque dichas copias de seguridad no estén disponibles. Los detalles proporcionados para una copia de seguridad incluyen el tiempo de copia de seguridad, la ubicación de almacenamiento de copia de seguridad, el identificador de la versión (necesario para el subcomando de **Wbadmin get items** y para realizar recuperaciones) y el tipo de recuperaciones que se pueden realizar.

Para obtener detalles acerca de las copias de seguridad disponibles con este subcomando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un **símbolo** del sistema con privilegios elevados del símbolo del sistema y, a continuación, haga clic en **Ejecutar como administrador**).

Para obtener ejemplos de cómo usar este subcomando, vea [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
wbadmin get versions
[-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}]
[-machine:BackupMachineName]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-backupTarget|Especifica la ubicación de almacenamiento que contiene las copias de seguridad de las que desea obtener información. Se usa para enumerar las copias de seguridad almacenadas en esa ubicación de destino. Las ubicaciones de destino de copia de seguridad pueden ser unidades de disco conectadas localmente, volúmenes, carpetas compartidas remotas, medios extraíbles, como unidades de DVD u otros medios ópticos. Si las **versiones de Wbadmin get** se ejecutan en el mismo equipo en el que se creó la copia de seguridad, este parámetro no es necesario. Sin embargo, este parámetro es necesario para obtener información acerca de una copia de seguridad creada desde otro equipo.|
|-equipo|Especifica el equipo para el que desea obtener detalles de la copia de seguridad. Se utiliza cuando las copias de seguridad de varios equipos se almacenan en la misma ubicación. Se debe usar cuando se especifica **-backupTarget** .|

## <a name="remarks"></a>Comentarios

Para enumerar los elementos disponibles para la recuperación a partir de una copia de seguridad específica, use **Wbadmin get items**.

## <a name="BKMK_examples"></a>Example

Para ver una lista de las copias de seguridad disponibles almacenadas en el volumen h, escriba:
```
wbadmin get versions -backupTarget:h:
```
Para ver una lista de las copias de seguridad disponibles almacenadas en la carpeta compartida remota \\ @ no__t-1servername\share para el equipo Server01, escriba:
```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBBackupTarget](https://technet.microsoft.com/library/jj902447.aspx)