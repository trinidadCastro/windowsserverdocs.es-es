---
title: wbadmin
description: Artículo de referencia de los comandos de Wbadmin, que permite realizar copias de seguridad y restaurar el sistema operativo, los volúmenes, los archivos, las carpetas y las aplicaciones desde un símbolo del sistema.
ms.topic: reference
ms.assetid: 4b0b3f32-d21f-4861-84bb-b2eadbf1e7b8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: df96e85d7cb4999088efe9bd6ede70087cd24cb7
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524670"
---
# <a name="wbadmin"></a>wbadmin

Permite realizar copias de seguridad y restaurar el sistema operativo, los volúmenes, los archivos, las carpetas y las aplicaciones desde un símbolo del sistema.

Para configurar una copia de seguridad programada con regularidad mediante este comando, debe ser miembro del grupo **administradores** . Para realizar todas las demás tareas con este comando, debe ser miembro del grupo **operadores de copia de seguridad** o del grupo **administradores** , o bien tener delegados los permisos adecuados.

Debe ejecutar **Wbadmin** desde un símbolo del sistema con privilegios elevados; para ello, haga clic con el botón secundario en **símbolo del sistema**y seleccione **Ejecutar como administrador**.

## <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| [wbadmin delete catalog](wbadmin-delete-catalog.md) | Elimina el catálogo de copias de seguridad en el equipo local. Utilice este comando solo si el catálogo de copias de seguridad de este equipo está dañado y no tiene copias de seguridad almacenadas en otra ubicación que pueda usar para restaurar el catálogo. |
| [wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md) | Elimina una o más copias de seguridad del estado del sistema. |
| [wbadmin disable backup](wbadmin-disable-backup.md) | Deshabilita las copias de seguridad diarias. |
| [wbadmin enable backup](wbadmin-enable-backup.md) | Configura y habilita una copia de seguridad programada periódicamente. |
| [wbadmin get disks](wbadmin-get-disks.md) | Enumera los discos que están actualmente en línea. |
| [wbadmin get items](wbadmin-get-items.md) | Enumera los elementos incluidos en una copia de seguridad. |
| [wbadmin get status](wbadmin-get-status.md) | Muestra el estado de la operación de copia de seguridad o recuperación que se está ejecutando actualmente. |
| [wbadmin get versions](wbadmin-get-versions.md) | Muestra detalles de las copias de seguridad recuperables desde el equipo local o, si se especifica otra ubicación, desde otro equipo. |
| [wbadmin restore catalog](wbadmin-restore-catalog.md) | Recupera un catálogo de copia de seguridad de una ubicación de almacenamiento especificada en el caso de que el catálogo de copia de seguridad del equipo local se haya dañado. |
| [wbadmin start backup](wbadmin-start-backup.md) | Ejecuta una copia de seguridad única. Si se usa sin parámetros, usa la configuración de la programación de copia de seguridad diaria. |
| [wbadmin start recovery](wbadmin-start-recovery.md) | Ejecuta una recuperación de los volúmenes, las aplicaciones, los archivos o las carpetas especificados. |
| [wbadmin start sysrecovery](wbadmin-start-sysrecovery.md) | Ejecuta una recuperación del sistema completo (al menos todos los volúmenes que contienen el estado del sistema operativo). Este comando solo está disponible si usa el entorno de recuperación de Windows. |
| [wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md) | Ejecuta una copia de seguridad del estado del sistema. |
| [wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md) | Ejecuta una recuperación de estado del sistema. |
| [wbadmin stop job](wbadmin-stop-job.md) | Detiene la operación de copia de seguridad o recuperación que se está ejecutando. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Cmdlets de Copias de seguridad de Windows Server en Windows PowerShell](/powershell/module/windowsserverbackup)

- [Entorno de recuperación de Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
