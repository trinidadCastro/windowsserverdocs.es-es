---
title: Wbadmin start systemstaterecovery
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c99a934987e320baaec0e56c69f36eda5a32819
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852686"
---
# <a name="wbadmin-start-systemstaterecovery"></a>Wbadmin start systemstaterecovery



Realiza una recuperación del estado del sistema en una ubicación y una copia de seguridad que especifique.

> [!NOTE]
> Copia de seguridad de Windows Server no copia de seguridad o recuperar subárboles del registro de usuario (HKEY_CURRENT_USER) como parte de la copia de seguridad o recuperación del estado del sistema.

Para llevar a cabo una recuperación del estado del sistema con este subcomando, debe ser miembro de la **operadores de copia de seguridad** grupo o la **administradores** grupo, o bien debe haber sido delegar los permisos adecuados. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados de contextual **símbolo**y, a continuación, haga clic en **ejecutar como administrador**.)

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

Sintaxis para Windows Server 2008:
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-quiet]
```
Sintaxis para Windows Server 2008 R2 o posterior:
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-autoReboot]
[-quiet]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-versión|Especifica el identificador de versión de la copia de seguridad recuperar en formato MM/DD/AAAA-formato hh: mm. Si no conoce el identificador de versión, escriba **wbadmin obtener versiones**.|
|-showsummary|Proporciona el resumen de la última recuperación de estado del sistema (tras el reinicio necesario para completar la operación). Este parámetro no puede ir acompañado de otros parámetros.|
|-backupTarget|Especifica la ubicación de almacenamiento que contiene la copia de seguridad o las copias de seguridad que desea recuperar. Este parámetro es útil cuando la ubicación de almacenamiento es diferente de donde normalmente se almacenan las copias de seguridad de este equipo.|
|-machine|Especifica el nombre del equipo que desea recuperar. Este parámetro es útil cuando se copiaron en varios equipos en la misma ubicación. Debe usarse cuando la **- backupTarget** se especifica el parámetro.|
|-recoveryTarget|Especifica el directorio para restaurar. Este parámetro es útil si se restaura la copia de seguridad en una ubicación alternativa.|
|-authsysvol|Si usa, realiza una restauración autoritativa de SYSVOL (el directorio compartido del volumen del sistema).|
|-autoReboot|Especifica que se reinicie el sistema al final de la operación de recuperación del estado del sistema. Este parámetro es válido solo para una recuperación en ubicación original. No se recomienda que usar este parámetro si tiene que realizar pasos después de la operación de recuperación.|
|-quiet|Se ejecuta el subcomando sin solicitudes para el usuario.|

## <a name="BKMK_examples"></a>Ejemplos

-   Para realizar una recuperación del estado del sistema de la copia de seguridad desde el 31/03/2013 a las 9:00 A.M., escriba:  
    ```
    wbadmin start systemstaterecovery -version:03/31/2013-09:00
    ```  
-   Para realizar una recuperación del estado del sistema de la copia de seguridad desde el 30/04/2013 a las 9:00 A.M. que se almacena en el recurso compartido \\ \\servername\share para server01, escriba:  
    ```
    wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
    ```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet