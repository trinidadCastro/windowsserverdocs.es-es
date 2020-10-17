---
title: wbadmin get versions
description: Artículo de referencia del comando Wbadmin get Versions, que muestra detalles sobre las copias de seguridad disponibles almacenadas en el equipo local o en otro equipo.
ms.topic: reference
ms.assetid: b986acc4-d083-4d32-9434-862314ed5e97
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6825b515f028046702283a2374de26100fd3015f
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155922"
---
# <a name="wbadmin-get-versions"></a>wbadmin get versions

Muestra detalles acerca de las copias de seguridad disponibles almacenadas en el equipo local o en otro equipo. Los detalles proporcionados para una copia de seguridad incluyen la hora de copia de seguridad, la ubicación de almacenamiento de copia de seguridad, el identificador de versión y el tipo de recuperaciones que se pueden realizar.

Para obtener detalles acerca de las copias de seguridad disponibles con este comando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

Si este comando se usa sin parámetros, enumera todas las copias de seguridad del equipo local, incluso si esas copias de seguridad no están disponibles.

## <a name="syntax"></a>Sintaxis

```
wbadmin get versions [-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}] [-machine:BackupMachineName]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -backupTarget | Especifica la ubicación de almacenamiento que contiene las copias de seguridad de las que desea obtener información. Se usa para enumerar las copias de seguridad almacenadas en esa ubicación de destino. Las ubicaciones de destino de copia de seguridad pueden ser unidades de disco conectadas localmente, volúmenes, carpetas compartidas remotas, medios extraíbles, como unidades de DVD u otros medios ópticos. Si este comando se ejecuta en el mismo equipo en el que se creó la copia de seguridad, este parámetro no es necesario. Sin embargo, este parámetro es necesario para obtener información acerca de una copia de seguridad creada desde otro equipo. |
| -equipo | Especifica el equipo para el que desea obtener detalles de la copia de seguridad. Se utiliza cuando las copias de seguridad de varios equipos se almacenan en la misma ubicación. Se debe usar cuando se especifica **-backupTarget** . |

## <a name="examples"></a>Ejemplos

Para ver una lista de las copias de seguridad disponibles almacenadas en el volumen H:, escriba:

```
wbadmin get versions -backupTarget:H:
```

Para ver una lista de las copias de seguridad disponibles almacenadas en la carpeta compartida remota del `\\<servername>\<share>` equipo Server01, escriba:

```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin get items](wbadmin-get-items.md)

- [Get-WBBackupTarget](/powershell/module/windowserverbackup/Get-WBBackupTarget)
