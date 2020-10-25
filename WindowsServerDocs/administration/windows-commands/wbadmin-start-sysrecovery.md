---
title: wbadmin start sysrecovery
description: Artículo de referencia para el comando Wbadmin Start sysrecovery, que realiza una recuperación del sistema (reconstrucción completa) con los parámetros especificados.
ms.topic: reference
ms.assetid: 95b8232f-7c42-452b-838e-15b0cf6faebe
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 292c0d6fc42f4fe58dda6a6fbbb6a693b70a8756
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524750"
---
# <a name="wbadmin-start-sysrecovery"></a>wbadmin start sysrecovery

Realiza una recuperación del sistema (reconstrucción completa) con los parámetros especificados.

Para realizar una recuperación del sistema con este comando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados.

> [!IMPORTANT]
> El comando **Wbadmin Start sysrecovery** se debe ejecutar desde la consola de recuperación de Windows y no aparece en el texto de uso predeterminado de la herramienta **Wbadmin** . Para obtener más información, vea [entorno de recuperación de Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxis

```
wbadmin start sysrecovery -version:<VersionIdentifier> -backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}  [-machine:<BackupMachineName>] [-restoreAllVolumes] [-recreateDisks] [-excludeDisks] [-skipBadClusterCheck] [-quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -version | Especifica el identificador de la versión de la copia de seguridad que se va a recuperar en formato MM/DD/AAAA-HH: MM. Si no conoce el identificador de la versión, ejecute el [comando Wbadmin get Versions](wbadmin-get-versions.md). |
| -backupTarget | Especifica la ubicación de almacenamiento que contiene las copias de seguridad que desea recuperar. Este parámetro es útil cuando la ubicación de almacenamiento es diferente de donde se almacenan normalmente las copias de seguridad de este equipo. |
| -equipo | Especifica el nombre del equipo para el que desea recuperar la copia de seguridad. Este parámetro debe usarse cuando se especifica el parámetro **-backupTarget** . El parámetro **-Machine** es útil cuando se ha realizado una copia de seguridad de varios equipos en la misma ubicación. |
| -restoreAllVolumes | Recupera todos los volúmenes de la copia de seguridad seleccionada. Si no se especifica este parámetro, solo se recuperan los volúmenes críticos (volúmenes que contienen los componentes del sistema operativo y el estado del sistema). Este parámetro es útil cuando se necesita recuperar volúmenes no críticos durante la recuperación del sistema. |
| -recreateDisks | Recupera una configuración de disco al estado que tenía cuando se creó la copia de seguridad.<p>**ADVERTENCIA:** Este parámetro elimina todos los datos de los volúmenes que hospedan componentes del sistema operativo. También puede eliminar los datos de los volúmenes de datos. |
| -excludeDisks | Solo es válido cuando se especifica con el parámetro **-recreateDisks** y debe ser una entrada como una lista delimitada por comas de identificadores de disco (como se muestra en la salida del [comando Wbadmin get Disks](wbadmin-get-disks.md)). Los discos excluidos no tienen particiones ni tienen formato. Este parámetro ayuda a conservar los datos en los discos que no desea modificar durante la operación de recuperación. |
| -skipBadClusterCheck | Solo es válido cuando se recuperan volúmenes. Omite la comprobación de los discos a los que se va a recuperar para obtener información de clúster incorrecta. Si va a efectuar la recuperación en un servidor o hardware alternativo, se recomienda no usar este parámetro. Puede ejecutar manualmente el comando **CHKDSK/b** en estos discos en cualquier momento para comprobarlos en busca de clústeres defectuosos y, a continuación, actualizar la información del sistema de archivos en consecuencia.<p>**Importante:** Hasta que ejecute **CHKDSK/b**, es posible que los clústeres incorrectos notificados en el sistema recuperado no sean precisos. |
| -quiet | Ejecuta el comando sin preguntar al usuario. |

## <a name="examples"></a>Ejemplos

Para iniciar la recuperación de la información de la copia de seguridad que se ejecutó el 31 de marzo de 2020 a las 9:00 A.M., que se encuentra en la unidad d:, escriba:

```
wbadmin start sysrecovery -version:03/31/2020-09:00 -backupTarget:d:
```

Para iniciar la recuperación de la información de la copia de seguridad que se ejecutó el 30 de abril de 2020 a las 9:00 A.M., que se encuentra en la carpeta compartida `\\servername\share` de Server01, escriba:

```
wbadmin start sysrecovery -version:04/30/2020-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [Get-WBBareMetalRecovery](/powershell/module/windowserverbackup/Get-WBBareMetalRecovery)
