---
title: wbadmin restore catalog
description: Artículo de referencia del comando Wbadmin restore Catalog, que recupera un catálogo de copia de seguridad del equipo local desde una ubicación de almacenamiento que especifique.
ms.topic: reference
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 046c4b4162c9512d5893c3c06e83e7260386c990
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155862"
---
# <a name="wbadmin-restore-catalog"></a>wbadmin restore catalog

Recupera un catálogo de copia de seguridad para el equipo local desde una ubicación de almacenamiento que especifique.

Para recuperar un catálogo de copia de seguridad incluido en una copia de seguridad específica mediante este comando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

> [!NOTE]
> Si la ubicación (disco, DVD o carpeta compartida remota) donde se almacenan las copias de seguridad está dañada o se ha perdido y no se puede usar para restaurar el catálogo de copias de seguridad, ejecute el comando [Wbadmin Delete Catalog](wbadmin-delete-catalog.md) para eliminar el catálogo dañado. En este caso, se recomienda crear una nueva copia de seguridad después de que se elimine el catálogo de copias de seguridad.

## <a name="syntax"></a>Sintaxis

```
wbadmin restore catalog -backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>} [-machine:<BackupMachineName>] [-quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -backupTarget | Especifica la ubicación del catálogo de copias de seguridad del sistema tal como se encontraba en el momento en que se creó la copia de seguridad. |
| -equipo | Especifica el nombre del equipo para el que desea recuperar el catálogo de copia de seguridad. Se utiliza cuando las copias de seguridad de varios equipos se han almacenado en la misma ubicación. Se debe usar cuando se especifica **-backupTarget** . |
| -quiet | Ejecuta el comando sin preguntar al usuario. |

## <a name="examples"></a>Ejemplos

Para restaurar un catálogo a partir de una copia de seguridad almacenada en disco D:, escriba:

```
wbadmin restore catalog -backupTarget:D
```

Para restaurar un catálogo a partir de una copia de seguridad almacenada en la carpeta compartida `\\<servername>\<share>` de Server01, escriba:

```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin Delete Catalog](wbadmin-delete-catalog.md)

- [Restore-WBCatalog](/powershell/module/windowserverbackup/Restore-WBCatalog)
