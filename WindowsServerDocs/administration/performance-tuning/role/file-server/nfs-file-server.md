---
title: Optimización del rendimiento para servidores de archivos NFS
description: Optimización del rendimiento para servidores de archivos NFS
ms.topic: article
author: phstee
ms.author: roopeshb, nedpyle
ms.date: 10/16/2017
ms.openlocfilehash: 897e45a99ac4640c5fbae4287ac99a0bce6eae66
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896185"
---
# <a name="performance-tuning-nfs-file-servers"></a>Optimizar el rendimiento de servidores de archivos NFS

## <a name="services-for-nfs-model"></a><a href="" id="servicesnfs"></a>Modelo de servicios para NFS


En las secciones siguientes se proporciona información sobre el modelo de servicios de red para Network File System (NFS) para la comunicación cliente-servidor. Dado que NFS V2 y NFS V3 siguen siendo las versiones más ampliamente implementadas del Protocolo, todas las claves del registro, excepto MaxConcurrentConnectionsPerIp, se aplican solo a NFS V2 y NFS V3.

No se requiere la optimización del registro para el protocolo NFS v 4.1.

### <a name="service-for-nfs-model-overview"></a>Información general sobre el modelo de servicio para NFS

Servicios de Microsoft para NFS proporciona una solución de uso compartido de archivos para empresas que tienen un entorno mixto de Windows y UNIX. Este modelo de comunicación se compone de equipos cliente y un servidor. Aplicaciones en los archivos de solicitud de cliente que se encuentran en el servidor a través del redirector (Rdbss.sys) y el redireccionador de NFS (Nfsrdr.sys). El miniredireccionador usa el protocolo NFS para enviar su solicitud a través de TCP/IP. El servidor recibe varias solicitudes de los clientes a través de TCP/IP y enruta las solicitudes al sistema de archivos local (Ntfs.sys), que tiene acceso a la pila de almacenamiento.

En la ilustración siguiente se muestra el modelo de comunicación para NFS.

![modelo de comunicación NFS](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>Parámetros de optimización para servidores de archivos NFS

La siguiente \_ configuración del registro de REG DWORD puede afectar al rendimiento de los servidores de archivos NFS:

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    El valor predeterminado es 0. Este parámetro determina si los archivos se abren para \_ el acceso aleatorio de archivos \_ o solo para archivos \_ secuenciales \_ , en función de las características de e/s de la carga de trabajo. Establezca este valor en 1 para forzar que se abran los archivos para el \_ acceso aleatorio de archivo \_ . \_ \_ El acceso aleatorio de archivos impide que el sistema de archivos y el administrador de caché realicen la captura previa.

    >[!NOTE]
    > Esta configuración se debe evaluar cuidadosamente porque puede tener un impacto potencial en el crecimiento de la caché de archivos del sistema.


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    El valor predeterminado es 5. Este parámetro controla la duración de una entrada de caché de NFS en la caché de identificadores de archivos. El parámetro hace referencia a las entradas de caché que tienen un identificador de archivo NTFS abierto asociado. La duración real es aproximadamente igual a RdWrHandleLifeTime multiplicada por RdWrThreadSleepTime. El mínimo es 1 y el máximo es 60.

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    El valor predeterminado es 5. Este parámetro controla la duración de una entrada de caché de NFS en la caché de identificadores de archivos. El parámetro hace referencia a las entradas de caché que no tienen un identificador de archivo NTFS abierto asociado. Servicios para NFS usa estas entradas de caché para almacenar los atributos de archivo de un archivo sin mantener un identificador abierto con el sistema de archivos. La duración real es aproximadamente igual a RdWrNfsHandleLifeTime multiplicada por RdWrThreadSleepTime. El mínimo es 1 y el máximo es 60.

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    El valor predeterminado es 5. Este parámetro controla la duración de una entrada de caché de lectura de NFS en la caché de identificadores de archivos. La duración real es aproximadamente igual a RdWrNfsReadHandlesLifeTime multiplicada por RdWrThreadSleepTime. El mínimo es 1 y el máximo es 60.

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    El valor predeterminado es 5. Este parámetro controla el intervalo de espera antes de ejecutar el subproceso de limpieza en la caché de identificadores de archivos. El valor se encuentra en pasos y no es determinista. Una marca es equivalente a aproximadamente 100 nanosegundos. El mínimo es 1 y el máximo es 60.

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    El valor predeterminado es 4. Este parámetro especifica la cantidad máxima de memoria que consumirán las entradas de la caché de identificadores de archivos. El mínimo es 1 y el máximo es 1 \* 1024 \* 1024 \* 1024 (1073741824).

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    El valor predeterminado es 0. Este parámetro especifica si las páginas físicas que se asignan para el tamaño de caché especificado por FileHandleCacheSizeInMB están bloqueadas en la memoria. Si se establece este valor en 1, se habilita esta actividad. Las páginas se bloquean en la memoria (no en el disco), lo que mejora el rendimiento de la resolución de identificadores de archivo, pero reduce la memoria disponible para las aplicaciones.

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    El valor predeterminado es 64. Este parámetro especifica el número máximo de identificadores por volumen para la memoria caché de lectura de datos. Las entradas de caché de lectura se crean solo en sistemas que tienen más de 1 GB de memoria. El valor mínimo es 0 y el máximo es 0xFFFFFFFF.

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    El valor predeterminado es 1. Este parámetro controla si se firman criptográficamente los identificadores que proporciona el servidor de archivos NFS. Si se establece en 0, se deshabilita la firma del controlador.

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    El valor predeterminado es 60. Este parámetro es un tiempo de espera flexible que controla la duración del almacenamiento en caché de datos de escritura no estable de NFS V3. El mínimo es 1 y el máximo es 600. La duración real es aproximadamente igual a RdWrNfsDeferredWritesFlushDelay multiplicada por RdWrThreadSleepTime.

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    El valor predeterminado es 1 (habilitado). Este parámetro controla si los identificadores que se abren durante los controladores de los procedimientos de NFS V2 y V3 CREATE y MKDIR RPC se conservan en la memoria caché de identificadores de archivos. Establezca este valor en 0 para deshabilitar la adición de entradas a la memoria caché en las rutas de acceso CREATE y MKDIR Code.

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    Aumenta el número de subprocesos de trabajo retrasados que se crean para la cola de trabajo especificada. Los subprocesos de trabajo retrasados procesan los elementos de trabajo que no se consideran críticos para el tiempo y que pueden paginar su pila de memoria mientras esperan los elementos de trabajo. Un número insuficiente de subprocesos reduce la velocidad a la que se presta servicio a los elementos de trabajo; un valor demasiado alto consume recursos del sistema innecesariamente.

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    El valor predeterminado en Windows Server 2012 y Windows Server 2012 R2 es 2. En las versiones anteriores a Windows Server 2012, el valor predeterminado es 0. Este parámetro determina si NTFS genera un nombre corto en la Convención de nomenclatura 8.3 (MSDOS) para los nombres de archivo largos y para los nombres de archivo que contienen caracteres del juego de caracteres extendido. Si el valor de esta entrada es 0, los archivos pueden tener dos nombres: el nombre que especifica el usuario y el nombre corto que genera NTFS. Si el nombre especificado por el usuario sigue la Convención de nomenclatura 8dot3, NTFS no genera un nombre corto. Un valor de 2 significa que este parámetro se puede configurar por volumen.

    >[!NOTE]
    > El volumen del sistema tiene el valor 8.3 habilitado de forma predeterminada. El resto de volúmenes en Windows Server 2012 y Windows Server 2012 R2 tienen 8.3 deshabilitado de forma predeterminada. Al cambiar este valor, no se cambia el contenido de un archivo, pero se evita la creación de atributos de nombre corto para el archivo, lo que también cambia el modo en que NTFS muestra y administra el archivo. Para la mayoría de los servidores de archivos, el valor recomendado es 1 (deshabilitado).


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    El valor predeterminado es 1. Este conmutador global del sistema reduce la carga y las latencias de e/s de disco al deshabilitar la actualización de la marca de fecha y hora para el último acceso al archivo o directorio.

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    El valor predeterminado del parámetro MaxConcurrentConnectionsPerIp es 16. Puede aumentar este valor hasta un máximo de 8192 para aumentar el número de conexiones por dirección IP.
