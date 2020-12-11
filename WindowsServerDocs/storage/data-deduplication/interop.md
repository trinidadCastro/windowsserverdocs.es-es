---
description: 'Más información sobre: interoperabilidad de desduplicación de datos'
ms.assetid: 60fca6b2-f1c0-451f-858f-2f6ab350d220
title: Interoperabilidad de Desduplicación de datos
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/16/2016
ms.openlocfilehash: 0acd0b05dcf4bf0d98cd14d9b5e1991d40feadea
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039192"
---
# <a name="data-deduplication-interoperability"></a>Interoperabilidad de Desduplicación de datos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2019

## <a name="supported"></a>Compatible

### <a name="refs"></a>ReFS
La desduplicación de datos se admite a partir de Windows Server 2019.

### <a name="failover-clustering"></a>Clústeres de conmutación por error

Los [clústeres de conmutación por error](../..//failover-clustering/failover-clustering-overview.md) son totalmente compatibles si todos los nodos del clúster tienen [instalada la característica Desduplicación de datos](install-enable.md#install-dedup). Otras notas importantes:

* Deben ejecutarse los [trabajos de Desduplicación de datos iniciados de forma manual](run.md#running-dedup-jobs-manually) en el nodo propietario para el volumen compartido de clúster.
* Los trabajos de Desduplicación de datos programados se almacenan en la tarea de clúster programada, de modo que si otro nodo toma un volumen desduplicado, el trabajo programado se aplique en el siguiente intervalo programado.
* Desduplicación de datos interopera completamente con la [actualización gradual del sistema operativo de clúster](../..//failover-clustering/cluster-operating-system-rolling-upgrade.md).
* Desduplicación de datos es totalmente compatible con los volúmenes con formato NTFS de [Espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md) (reflejo o paridad). La desduplicación no se admite en volúmenes con varios niveles. Consulte [Data Deduplication on ReFS](#unsupported) (Desduplicación de datos en ReFS) para obtener más información.

### <a name="storage-replica"></a>Réplica de almacenamiento
La [réplica de almacenamiento](../storage-replica/storage-replica-overview.md) es totalmente compatible. La desduplicación de datos se debe configurar para que no se ejecute en la copia secundaria.

### <a name="branchcache"></a>BranchCache
Para optimizar el acceso a los datos en la red, habilite [BranchCache](../../networking/branchcache/branchcache.md) en servidores y clientes. Cuando un sistema habilitado para BranchCache se comunica en una WAN con un servidor de archivos remoto que ejecuta Desduplicación de datos, todos los archivos desduplicados ya están indexados y se les aplicó un algoritmo hash. Por lo tanto, las solicitudes de datos desde una sucursal se calculan rápidamente. Esto es similar al indexado o al uso de hash previos de un servidor habilitado para BranchCache.

### <a name="dfs-replication"></a>Replicación DFS
Desduplicación de datos funciona con la replicación del Sistema de archivos distribuido (DFS). Optimizar o desoptimizar un archivo no desencadenará una replicación porque el archivo no cambia. La replicación DFS usa la Compresión diferencial remota (RDC), no los fragmentos del almacén de fragmentos, para ahorrar en la conexión. También se pueden optimizar los archivos de la réplica mediante la desduplicación si la réplica usa la desduplicación de datos.

### <a name="quotas"></a>Cuotas
Desduplicación de datos no permite establecer una cuota máxima en una carpeta raíz de volumen que también tenga habilitada la desduplicación. Cuando hay una cuota máxima en una raíz de volumen, el espacio libre real y el espacio restringido por la cuota no coinciden. Esto puede provocar errores en los trabajos de optimización de la desduplicación. Pero se permite establecer una cuota de advertencia en una carpeta raíz de volumen que tenga habilitada la desduplicación.

Cuando se habilita la cuota en un volumen desduplicado, la cuota usa el tamaño lógico del archivo en lugar del tamaño físico del archivo. El uso de cuotas (incluidos los umbrales de cuota) no cambian cuando la desduplicación procesa un archivo. Todas la demás funcionalidades de las cuotas, incluidas las cuotas de advertencia para raíces de volumen y las cuotas en subcarpetas, funcionan con normalidad cuando se usa la desduplicación.

### <a name="windows-server-backup"></a>Copias de seguridad de Windows Server
Copias de seguridad de Windows Server puede realizar una copia de seguridad "tal cual" de un volumen optimizado (es decir, sin eliminar los datos desduplicados). Los pasos siguientes muestran cómo realizar copias de seguridad de un volumen y cómo restaurar un volumen o archivos seleccionados de un volumen.
1. Instale Copias de seguridad de Windows Server.
    ```PowerShell
    Install-WindowsFeature -Name Windows-Server-Backup
    ```

2. Realice una copia de seguridad del volumen E: en otro volumen. Para ello, ejecute el comando siguiente, en el que deberá sustituir los nombres de volumen correctos en su caso.
    ```PowerShell
    wbadmin start backup –include:E: -backuptarget:F: -quiet
    ```
3. Obtenga el identificador de versión de la copia de seguridad que acaba de crear.

    ```PowerShell
    wbadmin get versions
    ```

    Este identificador de la versión de salida será una cadena de fecha y hora, por ejemplo: 18/08/2016-06:22.

4. Restaure todo el volumen.
    ```PowerShell
    wbadmin start recovery –version:02/16/2012-06:22 -itemtype:Volume  -items:E: -recoveryTarget:E:
    ```

    **--O bien--**

    Restaure una carpeta determinada (en este caso, la carpeta E:\Docs):
    ```PowerShell
    wbadmin start recovery –version:02/16/2012-06:22 -itemtype:File  -items:E:\Docs  -recursive
    ```

## <a name="unsupported"></a>No compatible

### <a name="windows-10-client-os"></a>Windows 10 (sistema operativo de cliente)
La desduplicación de datos no se admite en Windows 10. Hay varias entradas de blog populares en la comunidad de Windows que describen cómo quitar los binarios de Windows Server 2016 e instalar en Windows 10, pero este escenario no se ha validado como parte del desarrollo de desduplicación de datos. [Vote por este elemento para Windows 10 vNext en UserVoice de almacenamiento de Windows Server](https://windowsserver.uservoice.com/forums/295056-storage/suggestions/9011008-add-deduplication-support-to-client-os).

### <a name="windows-search"></a>Windows Search
Windows Search no es compatible con la desduplicación de datos. La desduplicación de datos usa puntos de reanálisis, que Windows Search no puede indexar, por lo que Windows Search omite todos los archivos desduplicados, excluidos del índice. Como resultado, los resultados de búsqueda podrían estar incompletos en los volúmenes desduplicados. [Vote por este elemento para Windows Server vNext en UserVoice de almacenamiento de Windows Server](https://windowsserver.uservoice.com/forums/295056-storage/suggestions/17888647-make-windows-search-service-work-with-data-dedupli).

### <a name="robocopy"></a>Robocopy
No se recomienda ejecutar Robocopy con Desduplicación de datos, porque ciertos comandos de Robocopy pueden dañar el almacén de fragmentos. El almacén de fragmentos se almacena en la carpeta de información del volumen del sistema para un volumen. Si se elimina la carpeta, los archivos optimizados (puntos de reanálisis) que se copian desde el volumen de origen se dañan porque los fragmentos de datos no se copian en el volumen de destino.
