---
title: Administrar copias de seguridad y restaurar en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 41000915-f6ff-4dbb-b7be-629ef36386d4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1741d514728452a8fd9d1963811d4508ec158d78
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470821"
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>Administrar copias de seguridad y restaurar en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server Essentials permite realizar copias de seguridad de su servidor y sus equipos de red con frecuencia y de manera fiable. En caso de perder sus datos, puede restaurarlos desde una copia de seguridad correcta en el servidor sin restaurar todo el equipo. Si es necesario, puede realizar una restauración completa del sistema en el servidor o los equipos cliente en la red. En la tabla siguiente se describen las distintas opciones de copia de seguridad disponibles, además de sus ventajas.

|Característica de copia de seguridad|Descripción|Ventajas|
|--------------------|-----------------|----------------|
|Copia de seguridad del servidor|Realiza una copia de seguridad del servidor que ejecuta Windows Server Essentials. Los datos se copian en una unidad USB externa.<br /><br /> Para obtener más información, consulte [administrar copias de seguridad del servidor](Manage-Server-Backup-in-Windows-Server-Essentials.md) y [restaurar o reparar el servidor](Restore-or-repair-your-server-running-Windows-Server-Essentials.md).|-Puede restaurar archivos y carpetas en el servidor.<br /><br /> -Puede realizar una restauración completa del sistema de su servidor.|
|Copia de seguridad del equipo cliente|Realiza una copia de seguridad de los equipos cliente en la red. Los datos se copian en el servidor que ejecuta Windows Server Essentials.<br /><br /> Para obtener más información, consulte [administrar copias de seguridad de cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md) y [restaurar un sistema completo desde una copia de seguridad de equipos cliente existente](Restore-a-full-system-from-an-existing-client-computer-backup.md).|-Puede restaurar archivos y carpetas desde el servidor.<br /><br /> -Puede realizar una restauración completa del sistema del equipo cliente.|
| Microsoft Azure Backup|Realiza una copia de seguridad de los archivos o carpetas en el servidor. Cuando se usa Azure Backup para hacer copias de seguridad de los datos del servidor, la información se cifra con la frase de contraseña antes de cargarse en un centro de datos seguro de Internet.<br /><br /> Para obtener más información, consulte [administrar copias de seguridad en línea](Manage-Online-Backup-in-Windows-Server-Essentials.md).|-Puede restaurar archivos y carpetas desde el servidor.<br /><br /> -Con copias de seguridad incrementales, solo se transfieren a la nube los cambios en los archivos.<br /><br /> -Las copias de seguridad se almacenan en Microsoft Azure y están fuera del sitio, lo que reduce la necesidad de proteger los medios de copia de seguridad en el sitio.|

## <a name="additional-references"></a>Referencias adicionales

-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)

-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
