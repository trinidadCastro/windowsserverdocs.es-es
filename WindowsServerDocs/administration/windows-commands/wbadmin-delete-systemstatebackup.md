---
title: Wbadmin Delete systemstatebackup
description: Tema de referencia de Wbadmin Delete systemstatebackup, que elimina las copias de seguridad de estado del sistema que especifique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a84112191ad1b5873ad09c467fb3668107f2b24e
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821435"
---
# <a name="wbadmin-delete-systemstatebackup"></a>Wbadmin Delete systemstatebackup



Elimina las copias de seguridad de estado del sistema que especifique. Si el volumen especificado contiene copias de seguridad distintas de las copias de seguridad de estado del sistema del servidor local, esas copias de seguridad no se eliminarán.

> [!NOTE]
> Copias de seguridad de Windows Server no realiza copias de seguridad ni recupera subárboles de usuario del registro (HKEY_CURRENT_USER) como parte de la copia de seguridad del estado del sistema o de la recuperación del estado del sistema.

Para eliminar una copia de seguridad de estado del sistema con este subcomando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).



## <a name="syntax"></a>Sintaxis

```
wbadmin delete systemstatebackup
{-keepVersions:<NumberofCopies> | -version:<VersionIdentifier> | -deleteOldest}
[-backupTarget:<VolumeName>]
[-machine:<BackupMachineName>]
[-quiet]
```

> [!IMPORTANT]
> Se debe especificar uno y solo uno de estos parámetros: **-keepVersions**, **-version**o **-deleteOldest**.

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-keepVersions|Especifica el número de las copias de seguridad de estado del sistema más recientes que se van a conservar. El valor debe ser un entero positivo. El valor del parámetro **-keepVersions: 0** elimina todas las copias de seguridad de estado del sistema.|
|-version|Especifica el identificador de la versión de la copia de seguridad en formato MM/DD/AAAA-HH: MM. Si no conoce el identificador de la versión, escriba **Wbadmin get Versions**.</br>Las versiones que son exclusivamente copias de seguridad de estado del sistema se pueden eliminar con este comando. Use **Wbadmin get items** para ver el tipo de versión.|
|-deleteOldest|Elimina la copia de seguridad de estado del sistema más antigua.|
|-backupTarget|Especifica la ubicación de almacenamiento de la copia de seguridad que desea eliminar. La ubicación de almacenamiento de las copias de seguridad de los discos puede ser una letra de unidad, un punto de montaje o una ruta de acceso de volumen basada en GUID. Este valor solo debe especificarse para buscar copias de seguridad que no sean del equipo local. La información sobre las copias de seguridad del equipo local estará disponible en el catálogo de copias de seguridad del equipo local.|
|-equipo|Especifica el equipo cuya copia de seguridad de estado del sistema desea eliminar. Resulta útil cuando se realizó una copia de seguridad de varios equipos en la misma ubicación. Debe usarse cuando se especifica el parámetro **-backupTarget** .|
|-quiet|Ejecuta el subcomando sin preguntar al usuario.|

## <a name="examples"></a>Ejemplos

Para eliminar la copia de seguridad del estado del sistema creada el 31 de marzo de 2013 a las 10:00 AM, escriba:
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
Para eliminar todas las copias de seguridad de estado del sistema, excepto las tres más recientes, escriba:
```
wbadmin delete systemstatebackup -keepVersions:3
```
Para eliminar la copia de seguridad de estado del sistema más antigua almacenada en el disco f, escriba:
```
wbadmin delete systemstatebackup -backupTarget:f -deleteOldest
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)