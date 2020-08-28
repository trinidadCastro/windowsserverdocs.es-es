---
title: wbadmin start sysrecovery
description: Artículo de referencia de Wbadmin Start sysrecovery, que realiza una recuperación del sistema (reconstrucción completa) mediante los parámetros especificados.
ms.topic: reference
ms.assetid: 95b8232f-7c42-452b-838e-15b0cf6faebe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c108636533b333224b925f22854622fff023abba
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031903"
---
# <a name="wbadmin-start-sysrecovery"></a>wbadmin start sysrecovery



Realiza una recuperación del sistema (reconstrucción completa) mediante los parámetros que especifique.

> [!NOTE]
> Este subcomando solo se puede ejecutar desde el entorno de recuperación de Windows y no se muestra de forma predeterminada en el texto de uso de **Wbadmin**. Para obtener más información, vea [información general sobre el entorno de recuperación de Windows (Windows re)](/previous-versions/windows/it-pro/windows-8.1-and-8/hh825173(v=win.10)).

Para realizar una recuperación del sistema con este subcomando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados.

## <a name="syntax"></a>Sintaxis

```
wbadmin start sysrecovery
-version:<VersionIdentifier>
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-restoreAllVolumes]
[-recreateDisks]
[-excludeDisks]
[-skipBadClusterCheck]
[-quiet]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-version|Especifica el identificador de la versión de la copia de seguridad que se va a recuperar en formato MM/DD/AAAA-HH: MM. Si no conoce el identificador de la versión, escriba **Wbadmin get Versions**.|
|-backupTarget|Especifica la ubicación de almacenamiento que contiene la copia o copias de seguridad que se desean recuperar. Este parámetro es útil cuando la ubicación de almacenamiento es diferente de donde se almacenan normalmente las copias de seguridad de este equipo.|
|-equipo|Especifica el nombre del equipo que desea recuperar. Este parámetro es útil cuando se ha realizado una copia de seguridad de varios equipos en la misma ubicación. Debe usarse cuando se especifica el parámetro **-backupTarget** .|
|-restoreAllVolumes|Recupera todos los volúmenes de la copia de seguridad seleccionada. Si no se especifica este parámetro, solo se recuperan los volúmenes críticos (volúmenes que contienen los componentes del sistema operativo y el estado del sistema). Este parámetro es útil cuando se necesita recuperar volúmenes no críticos durante la recuperación del sistema.|
|-recreateDisks|Recupera una configuración de disco al estado que tenía cuando se creó la copia de seguridad.</br>ADVERTENCIA: este parámetro elimina todos los datos de los volúmenes que hospedan componentes del sistema operativo. También puede eliminar los datos de los volúmenes de datos.|
|-excludeDisks|Solo es válido cuando se especifica con el parámetro **-recreateDisks** y debe ser una entrada como una lista delimitada por comas de identificadores de disco (como se muestra en la salida de los **discos de Wbadmin get**). Los discos excluidos no tienen particiones ni tienen formato. Este parámetro ayuda a conservar los datos en los discos que no desea modificar durante la operación de recuperación.|
|-skipBadClusterCheck|Omite la comprobación de los discos de recuperación para obtener información de clúster incorrecta. Si va a restaurar a un servidor o hardware alternativo, se recomienda que no use este parámetro. Puede ejecutar manualmente **CHKDSK/b** en los discos de recuperación en cualquier momento para comprobarlos en busca de clústeres defectuosos y, a continuación, actualizar la información del sistema de archivos en consecuencia.</br>ADVERTENCIA: hasta que ejecute **CHKDSK** como se describe, es posible que los clústeres incorrectos notificados en el sistema recuperado no sean precisos.|
|-quiet|Ejecuta el comando sin indicaciones al usuario.|

## <a name="examples"></a>Ejemplos

Para iniciar la recuperación de la información de la copia de seguridad que se ejecutó el 31 de marzo de 2013 a las 9:00 A.M., que se encuentra en la unidad d:, escriba:
```
wbadmin start sysrecovery -version:03/31/2013-09:00 -backupTarget:d:
```
Para iniciar la recuperación de la información de la copia de seguridad que se ejecutó el 30 de abril de 2013 a las 9:00 A.M., que se encuentra en la carpeta compartida \\ \\ servername\shared: para Server01, escriba:
```
wbadmin start sysrecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBBareMetalRecovery](/previous-versions/windows/it-pro/windows-8.1-and-8/hh825173(v=win.10))
