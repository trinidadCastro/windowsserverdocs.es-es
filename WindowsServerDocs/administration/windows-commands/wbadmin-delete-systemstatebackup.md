---
title: wbadmin delete systemstatebackup
description: Artículo de referencia para el comando Wbadmin Delete systemstatebackup, que elimina las copias de seguridad de estado del sistema que especifique.
ms.topic: reference
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 05ce1e2deb8a2466df70b56a43c4532871d491b9
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156071"
---
# <a name="wbadmin-delete-systemstatebackup"></a>wbadmin delete systemstatebackup

Elimina las copias de seguridad de estado del sistema que especifique. Si el volumen especificado contiene copias de seguridad distintas de las copias de seguridad de estado del sistema del servidor local, esas copias de seguridad no se eliminarán.

Para eliminar una copia de seguridad del estado del sistema mediante este comando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

> [!NOTE]
> Copias de seguridad de Windows Server no realiza copias de seguridad ni recupera subárboles de usuario del registro (HKEY_CURRENT_USER) como parte de la copia de seguridad del estado del sistema o de la recuperación del estado del sistema.

## <a name="syntax"></a>Sintaxis

```
wbadmin delete systemstatebackup {-keepVersions:<numberofcopies> | -version:<versionidentifier> | -deleteoldest} [-backupTarget:<volumename>] [-machine:<backupmachinename>] [-quiet]
```

> [!IMPORTANT]
> Solo debe especificar uno de estos parámetros: **-keepVersions**, **-version**o **-deleteOldest**.

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -keepVersions | Especifica el número de las copias de seguridad de estado del sistema más recientes que se van a conservar. El valor debe ser un entero positivo. El valor del parámetro **-keepversions: 0** elimina todas las copias de seguridad de estado del sistema. |
| -version | Especifica el identificador de la versión de la copia de seguridad en formato MM/DD/AAAA-HH: MM. Si no conoce el identificador de la versión, ejecute el comando [Wbadmin get Versions](wbadmin-get-versions.md) .<p>Las versiones compuestas de las copias de seguridad de estado del sistema exclusivamente se pueden eliminar con este comando. Ejecute el comando [Wbadmin get items](wbadmin-get-items.md) para ver el tipo de versión. |
| -deleteOldest | Elimina la copia de seguridad de estado del sistema más antigua. |
| -backupTarget | Especifica la ubicación de almacenamiento de la copia de seguridad que desea eliminar. La ubicación de almacenamiento de las copias de seguridad en disco puede ser una letra de unidad, un punto de montaje o una ruta de acceso de volumen basada en GUID. Este valor solo debe especificarse para buscar copias de seguridad que no estén en el equipo local. La información sobre las copias de seguridad del equipo local está disponible en el catálogo de copias de seguridad del equipo local. |
| -equipo | Especifica el equipo cuya copia de seguridad de estado del sistema desea eliminar. Resulta útil cuando se realizó una copia de seguridad de varios equipos en la misma ubicación. Debe usarse cuando se especifica el parámetro **-backupTarget** . |
| -quiet | Ejecuta el comando sin preguntar al usuario. |

## <a name="examples"></a>Ejemplos

Para eliminar la copia de seguridad del estado del sistema creada el 31 de marzo de 2013 a las 10:00 AM, escriba:

```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```

Para eliminar todas las copias de seguridad de estado del sistema, excepto las tres más recientes, escriba:

```
wbadmin delete systemstatebackup -keepVersions:3
```

Para eliminar la copia de seguridad de estado del sistema más antigua almacenada en el disco f:, escriba:

```
wbadmin delete systemstatebackup -backupTarget:f:\ -deleteOldest
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin get Versions](wbadmin-get-versions.md)

- [comando Wbadmin get items](wbadmin-get-items.md)
