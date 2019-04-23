---
title: Optimizar el rendimiento de los servidores de archivos NFS
description: Optimizar el rendimiento de los servidores de archivos NFS
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: RoopeshB, NedPyle
ms.date: 10/16/2017
ms.openlocfilehash: 06a2a7206d3673046bd5a926a657bac91b02bf5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879016"
---
# <a name="performance-tuning-nfs-file-servers"></a>Servidores de archivos NFS de optimización del rendimiento

## <a href="" id="servicesnfs"></a>Servicios para el modelo NFS


Las secciones siguientes proporcionan información acerca de Microsoft Services para el modelo de Network File System (NFS) para la comunicación cliente / servidor. Puesto que NFS v2 y v3 NFS son aún más ampliamente implementado versiones del protocolo, todas las claves del registro, excepto MaxConcurrentConnectionsPerIp se aplican a NFS v2 y v3 NFS únicamente.

Ningún registro de optimización es necesaria para el protocolo de la versión 4.1 NFS.

### <a name="service-for-nfs-model-overview"></a>Servicio de información general del modelo NFS

Microsoft Services para NFS proporciona una solución de uso compartido de archivos para las empresas que tienen un entorno mixto de Windows y UNIX. Este modelo de comunicación consta de los equipos cliente y un servidor. Aplicaciones en los archivos de solicitud de cliente que se encuentran en el servidor mediante el redirector (Rdbss.sys) y el redirector mínima de NFS (Nfsrdr.sys). El mini redireccionador usa el protocolo NFS para enviar su solicitud a través de TCP/IP. El servidor recibe varias solicitudes de los clientes a través de TCP/IP y enruta las solicitudes al sistema de archivos local (Ntfs.sys), que tiene acceso a la pila de almacenamiento.

En la siguiente ilustración se muestra el modelo de comunicación para NFS.

![modelo de comunicación de NFS](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>Ajuste de los parámetros para los servidores de archivos NFS

El siguiente REG\_valores de DWORD del registro pueden afectar al rendimiento de los servidores de archivos NFS:

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    El valor predeterminado es 0. Este parámetro determina si se abren archivos para el archivo\_RANDOM\_acceso o archivo\_SEQUENTIAL\_solo según las características de E/S de la carga de trabajo. Establezca este valor en 1 para forzar que los archivos de estar abiertos para el archivo\_RANDOM\_acceso. ARCHIVO\_RANDOM\_acceso impide que el administrador del sistema y la memoria caché de archivos de captura previa.

    >[!NOTE]
    > Esta configuración debe evaluar cuidadosamente porque puede tener impacto potencial en la memoria caché de archivos de sistema crezca.


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    El valor predeterminado es 5. Este parámetro controla la duración de una entrada de caché NFS en la caché del identificador de archivo. El parámetro hace referencia a las entradas de caché que tienen un archivo NTFS abierto asociado a controlar. Duración real es aproximadamente igual a RdWrHandleLifeTime multiplicado por RdWrThreadSleepTime. El valor mínimo es 1 y el máximo es 60.

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    El valor predeterminado es 5. Este parámetro controla la duración de una entrada de caché NFS en la caché del identificador de archivo. El parámetro hace referencia a las entradas de caché que no tienen un archivo abierto asociado de NTFS controlar. Servicios para NFS usa estas entradas de caché para almacenar los atributos de archivo para un archivo sin necesidad de mantener un identificador abierto con el sistema de archivos. Duración real es aproximadamente igual a RdWrNfsHandleLifeTime multiplicado por RdWrThreadSleepTime. El valor mínimo es 1 y el máximo es 60.

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    El valor predeterminado es 5. Este parámetro controla la duración de un NFS leer la entrada de caché en la caché del identificador de archivo. Duración real es aproximadamente igual a RdWrNfsReadHandlesLifeTime multiplicado por RdWrThreadSleepTime. El valor mínimo es 1 y el máximo es 60.

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    El valor predeterminado es 5. Este parámetro controla el intervalo de espera antes de ejecutar el subproceso de limpieza en la caché del identificador de archivo. El valor es en tics, y es no determinista. Un TIC equivale a aproximadamente 100 nanosegundos. El valor mínimo es 1 y el máximo es 60.

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    El valor predeterminado es 4. Este parámetro especifica la memoria máxima que será consumido por las entradas de caché del identificador de archivo. El valor mínimo es 1 y el máximo es 1\*1024\*1024\*1024 (1073741824).

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    El valor predeterminado es 0. Este parámetro especifica si las páginas físicas que se asignan para el tamaño de caché especificado por FileHandleCacheSizeInMB están bloqueadas en la memoria. Establecer este valor en 1, permite esta actividad. Se bloquean las páginas en memoria (no paginada al disco), lo que mejora el rendimiento de la resolución de identificadores de archivos, pero reduce la memoria que está disponible para las aplicaciones.

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    El valor predeterminado es 64. Este parámetro especifica el número máximo de identificadores por volumen para la caché de lectura de datos. Las entradas de caché de lectura se crean únicamente en sistemas que tienen más de 1 GB de memoria. El valor mínimo es 0 y el máximo es 0xFFFFFFFF.

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    El valor predeterminado es 1. Este parámetro controla si los identificadores que son asignadas por el servidor de archivos NFS están firmados criptográficamente. Si se establece en 0 deshabilita la firma del controlador.

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    El valor predeterminado es 60. Este parámetro es un tiempo de espera temporalmente que controla la duración de NFS V3 INESTABLE escribir datos de almacenamiento en caché. El valor mínimo es 1 y el máximo es 600. Duración real es aproximadamente igual a RdWrNfsDeferredWritesFlushDelay multiplicado por RdWrThreadSleepTime.

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    El valor predeterminado es 1 (habilitado). Este parámetro controla si manipuladores que están abiertos durante NFS V2 y V3 crear y MKDIR RPC procedimiento controladores se conservan en el archivo de controlan la memoria caché. Establezca este valor en 0 para deshabilitar agregar entradas a la memoria caché en las rutas de código de creación y MKDIR.

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    Aumenta el número de subprocesos de trabajo diferido que se crean para la cola de trabajo especificado. Retrasa los elementos de trabajo de proceso de trabajo subprocesos que no se consideran críticas en el tiempo y que puede tener su pila de memoria paginado mientras se espera que los elementos de trabajo. Un número suficiente de subprocesos reduce la velocidad en qué trabajo atiende elementos; un valor que es demasiado alto consume innecesariamente los recursos del sistema.

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    El valor predeterminado en Windows Server 2012 y Windows Server 2012 R2 es 2. En versiones anteriores a Windows Server 2012, el valor predeterminado es 0. Este parámetro determina si NTFS genera un nombre corto en el 8.3 convención de nomenclatura (MS-DOS) para nombres de archivo largos y nombres de archivo que contengan caracteres del juego de caracteres extendidos. Si el valor de esta entrada es 0, los archivos pueden tener dos nombres: el nombre que especifica el usuario y el nombre corto que genera NTFS. Si el nombre especificado por el usuario sigue la convención de nomenclatura 8.3, NTFS no genera un nombre corto. Un valor de 2 significa que este parámetro puede configurarse por volumen.

    >[!NOTE]
    > El volumen del sistema tiene 8.3 habilitado de forma predeterminada. Todos los otros volúmenes en Windows Server 2012 y Windows Server 2012 R2 tienen 8.3 deshabilitada de forma predeterminada. Si cambia este valor no cambia el contenido de un archivo, pero evita la creación del atributo de nombre corto para el archivo, que cambia también cómo se muestra y administra el archivo de NTFS. Para la mayoría de los servidores de archivos, el valor recomendado es 1 (deshabilitado).


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    El valor predeterminado es 1. Este modificador global del sistema reduce la carga de E/S de disco y las latencias al deshabilitar la actualización de la marca de fecha y hora para el último acceso al archivo o directorio.

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    El valor predeterminado del parámetro MaxConcurrentConnectionsPerIp es 16. Puede aumentar este valor hasta un máximo de 8192 para aumentar el número de conexiones por dirección IP.
