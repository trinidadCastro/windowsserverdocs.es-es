---
title: wbadmin start recovery
description: Artículo de referencia del comando Wbadmin Start recovery, que ejecuta una operación de recuperación basada en los parámetros especificados.
ms.topic: reference
ms.assetid: 52381316-a0fa-459f-b6a6-01e31fb21612
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 408ccadd830e50e9b51447cc19f7243d349cb3ef
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524761"
---
# <a name="wbadmin-start-recovery"></a>wbadmin start recovery

Ejecuta una operación de recuperación basada en los parámetros que se especifiquen.

Para realizar una recuperación mediante este comando, debe ser miembro del grupo operadores de **copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados, haciendo clic con el botón secundario en **símbolo del sistema**y, a continuación, seleccionando **Ejecutar como administrador**.

## <a name="syntax"></a>Sintaxis

```
wbadmin start recovery -version:<VersionIdentifier> -items:{<VolumesToRecover> | <AppsToRecover> | <FilesOrFoldersToRecover>} -itemtype:{Volume | App | File} [-backupTarget:{<VolumeHostingBackup> | <NetworkShareHostingBackup>}] [-machine:<BackupMachineName>] [-recoveryTarget:{<TargetVolumeForRecovery> | <TargetPathForRecovery>}] [-recursive] [-overwrite:{Overwrite | CreateCopy | Skip}] [-notRestoreAcl] [-skipBadClusterCheck] [-noRollForward] [-quiet]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -version | Especifica el identificador de la versión de la copia de seguridad que se va a recuperar en formato MM/DD/AAAA-HH: MM. Si no conoce el identificador de la versión, ejecute el [comando Wbadmin get Versions](wbadmin-get-versions.md). |
| -elementos | Especifica una lista delimitada por comas de volúmenes, aplicaciones, archivos o carpetas que se van a recuperar. Debe usar este parámetro con el parámetro **-itemType** . |
| -ItemType | Especifica el tipo de elementos que se van a recuperar. Debe ser **volumen**, **aplicación**o **archivo**. Si el *volumen* **de-ItemType** es, puede especificar un solo volumen, proporcionando la letra de unidad del volumen, el punto de montaje del volumen o el nombre de volumen basado en GUID. Si **-itemType** es una *aplicación*, puede especificar una sola aplicación o puede usar el valor **ADIFM** para recuperar una instalación de Active Directory. Para poder recuperarse, la aplicación debe estar registrada en Copias de seguridad de Windows Server. Si el *archivo* **-itemType** es, puede especificar archivos o carpetas, pero deben formar parte del mismo volumen y deben estar en la misma carpeta principal. |
| -backupTarget | Especifica la ubicación de almacenamiento que contiene la copia de seguridad que desea recuperar. Este parámetro es útil cuando la ubicación es diferente de donde se almacenan normalmente las copias de seguridad de este equipo. |
| -equipo | Especifica el nombre del equipo para el que desea recuperar la copia de seguridad. Este parámetro debe usarse cuando se especifica el parámetro **-backupTarget** . El parámetro **-Machine** es útil cuando se ha realizado una copia de seguridad de varios equipos en la misma ubicación. |
| -recoveryTarget | Especifica la ubicación en la que se va a restaurar. Este parámetro es útil si esta ubicación es diferente de la ubicación de la que se realizó previamente una copia de seguridad. También se puede usar para las restauraciones de volúmenes, archivos o aplicaciones. Si va a restaurar un volumen, puede especificar la letra de unidad del volumen alternativo. Si va a restaurar un archivo o una aplicación, puede especificar una ubicación de recuperación alternativa. |
| -recursivo | Solo es válido cuando se recuperan archivos. Recupera los archivos de las carpetas y todos los archivos subordinados a las carpetas especificadas. De forma predeterminada, solo se recuperan los archivos que residen directamente en las carpetas especificadas. |
|-overwrite | Solo es válido cuando se recuperan archivos. Especifica la acción que debe llevarse a cabo cuando un archivo que se va a recuperar ya existe en la misma ubicación. Las opciones válidas son:<ul><li>**SKIP** : hace que copias de seguridad de Windows Server omita el archivo existente y continúe con la recuperación del siguiente archivo.</li><li>**CreateCopy** : hace que copias de seguridad de Windows Server cree una copia del archivo existente para que no se modifique el archivo existente.</li><li>**Overwrite** : hace que copias de seguridad de Windows Server sobrescriba el archivo existente con el archivo de la copia de seguridad. |
| -notRestoreAcl | Solo es válido cuando se recuperan archivos. Especifica que no se restauren las listas de control de acceso (ACL) de seguridad de los archivos que se recuperan de la copia de seguridad. De forma predeterminada, se restauran las ACL de seguridad (el valor predeterminado es **true**). Si se usa este parámetro, las ACL de los archivos restaurados se heredarán de la ubicación en la que se restauran los archivos. |
| -skipBadClusterCheck | Solo es válido cuando se recuperan volúmenes. Omite la comprobación de los discos a los que se va a recuperar para obtener información de clúster incorrecta. Si va a efectuar la recuperación en un servidor o hardware alternativo, se recomienda no usar este parámetro. Puede ejecutar manualmente el comando **CHKDSK/b** en estos discos en cualquier momento para comprobarlos en busca de clústeres defectuosos y, a continuación, actualizar la información del sistema de archivos en consecuencia.<p>**Importante:** Hasta que ejecute **CHKDSK/b**, es posible que los clústeres incorrectos notificados en el sistema recuperado no sean precisos. |
| -noRollForward | Solo es válido cuando se recuperan aplicaciones. Permite la recuperación en un momento dado anterior de una aplicación si se selecciona la versión más reciente de las copias de seguridad. La recuperación a un momento dado anterior se realiza como valor predeterminado para todas las demás versiones no más recientes de la aplicación. |
| -quiet | Ejecuta el comando sin preguntar al usuario. |

#### <a name="remarks"></a>Comentarios

- Para ver una lista de los elementos disponibles para su recuperación desde una versión de copia de seguridad específica, ejecute el [comando Wbadmin get items](wbadmin-get-items.md). Si un volumen no tiene un punto de montaje o una letra de unidad en el momento de la copia de seguridad, este comando devuelve un nombre de volumen basado en GUID que se debe usar para recuperar el volumen.

- Si usa un valor de **ADIFM** para realizar una instalación desde el medio de la operación para recuperar los datos relacionados necesarios para Active Directory Domain Services, **ADIFM** crea una copia del Active Directory base de datos, el registro y el estado de SYSVOL y, a continuación, guarda esta información en la ubicación especificada por **-recoveryTarget**. Use este parámetro solo cuando se especifica **-recoveryTarget** .

## <a name="examples"></a>Ejemplos

Para ejecutar una recuperación de la copia de seguridad del 31 de marzo de 2020, tomada a las 9:00 A.M., del volumen d:, escriba:

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:Volume -items:d:
```

Para ejecutar una recuperación en la unidad d de la copia de seguridad del 31 de marzo de 2020, tomada a las 9:00 A.M. del registro, escriba:

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```

Para ejecutar una recuperación de la copia de seguridad desde el 31 de marzo de 2020, tomada a las 9:00 A.M., del d:\folder y de las carpetas subordinadas a d:\folder, escriba:

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:File -items:d:\folder -recursive
```

Para ejecutar una recuperación de la copia de seguridad desde el 31 de marzo de 2020, tomada a las 9:00 A.M. del volumen `\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\` , escriba:

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:Volume -items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```

Para ejecutar una recuperación de la copia de seguridad del 30 de abril de 2020, tomada a las 9:00 A.M., de la carpeta compartida `\\servername\share` de Server01, escriba:

```
wbadmin start recovery -version:04/30/2020-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [Start-WBFileRecovery](/powershell/module/windowserverbackup/Start-WBFileRecovery)

- [Start-WBHyperVRecovery](/powershell/module/windowserverbackup/Start-WBHyperVRecovery)

- [Start-WBSystemStateRecovery](/powershell/module/windowserverbackup/Start-WBSystemStateRecovery)

- [Start-WBVolumeRecovery](/powershell/module/windowserverbackup/Start-WBVolumeRecovery)
