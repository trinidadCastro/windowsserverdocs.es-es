---
title: catálogo de restauración de Wbadmin
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5876a44b178025baac7ee5901cdc32c1b5d33dad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851716"
---
# <a name="wbadmin-restore-catalog"></a>catálogo de restauración de Wbadmin



Recupera un catálogo de copia de seguridad para el equipo local desde una ubicación de almacenamiento que especifique.

Para recuperar un catálogo de copia de seguridad con este subcomando, debe ser miembro de la **operadores de copia de seguridad** grupo o la **administradores** grupo, o bien debe haber sido delegar los permisos adecuados. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados de contextual **símbolo**y, a continuación, haga clic en **ejecutar como administrador**.)

Para obtener ejemplos de cómo usar este subcomando, consulte [ejemplos](#BKMK_examples).

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
|-backupTarget|Especifica la ubicación del catálogo de copia de seguridad del sistema que estaba en el punto después de que se creó la copia de seguridad.|
|-machine|Especifica el nombre del equipo que desea recuperar el catálogo de copia de seguridad. Se utiliza cuando las copias de seguridad para varios equipos se han almacenado en la misma ubicación. Debe usarse cuando **- backupTarget** se especifica.|
|-quiet|Se ejecuta el subcomando sin solicitudes para el usuario.|

## <a name="remarks"></a>Comentarios

Si se daña o se pierde la ubicación donde se almacenan las copias de seguridad (disco, DVD o carpeta compartida remota) y no se puede usar para restaurar el catálogo de copia de seguridad, use **wbadmin Eliminar catálogo** para eliminar el catálogo está dañado. En este caso, debe crear una nueva copia de seguridad una vez que se elimina el catálogo de copia de seguridad.

## <a name="BKMK_examples"></a>Ejemplos

Para restaurar un catálogo desde una copia de seguridad almacenada en la unidad de disco d:, escriba:
```
wbadmin restore catalog -backupTarget:d
```
Para restaurar un catálogo desde una copia de seguridad almacenada en la carpeta compartida \\ \\servername\share de server01, escriba:
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Restore-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx) cmdlet