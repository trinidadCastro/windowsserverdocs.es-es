---
title: Administrar copias de seguridad y restaurar en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41000915-f6ff-4dbb-b7be-629ef36386d4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6f6f0d27472664cd1cc538897d3d525fad506282
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828526"
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Administrar copias de seguridad y restaurar en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials permite realizar copias de seguridad de su servidor y sus equipos de red con frecuencia y de manera fiable. En caso de perder sus datos, puede restaurarlos desde una copia de seguridad correcta en el servidor sin restaurar todo el equipo. Si es necesario, puede realizar una restauración completa del sistema en el servidor o los equipos cliente en la red. En la tabla siguiente se describen las distintas opciones de copia de seguridad disponibles, además de sus ventajas.  
  
|Característica de copia de seguridad|Descripción|Ventajas|  
|--------------------|-----------------|----------------|  
|Copia de seguridad del servidor|Realiza una copia de seguridad del servidor que ejecuta Windows Server Essentials. Los datos se copian en una unidad USB externa.<br /><br /> Para obtener más información, consulte [Manage Server Backup](Manage-Server-Backup-in-Windows-Server-Essentials.md) y [restaurar o reparar el servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-Se pueden restaurar archivos y carpetas en el servidor.<br /><br /> -Puede realizar la restauración completa del sistema del servidor.|  
|Copia de seguridad del equipo cliente|Realiza una copia de seguridad de los equipos cliente en la red. Los datos se copian en el servidor que ejecuta Windows Server Essentials.<br /><br /> Para obtener más información, consulte [administración de cliente de copia de seguridad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) y [restaurar completamente el sistema desde una copia de seguridad del equipo cliente existente](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-Se pueden restaurar archivos y carpetas desde su servidor.<br /><br /> -Puede realizar la restauración completa del sistema del equipo cliente.|  
| Microsoft Azure Backup|Realiza una copia de seguridad de los archivos o carpetas en el servidor. Al usar Azure Backup para realizar una copia de seguridad de los datos del servidor, la información se cifra mediante el uso de la frase de contraseña antes de cargarlos en un centro de datos segura en Internet.<br /><br /> Para obtener más información, consulte [administrar copias de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-Se pueden restaurar archivos y carpetas desde su servidor.<br /><br /> -Con copias de seguridad incrementales, solo los cambios en los archivos se transfieren a la nube.<br /><br /> -Las copias de seguridad se almacenan en fuera del sitio de Microsoft Azure y, lo que reduce la necesidad de asegurar y proteger los medios de copia de seguridad.|  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
