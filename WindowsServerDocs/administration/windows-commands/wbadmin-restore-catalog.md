---
title: Wbadmin restore Catalog
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0d646440ca9b30f9fa30fb1ac3ff08458b8e44d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362328"
---
# <a name="wbadmin-restore-catalog"></a>Wbadmin restore Catalog



Recupera un catálogo de copia de seguridad para el equipo local desde una ubicación de almacenamiento que especifique.

Para recuperar un catálogo de copia de seguridad con este subcomando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).

Para obtener ejemplos de cómo usar este subcomando, vea [ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-backupTarget|Especifica la ubicación del catálogo de copias de seguridad del sistema tal como se encontraba en el momento en que se creó la copia de seguridad.|
|-equipo|Especifica el nombre del equipo para el que desea recuperar el catálogo de copia de seguridad. Se utiliza cuando las copias de seguridad de varios equipos se han almacenado en la misma ubicación. Se debe usar cuando se especifica **-backupTarget** .|
|-quiet|Ejecuta el subcomando sin preguntar al usuario.|

## <a name="remarks"></a>Observaciones

Si la ubicación (disco, DVD o carpeta compartida remota) donde se almacenan las copias de seguridad está dañada o se ha perdido y no se puede usar para restaurar el catálogo de copias de seguridad, use **Wbadmin Delete Catalog** para eliminar el catálogo dañado. En este caso, debe crear una nueva copia de seguridad una vez que se elimine el catálogo de copias de seguridad.

## <a name="BKMK_examples"></a>Example

Para restaurar un catálogo a partir de una copia de seguridad almacenada en disco d:, escriba:
```
wbadmin restore catalog -backupTarget:d
```
Para restaurar un catálogo a partir de una copia de seguridad almacenada en la carpeta compartida \\\\servername\share de Server01, escriba:
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [restore-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx)