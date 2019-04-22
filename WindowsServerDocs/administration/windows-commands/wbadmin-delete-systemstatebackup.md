---
title: Wbadmin delete systemstatebackup
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6801ca5985af626ccb7f6170fbcd6f8fc8305ba1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813156"
---
# <a name="wbadmin-delete-systemstatebackup"></a>Wbadmin delete systemstatebackup



Elimina las copias de seguridad del estado del sistema que especifique. Si el volumen especificado contiene copias de seguridad que no sea de copias de seguridad del estado del sistema del servidor local, no se eliminarán esas copias de seguridad.

> [!NOTE]
> Copia de seguridad de Windows Server no copia de seguridad o recuperar subárboles del registro de usuario (HKEY_CURRENT_USER) como parte de la copia de seguridad o recuperación del estado del sistema.

Para eliminar una copia de seguridad del estado del sistema con este subcomando, debe ser miembro de la **operadores de copia de seguridad** grupo o la **administradores** grupo, o bien debe haber sido delegar los permisos adecuados. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados de contextual **símbolo**y, a continuación, haga clic en **ejecutar como administrador**.)

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
> Deben especificarse uno y solo uno de estos parámetros: **- keepVersions**, **-versión**, o **- deleteOldest**.

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-keepVersions|Especifica el número de las copias de seguridad de estado del sistema más recientes para mantener. El valor debe ser un entero positivo. El valor del parámetro **- keepVersions: 0** elimina todas las copias de seguridad de estado del sistema.|
|-versión|Especifica el identificador de la versión de la copia de seguridad de MM/DD/AAAA-formato hh: mm. Si no conoce el identificador de versión, escriba **wbadmin obtener versiones**.</br>Las versiones que son exclusivamente sistema estado copias de seguridad pueden eliminarse mediante este comando. Use **wbadmin obtener elementos** para ver el tipo de versión.|
|-deleteOldest|Elimina la copia de seguridad del estado del sistema más antigua.|
|-backupTarget|Especifica la ubicación de almacenamiento para la copia de seguridad que desea eliminar. La ubicación de almacenamiento para copias de seguridad de discos puede ser una letra de unidad, un punto de montaje o una ruta de acceso basado en GUID de volumen. Este valor solo debe especificarse para la localización de las copias de seguridad que no son del equipo local. Información acerca de las copias de seguridad para el equipo local estará disponible en el catálogo de copia de seguridad en el equipo local.|
|-machine|Especifica el equipo cuya copia de seguridad del estado del sistema que desea eliminar. Resulta útil cuando varios equipos se incluyeron en la misma ubicación. Debe usarse cuando la **- backupTarget** se especifica el parámetro.|
|-quiet|Se ejecuta el subcomando sin solicitudes para el usuario.|

## <a name="BKMK_examples"></a>Ejemplos

Para eliminar la copia de seguridad del estado del sistema creado el 31 de marzo de 2013 a las 10:00 a. M., escriba:
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
Para eliminar todos los sistema estado copias de seguridad, excepto los tres últimos, escriba:
```
wbadmin delete systemstatebackup -keepVersions:3
```
Para eliminar la más antigua copia de seguridad almacenado en el disco f, escriba:
```
wbadmin delete systemstatebackup -backupTarget:f -deleteOldest
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)