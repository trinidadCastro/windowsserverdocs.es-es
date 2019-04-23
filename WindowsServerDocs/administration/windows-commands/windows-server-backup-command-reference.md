---
title: Referencia de comandos de copia de seguridad de Windows Server
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03de0a65-21f0-4dd7-a3ae-251c98bbf6eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ded5039e122832c95eda864bcdcc76f580ca7108
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839816"
---
# <a name="windows-server-backup-command-reference"></a>Referencia de comandos de copia de seguridad de Windows Server



Los siguientes subcomandos para **wbadmin** proporciona funcionalidades de copia de seguridad y recuperación desde un símbolo del sistema.

Para configurar una programación de copia de seguridad, debe ser miembro de la **administradores** grupo. Para llevar a cabo todas las tareas con este comando, debe ser miembro de la **operadores de copia de seguridad** o el **administradores** grupo, o bien debe haber sido delegar los permisos adecuados.

Debe ejecutar **wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic en **iniciar**, haga clic en **símbolo**y, a continuación, haga clic en **ejecutar como administrador**.)

|Subcomando|Descripción|
|----------|-----------|
|[Wbadmin habilitar copia de seguridad](wbadmin-enable-backup.md)|Configura y habilita una programación de copia de seguridad diaria.|
|[copia de seguridad de Wbadmin disable](wbadmin-disable-backup.md)|Deshabilita las copias de seguridad diarias.|
|[Wbadmin start backup](wbadmin-start-backup.md)|Se ejecuta una copia de seguridad única. Si se usa sin parámetros, se usa la configuración de la programación de copia de seguridad diaria.|
|[detención del trabajo de Wbadmin](wbadmin-stop-job.md)|Detiene la operación de recuperación o copia de seguridad activo.|
|[versiones de Wbadmin get](wbadmin-get-versions.md)|Muestra detalles de las copias de seguridad recuperables desde el equipo local o, si se especifica otra ubicación, desde otro equipo.|
|[Wbadmin get elementos](wbadmin-get-items.md)|Enumera los elementos incluidos en una copia de seguridad específica.|
|[recuperación de Wbadmin start](wbadmin-start-recovery.md)|Ejecuta la recuperación de los volúmenes, aplicaciones, archivos o carpetas especificadas.|
|[obtener el estado de Wbadmin](wbadmin-get-status.md)|Muestra el estado de la operación de recuperación o copia de seguridad activo.|
|[Wbadmin get discos](wbadmin-get-disks.md)|Muestra los discos que están actualmente en línea.|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Se ejecuta una recuperación del estado del sistema.|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|Se ejecuta una copia de seguridad del estado del sistema.|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Elimina uno o más copias de estado del sistema.|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|Ejecuta la recuperación de todo el sistema (por lo menos todos los volúmenes que contienen el estado del sistema operativo). Este subcomando solo está disponible si utiliza el entorno de recuperación de Windows.|
|[catálogo de restauración de Wbadmin](wbadmin-restore-catalog.md)|Recupera un catálogo de copia de seguridad desde una ubicación de almacenamiento especificada en el caso donde se ha dañado el catálogo de copia de seguridad en el equipo local.|
|[catálogo de Wbadmin delete](wbadmin-delete-catalog.md)|Elimina el catálogo de copia de seguridad en el equipo local. Use este comando sólo si el catálogo de copia de seguridad en este equipo está dañado y no hay copias de seguridad almacenadas en otra ubicación que puede usar para restaurar el catálogo.|