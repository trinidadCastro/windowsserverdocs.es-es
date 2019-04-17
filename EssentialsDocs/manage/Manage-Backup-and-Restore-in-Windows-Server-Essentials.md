---
title: "Administrar la copia de seguridad y restauración en Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Administrar la copia de seguridad y restauración en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Windows Server Essentials proporciona formas confiable para realizar copias de seguridad regulares de tu servidor y equipos de la red. En caso de pérdida de datos, puedes restaurar datos desde una copia de seguridad correcta en el servidor sin restaurar todo el equipo. Si es necesario, puedes realizar una restauración completa del sistema en el servidor o los equipos cliente en la red. La siguiente tabla describen las diferentes opciones de copia de seguridad a tu disposición junto con sus ventajas.  
  
|Característica copia de seguridad|Descripción|Ventajas|  
|--------------------|-----------------|----------------|  
|Copia de seguridad del servidor|Copia el servidor que ejecuta Windows Server Essentials. Los datos se copia en una unidad USB externa.<br /><br /> Para obtener más información, consulta [administrar copias de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md) y [restaurar o reparar el servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-Restaurar archivos y carpetas en el servidor.<br /><br /> -Puede realizar la restauración del sistema completa del servidor.|  
|Copia de seguridad de equipo cliente|Copia de seguridad de los equipos cliente en la red. Los datos se copien en el servidor que ejecuta Windows Server Essentials.<br /><br /> Para obtener más información, consulta [administrar cliente copia de seguridad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) y [restaurar un sistema completo de una copia de seguridad del equipo cliente existente](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-Restaurar archivos y carpetas desde el servidor.<br /><br /> -Puede realizar la restauración del sistema completa del equipo cliente.|  
| Copia de seguridad de Microsoft Azure|Realiza una copia de seguridad de archivos o carpetas en el servidor. Cuando usas copia de seguridad de Azure para hacer copia de seguridad de datos del servidor, la información se cifra con la frase de contraseña antes de que se carga en un centro de datos segura en Internet.<br /><br /> Para obtener más información, consulta [administrar copias de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-Restaurar archivos y carpetas desde el servidor.<br /><br /> -Con incrementales, solo los cambios en los archivos se transfieren a la nube.<br /><br /> -Las copias de seguridad se almacenan en fuera del sitio de Microsoft Azure y son, lo que reduce la necesidad de proteger el medio de copia de seguridad en el sitio.|  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
