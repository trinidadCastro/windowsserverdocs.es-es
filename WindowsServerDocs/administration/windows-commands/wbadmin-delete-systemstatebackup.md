---
title: Wbadmin Delete systemstatebackup
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f324cba3fcdae8639009395c4df734a2db6b814
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362523"
---
# <a name="wbadmin-delete-systemstatebackup"></a>Wbadmin Delete systemstatebackup



Elimina las copias de seguridad de estado del sistema que especifique. Si el volumen especificado contiene copias de seguridad distintas de las copias de seguridad de estado del sistema del servidor local, esas copias de seguridad no se eliminarán.

> [!NOTE]
> Copias de seguridad de Windows Server no realiza copias de seguridad ni recupera secciones de usuarios del registro (HKEY_CURRENT_USER) como parte de la copia de seguridad del estado del sistema o de la recuperación del estado del sistema.

Para eliminar una copia de seguridad de estado del sistema con este subcomando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

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

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-keepVersions|Especifica el número de las copias de seguridad de estado del sistema más recientes que se van a conservar. El valor debe ser un entero positivo. El valor del parámetro **-keepVersions: 0** elimina todas las copias de seguridad de estado del sistema.|
|-versión|Especifica el identificador de la versión de la copia de seguridad en formato MM/DD/AAAA-HH: MM. Si no conoce el identificador de la versión, escriba **Wbadmin get Versions**.</br>Las versiones que son exclusivamente copias de seguridad de estado del sistema se pueden eliminar con este comando. Use **Wbadmin get items** para ver el tipo de versión.|
|-deleteOldest|Elimina la copia de seguridad de estado del sistema más antigua.|
|-backupTarget|Especifica la ubicación de almacenamiento de la copia de seguridad que desea eliminar. La ubicación de almacenamiento de las copias de seguridad de los discos puede ser una letra de unidad, un punto de montaje o una ruta de acceso de volumen basada en GUID. Este valor solo debe especificarse para buscar copias de seguridad que no sean del equipo local. La información sobre las copias de seguridad del equipo local estará disponible en el catálogo de copias de seguridad del equipo local.|
|-equipo|Especifica el equipo cuya copia de seguridad de estado del sistema desea eliminar. Resulta útil cuando se realizó una copia de seguridad de varios equipos en la misma ubicación. Debe usarse cuando se especifica el parámetro **-backupTarget** .|
|-quiet|Ejecuta el subcomando sin preguntar al usuario.|

## <a name="BKMK_examples"></a>Example

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

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)