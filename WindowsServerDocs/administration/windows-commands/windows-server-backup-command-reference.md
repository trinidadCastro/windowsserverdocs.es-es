---
title: Referencia de comandos de copia de seguridad de Windows Server
description: Artículo de referencia de referencia de comandos de copia de seguridad.
ms.topic: article
ms.assetid: 03de0a65-21f0-4dd7-a3ae-251c98bbf6eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2042c55774aefbb603c8592d48be12d22c0f545e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896860"
---
# <a name="windows-server-backup-command-reference"></a>Referencia de comandos de copia de seguridad de Windows Server



Los siguientes subcomandos de **Wbadmin** proporcionan funcionalidad de copia de seguridad y recuperación desde un símbolo del sistema.

Para configurar una programación de copia de seguridad, debe ser miembro del grupo **administradores** . Para realizar todas las demás tareas con este comando, debe ser miembro de los grupos **operadores de copia de seguridad** o **administradores** , o bien tener delegados los permisos adecuados.

Debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados. (Para abrir un símbolo del sistema con privilegios elevados, haga clic en **Inicio**, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**).

|Subcomando|Descripción|
|----------|-----------|
|[Wbadmin enable backup](wbadmin-enable-backup.md)|Configura y habilita una programación de copia de seguridad diaria.|
|[Wbadmin disable backup](wbadmin-disable-backup.md)|Deshabilita las copias de seguridad diarias.|
|[Wbadmin start backup](wbadmin-start-backup.md)|Ejecuta una copia de seguridad única. Si se usa sin parámetros, usa la configuración de la programación de copia de seguridad diaria.|
|[Wbadmin stop job](wbadmin-stop-job.md)|Detiene la operación de copia de seguridad o recuperación que se está ejecutando.|
|[Wbadmin get versions](wbadmin-get-versions.md)|Muestra detalles de las copias de seguridad recuperables desde el equipo local o, si se especifica otra ubicación, desde otro equipo.|
|[Wbadmin get items](wbadmin-get-items.md)|Enumera los elementos incluidos en una copia de seguridad específica.|
|[Wbadmin start recovery](wbadmin-start-recovery.md)|Ejecuta una recuperación de los volúmenes, las aplicaciones, los archivos o las carpetas especificados.|
|[Wbadmin get status](wbadmin-get-status.md)|Muestra el estado de la operación de copia de seguridad o recuperación que se está ejecutando actualmente.|
|[Wbadmin get disks](wbadmin-get-disks.md)|Enumera los discos que están actualmente en línea.|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Ejecuta una recuperación de estado del sistema.|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|Ejecuta una copia de seguridad del estado del sistema.|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Elimina una o más copias de seguridad del estado del sistema.|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|Ejecuta una recuperación del sistema completo (al menos todos los volúmenes que contienen el estado del sistema operativo). Este subcomando solo está disponible si usa el entorno de recuperación de Windows.|
|[Wbadmin restore catalog](wbadmin-restore-catalog.md)|Recupera un catálogo de copia de seguridad de una ubicación de almacenamiento especificada en el caso de que el catálogo de copia de seguridad del equipo local se haya dañado.|
|[Wbadmin delete catalog](wbadmin-delete-catalog.md)|Elimina el catálogo de copias de seguridad en el equipo local. Utilice este comando solo si el catálogo de copias de seguridad de este equipo está dañado y no tiene copias de seguridad almacenadas en otra ubicación que pueda usar para restaurar el catálogo.|