---
title: recuperación de inicio de Wbadmin
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: edb287573dc76619502faf58018f48c464140629
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362352"
---
# <a name="wbadmin-start-recovery"></a>recuperación de inicio de Wbadmin



Ejecuta una operación de recuperación basada en los parámetros que se especifiquen.

Para realizar una recuperación con este subcomando, debe ser miembro del grupo operadores de **copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados. Además, debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic en **Inicio**, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).

Para obtener ejemplos de cómo usar este subcomando, vea [ejemplos](#BKMK_Examples).

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
|-versión|Especifica el identificador de la versión de la copia de seguridad que se va a recuperar en formato MM/DD/AAAA-HH: MM. Si no conoce el identificador de la versión, escriba **Wbadmin get Versions**.|
|-elementos|Especifica una lista delimitada por comas de volúmenes, aplicaciones, archivos o carpetas que se van a recuperar.</br>-Si **-itemType** es un **volumen**, solo puede especificar un único volumen; para ello, proporcione la letra de unidad del volumen, el punto de montaje del volumen o el nombre del volumen basado en GUID.</br>-Si **-itemType** es una **aplicación**, puede especificar una sola aplicación. Para poder recuperarse, la aplicación debe estar registrada con Copias de seguridad de Windows Server. También puede usar el valor **ADIFM** para recuperar una instalación de Active Directory. Vea la sección Comentarios en para obtener más información.</br>-Si **-itemType** es un **archivo**, puede especificar archivos o carpetas, pero deben formar parte del mismo volumen y deben estar en la misma carpeta principal.|
|-ItemType|Especifica el tipo de elementos que se van a recuperar. Debe ser **volumen**, **aplicación**o **archivo**.|
|-backupTarget|Especifica la ubicación de almacenamiento que contiene la copia de seguridad que desea recuperar. Este parámetro es útil cuando la ubicación es diferente de donde se almacenan normalmente las copias de seguridad de este equipo.|
|-equipo|Especifica el nombre del equipo para el que desea recuperar la copia de seguridad. Este parámetro es útil cuando se ha realizado una copia de seguridad de varios equipos en la misma ubicación. Se debe usar cuando se especifica el parámetro **-backupTarget** .|
|-recoveryTarget|Especifica la ubicación en la que se va a restaurar. Este parámetro es útil si esta ubicación es diferente de la ubicación de la que se realizó previamente una copia de seguridad. También se puede usar para las restauraciones de volúmenes, archivos o aplicaciones. Si va a restaurar un volumen, puede especificar la letra de unidad del volumen alternativo. Si va a restaurar un archivo o una aplicación, puede especificar una ubicación de recuperación alternativa.|
|-recursivo|Solo es válido cuando se recuperan archivos. Recupera los archivos de las carpetas y todos los archivos subordinados a las carpetas especificadas. De forma predeterminada, solo se recuperan los archivos que residen directamente en las carpetas especificadas.|
|-overwrite|Solo es válido cuando se recuperan archivos. Especifica la acción que debe llevarse a cabo cuando un archivo que se va a recuperar ya existe en la misma ubicación.</br>-   **SKIP** hace que copias de seguridad de Windows Server omita el archivo existente y continúe con la recuperación del siguiente archivo.</br>-   **CreateCopy** hace que copias de seguridad de Windows Server cree una copia del archivo existente para que no se modifique el archivo existente.</br>-   **overwrite** hace que copias de seguridad de Windows Server sobrescriba el archivo existente con el archivo de la copia de seguridad.|
|-notRestoreAcl|Solo es válido cuando se recuperan archivos. Especifica que no se restauren las listas de control de acceso (ACL) de seguridad de los archivos que se recuperan de la copia de seguridad. De forma predeterminada, se restauran las ACL de seguridad (el valor predeterminado es **true)** . Si se usa este parámetro, las ACL de los archivos restaurados se heredarán de la ubicación en la que se restauran los archivos.|
|-skipBadClusterCheck|Solo es válido cuando se recuperan volúmenes. Omite la comprobación de los discos a los que se va a recuperar para obtener información de clúster incorrecta. Si va a realizar la recuperación en un servidor o hardware alternativo, se recomienda que no utilice este parámetro. Puede ejecutar manualmente el comando **CHKDSK/b** en estos discos en cualquier momento para comprobarlos en busca de clústeres defectuosos y, a continuación, actualizar la información del sistema de archivos en consecuencia.</br>Importante: Hasta que ejecute **CHKDSK** tal y como se describe, es posible que los clústeres incorrectos notificados en el sistema recuperado no sean precisos.|
|-noRollForward|Solo es válido cuando se recuperan aplicaciones. Permite la recuperación en un momento dado anterior de una aplicación si se selecciona la versión más reciente de las copias de seguridad. En el caso de otras versiones de la aplicación que no son la última recuperación a un momento dado anterior, se realiza como valor predeterminado.|
|-quiet|Ejecuta el subcomando sin preguntar al usuario.|

## <a name="remarks"></a>Comentarios

-   Para ver una lista de los elementos que están disponibles para la recuperación a partir de una versión de copia de seguridad específica, use **Wbadmin get items**. Si un volumen no tenía un punto de montaje o una letra de unidad en el momento de la copia de seguridad, este subcomando devolvería un nombre de volumen basado en GUID que se debe usar para recuperar el volumen.
-   Cuando **-itemType** es una **aplicación**, puede usar un valor de **ADIFM** para que realice una instalación desde el **medio de la** operación para recuperar todos los datos relacionados necesarios para Active Directory Domain Services. **ADIFM** crea una copia de la base de datos Active Directory, el registro y el estado de SYSVOL y, a continuación, guarda esta información en la ubicación especificada por **-recoveryTarget**. Use este parámetro solo cuando se especifica **-recoveryTarget** .

>     [!NOTE]
>     Before using **wbadmin** to perform an install from media operation, you should consider using the **ntdsutil** command because **ntdsutil** only copies the minimum amount of data needed, and it uses a more secure data transport method.

## <a name="BKMK_Examples"></a>Example

Para ejecutar una recuperación de la copia de seguridad del 31 de marzo de 2013, tomada a las 9:00 A.M., del volumen d:, escriba:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume -items:d:
```
Para ejecutar una recuperación en la unidad d de la copia de seguridad del 31 de marzo de 2013, tomada a las 9:00 A.M. del registro, escriba:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```
Para ejecutar una recuperación de la copia de seguridad desde el 31 de marzo de 2013, tomada a las 9:00 A.M., del d:\folder y de las carpetas subordinadas a d:\folder, escriba:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:File -items:d:\folder -recursive
```
Para ejecutar una recuperación de la copia de seguridad del 31 de marzo de 2013, tomada a las 9:00 A.M., del volumen \\ @ no__t-1? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963} \,:
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume 
-items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Para ejecutar una recuperación de la copia de seguridad del 30 de abril de 2013, tomada a las 9:00 A.M., de la carpeta compartida \\ @ no__t-1servername\share de Server01, escriba:
```
wbadmin start recovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Start-WBFileRecovery](https://technet.microsoft.com/library/jj902457.aspx)
-   Cmdlet [Start-WBHyperVRecovery](https://technet.microsoft.com/library/jj902463.aspx)
-   Cmdlet [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx)
-   Cmdlet [Start-WBVolumeRecovery](https://technet.microsoft.com/library/jj902470.aspx)