---
title: wbadmin restore catalog
description: Artículo de referencia de Wbadmin restore Catalog, que recupera un catálogo de copia de seguridad del equipo local desde una ubicación de almacenamiento que especifique.
ms.topic: reference
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c551aa77ff98a66d3faacc3901806be8cc67b8f2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637599"
---
# <a name="wbadmin-restore-catalog"></a>wbadmin restore catalog

Recupera un catálogo de copia de seguridad para el equipo local desde una ubicación de almacenamiento que especifique.

Para recuperar un catálogo de copia de seguridad con este subcomando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).

## <a name="syntax"></a>Sintaxis

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-backupTarget|Especifica la ubicación del catálogo de copias de seguridad del sistema tal como se encontraba en el momento en que se creó la copia de seguridad.|
|-equipo|Especifica el nombre del equipo para el que desea recuperar el catálogo de copia de seguridad. Se utiliza cuando las copias de seguridad de varios equipos se han almacenado en la misma ubicación. Se debe usar cuando se especifica **-backupTarget** .|
|-quiet|Ejecuta el subcomando sin preguntar al usuario.|

## <a name="remarks"></a>Observaciones

Si la ubicación (disco, DVD o carpeta compartida remota) donde se almacenan las copias de seguridad está dañada o se ha perdido y no se puede usar para restaurar el catálogo de copias de seguridad, use **Wbadmin Delete Catalog** para eliminar el catálogo dañado. En este caso, debe crear una nueva copia de seguridad una vez que se elimine el catálogo de copias de seguridad.

## <a name="examples"></a>Ejemplos

Para restaurar un catálogo a partir de una copia de seguridad almacenada en disco d:, escriba:
```
wbadmin restore catalog -backupTarget:d
```
Para restaurar un catálogo a partir de una copia de seguridad almacenada en la carpeta compartida \\ \\ servername\share de Server01, escriba:
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [restore-WBCatalog](/powershell/module/windowserverbackup/?view=winserver2012r2-ps)
