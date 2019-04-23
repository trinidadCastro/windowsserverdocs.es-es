---
title: Wbadmin start sysrecovery
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95b8232f-7c42-452b-838e-15b0cf6faebe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8e0ff114d09d70b9e50e8c4ea6af6330c74128c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847166"
---
# <a name="wbadmin-start-sysrecovery"></a>Wbadmin start sysrecovery



Realiza una recuperación del sistema (recuperación) con los parámetros que especifique.

> [!NOTE]
> Se puede ejecutar este subcomando solo desde el entorno de recuperación de Windows y no aparece de forma predeterminada en el texto sobre el uso de **Wbadmin**. Para obtener más información, consulte [Introducción al entorno de recuperación de Windows (Windows RE)](https://technet.microsoft.com/library/hh825173.aspx).

Para llevar a cabo una recuperación del sistema con este subcomando, debe ser miembro de la **operadores de copia de seguridad** grupo o la **administradores** grupo, o bien debe haber sido delegar los permisos adecuados.

Para obtener ejemplos de cómo usar este subcomando, consulte [ejemplos](#BKMK_examples).

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

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-versión|Especifica el identificador de versión de la copia de seguridad recuperar en formato MM/DD/AAAA-formato hh: mm. Si no conoce el identificador de versión, escriba **wbadmin obtener versiones**.|
|-backupTarget|Especifica la ubicación de almacenamiento que contiene la copia de seguridad o las copias de seguridad que desea recuperar. Este parámetro es útil cuando la ubicación de almacenamiento es diferente de donde normalmente se almacenan las copias de seguridad de este equipo.|
|-machine|Especifica el nombre del equipo que desea recuperar. Este parámetro es útil cuando se copiaron en varios equipos en la misma ubicación. Debe usarse cuando la **- backupTarget** se especifica el parámetro.|
|-restoreAllVolumes|Recupera todos los volúmenes de la copia de seguridad seleccionada. Si no se especifica este parámetro, se recuperan solo críticos (volúmenes que contienen los componentes del sistema de estado y el sistema operativo). Este parámetro es útil cuando necesita recuperar volúmenes no críticos durante la recuperación del sistema.|
|-recreateDisks|Recupera una configuración de disco al estado que existía cuando se creó la copia de seguridad.</br>Advertencia: Este parámetro elimina todos los datos en volúmenes que componentes del sistema operativo host. También pueden eliminar datos de los volúmenes de datos.|
|-excludeDisks|Válido solo cuando se especifica con el **- recreateDisks** parámetro y debe especificarse como una lista delimitada por comas de identificadores de disco (como se muestra en la salida de **wbadmin obtener discos**). Excluidos los discos no son particiones o con formato. Este parámetro le ayuda a conservar los datos en los discos que no desea modificar durante la operación de recuperación.|
|-skipBadClusterCheck|Omite la comprobación de los discos de recuperación para obtener información de clúster no válido. Si va a restaurar a un servidor alternativo o hardware, se recomienda que no use este parámetro. Puede ejecutar manualmente **chkdsk /b** en los discos de recuperación en cualquier momento para buscar clústeres defectuosos y, a continuación, actualice la información del sistema de archivos.</br>Advertencia: Hasta que se ejecuta **chkdsk** tal como se describe, los clústeres defectuosos notificados en el sistema recuperado pueden no ser exacto.|
|-quiet|Ejecuta el comando sin solicitudes para el usuario.|

## <a name="BKMK_examples"></a>Ejemplos

Para iniciar la recuperación de la información de la copia de seguridad que se ejecutó en el 31 de marzo de 2013 a las 9:00 A.M., ubicado en la unidad d:, tipo:
```
wbadmin start sysrecovery -version:03/31/2013-09:00 -backupTarget:d:
```
Para iniciar la recuperación de la información de la copia de seguridad que se ejecutó en el 30 de abril de 2013 a las 9:00 A.M., ubicado en la carpeta compartida \\ \\servername\shared: para server01, escriba:
```
wbadmin start sysrecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBareMetalRecovery](https://technet.microsoft.com/library/jj902461.aspx) cmdlet