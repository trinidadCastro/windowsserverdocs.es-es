---
title: recuperación de Wbadmin start
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52381316-a0fa-459f-b6a6-01e31fb21612
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f24c9dfeb0ce87474e58d3bd2bce8b68e31cb63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823286"
---
# <a name="wbadmin-start-recovery"></a>recuperación de Wbadmin start



Se ejecuta una operación de recuperación según los parámetros que especifique.

Para llevar a cabo una recuperación con este subcomando, debe ser miembro de la **operadores de copia de seguridad** grupo o la **administradores** grupo, o bien debe haber sido delegar los permisos adecuados. Además, debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic en **iniciar**, haga clic en **símbolo**y, a continuación, haga clic en **ejecutar como administrador**.)

Para obtener ejemplos de cómo usar este subcomando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
wbadmin start recovery
-version:<VersionIdentifier>
-items:{<VolumesToRecover> | <AppsToRecover> | <FilesOrFoldersToRecover>}
-itemtype:{Volume | App | File}
[-backupTarget:{<VolumeHostingBackup> | <NetworkShareHostingBackup>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:{<TargetVolumeForRecovery> | <TargetPathForRecovery>}]
[-recursive]
[-overwrite:{Overwrite | CreateCopy | Skip}]
[-notRestoreAcl]
[-skipBadClusterCheck]
[-noRollForward]
[-quiet]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-versión|Especifica el identificador de versión de la copia de seguridad para recuperar en formato MM/DD/AAAA-formato hh: mm. Si no conoce el identificador de versión, escriba **wbadmin obtener versiones**.|
|-artículos|Especifica una lista delimitada por comas de los volúmenes, aplicaciones, archivos o carpetas para recuperar.</br>-If **- itemtype** es **volumen**, puede especificar solo un volumen, proporcionando la letra de unidad de volumen, punto de montaje o nombre de volumen basada en GUID.</br>-If **- itemtype** es **aplicación**, puede especificar una sola aplicación. Para recuperarse, la aplicación debe haber registrado con copias de seguridad de Windows Server. También puede usar el valor **ADIFM** para recuperar una instalación de Active Directory. Para obtener más información, vea comentarios de.</br>-If **- itemtype** es **archivo**, puede especificar los archivos o carpetas, pero deben formar parte del mismo volumen y deben estar en la misma carpeta primaria.|
|-itemtype|Especifica el tipo de elementos que desea recuperar. Debe ser **volumen**, **aplicación**, o **archivo**.|
|-backupTarget|Especifica la ubicación de almacenamiento que contiene la copia de seguridad que desea recuperar. Este parámetro es útil cuando la ubicación es diferente de donde normalmente se almacenan las copias de seguridad de este equipo.|
|-machine|Especifica el nombre del equipo que desea recuperar la copia de seguridad. Este parámetro es útil cuando se copiaron en varios equipos en la misma ubicación. Debe ser se usa cuando el **- backupTarget** se especifica el parámetro.|
|-recoveryTarget|Especifica la ubicación para restaurar. Este parámetro es útil si esta ubicación es diferente de la ubicación que se realizó anteriormente. También puede usarse para la restauración de volúmenes, archivos o aplicaciones. Si va a restaurar un volumen, puede especificar la letra de unidad de volumen del volumen alternativo. Si va a restaurar un archivo o una aplicación, puede especificar una ubicación alternativa de recuperación.|
|-recursivo|Válido solo cuando la recuperación de archivos. Recupera los archivos en las carpetas y todos los archivos subordinados a las carpetas especificadas. De forma predeterminada, se pueden recuperar sólo los archivos que se encuentran directamente en las carpetas especificadas.|
|-overwrite|Válido solo cuando la recuperación de archivos. Especifica la acción que se realizará cuando un archivo que se está recuperando ya existe en la misma ubicación.</br>-   **Omitir** hace copias de seguridad de Windows Server omitir el archivo existente y continuar con la recuperación del siguiente archivo.</br>-   **CreateCopy** hace copias de seguridad de Windows Server crear una copia del archivo existente para que no se modifica el archivo existente.</br>-   **Sobrescribir** hace copias de seguridad de Windows Server sobrescribir el archivo existente con el archivo desde la copia de seguridad.|
|-notRestoreAcl|Válido solo cuando la recuperación de archivos. Especifica las listas de control de acceso (ACL) de seguridad de los archivos que se va a recuperar de la copia de seguridad para restaurar no. De forma predeterminada, se restauran las ACL de seguridad (el valor predeterminado es **true)**. Si se usa este parámetro, las ACL de los archivos restaurados se heredará de la ubicación a la que se están restaurando los archivos.|
|-skipBadClusterCheck|Válido solo cuando la recuperación de volúmenes. Omite la comprobación de los discos que se va a recuperar para obtener información de clúster no válido. Si va a recuperar a un servidor alternativo o hardware, se recomienda que no use este parámetro. Puede ejecutar manualmente el comando **chkdsk /b** en estos discos en cualquier momento para buscar clústeres defectuosos y, a continuación, actualice la información del sistema de archivos.</br>Importante: Hasta que se ejecuta **chkdsk** tal como se describe, los clústeres defectuosos notificados en el sistema recuperado pueden no ser exacto.|
|-noRollForward|Válido solo cuando la recuperación de aplicaciones. Permite una recuperación en un momento anterior de una aplicación si se selecciona la versión más reciente de las copias de seguridad. Para otras versiones de la aplicación que no se realiza la recuperación en un momento anterior, más reciente como valor predeterminado.|
|-quiet|Se ejecuta el subcomando sin solicitudes para el usuario.|

## <a name="remarks"></a>Comentarios

-   Para ver una lista de elementos que están disponibles para la recuperación de una versión de copia de seguridad específica, use **wbadmin obtener elementos**. Si un volumen no tenía una letra de unidad o punto de montaje en el momento de la copia de seguridad, este subcomando devolvería un nombre basado en GUID de volumen que se debe usar para recuperar el volumen.
-   Cuando el **- itemtype** es **aplicación**, puede usar un valor de **ADIFM** para **-elemento** para realizar una instalación de la operación de medios para recuperar todos los datos relacionados necesarios para los servicios de dominio de Active Directory. **ADIFM** crea una copia de la base de datos de Active Directory, el registro y el estado de SYSVOL y, a continuación, guarda esta información en la ubicación especificada por **- recoveryTarget**. Utilice este parámetro solo cuando **- recoveryTarget** se especifica.

>     [!NOTE]
>     Before using **wbadmin** to perform an install from media operation, you should consider using the **ntdsutil** command because **ntdsutil** only copies the minimum amount of data needed, and it uses a more secure data transport method.

## <a name="BKMK_Examples"></a>Ejemplos

Para ejecutar una recuperación de la copia de seguridad 31 de marzo de 2013, realizadas a las 9:00 A.M., de volumen d:, escriba:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume -items:d:
```
Para ejecutar una recuperación a la unidad d de la copia de seguridad 31 de marzo de 2013, realizadas a las 9:00 A.M., del registro, escriba:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```
Para ejecutar una recuperación de la copia de seguridad 31 de marzo de 2013, realizadas a 9:00 A.M., de los d:\folder y las carpetas subordinadas a d:\folder, escriba:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:File -items:d:\folder -recursive
```
Para ejecutar una recuperación de la copia de seguridad desde el 31 de marzo de 2013, realizadas a 9:00 A.M., del volumen \\ \\? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\, tipo:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume 
-items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Para ejecutar una recuperación de la copia de seguridad desde el 30 de abril de 2013, realizadas a las 9:00 A.M., de la carpeta compartida \\ \\servername\share de server01, escriba:
```
wbadmin start recovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-WBFileRecovery](https://technet.microsoft.com/library/jj902457.aspx) cmdlet
-   [Start-WBHyperVRecovery](https://technet.microsoft.com/library/jj902463.aspx) cmdlet
-   [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet
-   [Start-WBVolumeRecovery](https://technet.microsoft.com/library/jj902470.aspx) cmdlet