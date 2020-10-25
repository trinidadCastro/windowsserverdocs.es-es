---
title: wbadmin start systemstaterecovery
description: Artículo de referencia para el comando Wbadmin Start systemstaterecovery, que realiza una recuperación del estado del sistema en una ubicación y, a partir de una copia de seguridad, que especifique.
ms.topic: reference
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b52b4fedf14eb2d4caab4e9d521767ff9e89365a
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524720"
---
# <a name="wbadmin-start-systemstaterecovery"></a>wbadmin start systemstaterecovery

Realiza una recuperación de estado del sistema en una ubicación y desde una copia de seguridad que especifique.

Para realizar una recuperación del estado del sistema mediante este comando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

> [!NOTE]
> Copias de seguridad de Windows Server no realiza una copia de seguridad ni recupera subárboles de usuario del registro (HKEY_CURRENT_USER) como parte de la copia de seguridad del estado del sistema o de la recuperación del estado del sistema.

## <a name="syntax"></a>Sintaxis

```
wbadmin start systemstaterecovery -version:<VersionIdentifier> -showsummary [-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>] [-recoveryTarget:<TargetPathForRecovery>] [-authsysvol] [-autoReboot] [-quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -version | Especifica el identificador de la versión de la copia de seguridad que se va a recuperar en formato MM/DD/AAAA-HH: MM. Si no conoce el identificador de la versión, ejecute el [comando Wbadmin get Versions](wbadmin-get-versions.md). |
| -showsummary | Informa del Resumen de la última recuperación del estado del sistema (después del reinicio necesario para finalizar la operación). Este parámetro no puede ir acompañado de otros parámetros. |
| -backupTarget | Especifica la ubicación de almacenamiento con las copias de seguridad que desea recuperar. Este parámetro es útil cuando la ubicación de almacenamiento es diferente de donde se almacenan normalmente las copias de seguridad. |
| -equipo | Especifica el nombre del equipo para el que se va a recuperar la copia de seguridad. Este parámetro debe usarse cuando se especifica el parámetro **-backupTarget** . El parámetro **-Machine** es útil cuando se ha realizado una copia de seguridad de varios equipos en la misma ubicación. |
| -recoveryTarget | Especifica el directorio en el que se va a restaurar. Este parámetro es útil si la copia de seguridad se restaura en una ubicación alternativa. |
| -authsysvol | Realiza una restauración autoritativa del directorio compartido del volumen del sistema (SYSVOL). |
| -autoReboot | Especifica que se reinicie el sistema al final de la operación de recuperación del estado del sistema. Este parámetro solo es válido para una recuperación en la ubicación original. No se recomienda usar este parámetro si necesita realizar pasos después de la operación de recuperación. |
| -quiet | Ejecuta el comando sin preguntar al usuario. |

## <a name="examples"></a>Ejemplos

Para iniciar una recuperación de estado del sistema de la copia de seguridad de 03/31/2020 a las 9:00 A.M., escriba:

```
wbadmin start systemstaterecovery -version:03/31/2020-09:00
```

Para iniciar una recuperación de estado del sistema de la copia de seguridad de 04/30/2020 a las 9:00 A.M. que está almacenado en el recurso compartido `\\servername\share` para Server01, escriba:

```
wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [Start-WBSystemStateRecovery](/powershell/module/windowserverbackup/Start-WBSystemStateRecovery)
