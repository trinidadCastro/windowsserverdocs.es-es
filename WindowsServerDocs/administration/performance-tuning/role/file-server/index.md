---
title: Optimización del rendimiento de servidores de archivos
description: Optimización del rendimiento de servidores de archivos que ejecutan Windows Server
ms.topic: article
author: phstee
ms.author: nedpyle; danlo; dkruse; v-tea
ms.date: 12/12/2019
manager: dcscontentpm
audience: Admin
ms.openlocfilehash: ecbd1bc751f133b80cf1d9cb264cf70a4ac4f47c
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992202"
---
# <a name="performance-tuning-for-file-servers"></a>Optimización del rendimiento de servidores de archivos

Debe seleccionar el hardware adecuado para satisfacer la carga web esperada, teniendo en cuenta la carga promedio, la carga máxima, la capacidad, los planes de crecimiento y los tiempos de respuesta. Los cuellos de botella de hardware limitan la eficacia de la optimización del software.

## <a name="general-tuning-parameters-for-clients"></a>Parámetros generales de optimización para clientes

La siguiente configuración del Registro REG\_DWORD puede afectar al rendimiento de los equipos cliente que interactúan con los servidores de archivos SMB:

-   **ConnectionCountPerNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerNetworkInterface
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

    El valor predeterminado es 1 y se recomienda encarecidamente usar ese valor. El intervalo válido es entre 1 y 16. Número máximo de conexiones por interfaz que se establecerá con un servidor para interfaces no RSS.


-   **ConnectionCountPerRssNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRssNetworkInterface
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

    El valor predeterminado es 4 y se recomienda encarecidamente usar ese valor. El intervalo válido es entre 1 y 16. Número máximo de conexiones por interfaz que se establecerá con un servidor para interfaces RSS.

-   **ConnectionCountPerRdmaNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRdmaNetworkInterface
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

    El valor predeterminado es 2 y se recomienda encarecidamente usar ese valor. El intervalo válido es entre 1 y 16. Número máximo de conexiones por interfaz que se establecerá con un servidor para interfaces RDMA.

-   **MaximumConnectionCountPerServer**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaximumConnectionCountPerServer
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

    El valor predeterminado es 32, con un intervalo válido entre 1 y 64. Número máximo de conexiones que se establecerá con un solo servidor que ejecuta Windows Server 2012 en todas las interfaces.

-   **DormantDirectoryTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantDirectoryTimeout
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

    El valor predeterminado es de 600 segundos. Tiempo máximo que los controladores del directorio de servidor se mantienen abiertos con las concesiones de directorio.

-   **FileInfoCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheLifetime
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 10 segundos. Tiempo de espera de caché de información de archivo.

-   **DirectoryCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheLifetime
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 10 segundos. Tiempo de espera de caché de directorio.

    > [!NOTE]
    > Este parámetro controla el almacenamiento en caché de los metadatos del directorio en ausencia de concesiones de directorio.

     > [!NOTE]
     > Un problema conocido de Windows 10, versión 1803, afecta a la capacidad de Windows 10 de almacenar en caché directorios grandes. Después de actualizar un equipo a la versión 1803 de Windows 10, tienes acceso a un recurso compartido de red que contiene miles de archivos y carpetas, y abres un documento que se encuentra en ese recurso compartido. Durante ambas operaciones, experimentas retrasos importantes.
     >
     > Para resolver este problema, instala Windows 10, versión 1809 o posterior.
     >
     > Para solucionar este problema, establece  **DirectoryCacheLifetime** en **0**.
     >
     > Este problema afecta a las siguientes ediciones de Windows 10:
     > - Windows 10 Enterprise, versión 1803
     > - Windows 10 Pro for Workstations, versión 1803
     > - Windows 10 Pro Education, versión 1803
     > - Windows 10 Professional, versión 1803
     > - Windows 10 Education, versión 1803
     > - Windows 10 Home, versión 1803

-   **DirectoryCacheEntrySizeMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntrySizeMax
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 64 KB. El tamaño máximo de las entradas de caché de directorio.

-   **FileNotFoundCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheLifetime
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 5 segundos. Período de tiempo de caché de archivo no encontrado.

-   **CacheFileTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\CacheFileTimeout
    ```

    Se aplica a Windows 8.1, Windows 8, Windows Server 2012, Windows Server 2012 R2 y Windows 7

    El valor predeterminado es 10 segundos. Esta configuración controla el período (en segundos) durante el cual el redirector conservará los datos en caché de un archivo después de que una aplicación cierra el último controlador del archivo.

-   **DisableBandwidthThrottling**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 0. De manera predeterminada, el redirector de SMB limita el rendimiento en las conexiones de red de alta latencia, en algunos casos para evitar tiempos de espera relacionados con la red. Establecer este valor del Registro en 1 deshabilita esta limitación y permite un mayor rendimiento de transferencia de archivos a través de conexiones de red de alta frecuencia.

-   **DisableLargeMtu**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableLargeMtu
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 0 solo para Windows 8. En Windows 8, el redirector de SMB transfiere cargas de hasta 1 MB por solicitud, lo que puede mejorar la velocidad de la transferencia de archivos. Establecer este valor del Registro en 1 limita el tamaño de solicitud a 64 KB. Debe evaluar el impacto de esta configuración antes de aplicarla.

-   **RequireSecuritySignature**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\RequireSecuritySignature
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 0, lo que deshabilita la firma SMB. Cambiar este valor a 1 habilita la firma SMB para toda la comunicación de SMB, lo que impide la comunicación de SMB con equipos en los que la firma SMB está deshabilitada. La firma SMB puede aumentar el costo de CPU y los recorridos de ida y vuelta de la red, pero ayuda a bloquear ataques de tipo "Man in the middle". Si no se requiere la firma SMB, asegúrese de que este valor de Registro sea 0 en todos los clientes y servidores.

    Para más información, consulte [los aspectos básicos de la firma SMB](/archive/blogs/josebda/the-basics-of-smb-signing-covering-both-smb1-and-smb2).

-   **FileInfoCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 64, con un intervalo válido entre 1 y 65536. Este valor se usa para determinar la cantidad de metadatos de archivo que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico y aumentar el rendimiento cuando se accede a un gran número de archivos.

-   **DirectoryCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 16, con un intervalo válido entre 1 y 4096. Este valor se usa para determinar la cantidad de información de directorio que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico y aumentar el rendimiento cuando se accede a directorios de gran tamaño.

-   **FileNotFoundCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 128, con un intervalo válido entre 1 y 65536. Este valor se usa para determinar la cantidad de información de nombres de archivos que el cliente puede almacenar en caché. Aumentar el valor puede disminuir el tráfico y aumentar el rendimiento cuando se accede a un gran número de nombres de archivos.

-   **MaxCmds**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaxCmds
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 15. Este parámetro limita el número de solicitudes pendientes de una sesión. Aumentar el valor puede usar más memoria, pero puede mejorar el rendimiento al habilitar una canalización de solicitud más profunda. Aumentar el valor junto con MaxMpxCt también puede eliminar los errores que se encuentran debido al gran número de solicitudes de archivo a largo plazo pendientes, como las llamadas de FindFirstChangeNotification. Este parámetro no afecta las conexiones con los servidores SMB 2.0.

-   **DormantFileLimit**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit
    ```

    Se aplica a Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

    El valor predeterminado es 1023. Este parámetro especifica el número máximo de archivos que se deben dejar abiertos en un recurso compartido una vez que la aplicación cerró el archivo.

### <a name="client-tuning-example"></a>Ejemplo de optimización de cliente

Los parámetros generales de optimización de equipos cliente pueden optimizar un equipo para acceder a recursos compartidos de archivos remotos, en especial en algunas redes de alta latencia (como sucursales, comunicación entre centros de datos, oficinas domésticas y banda ancha móvil). Los valores no son óptimos ni adecuados para todos los equipos. Debe evaluar el impacto de cada uno de los valores antes de aplicarlos.

| Parámetro                   | Value | Valor predeterminado |
|-----------------------------|-------|---------|
| DisableBandwidthThrottling  | 1     | 0       |
| FileInfoCacheEntriesMax     | 32 768 | 64      |
| DirectoryCacheEntriesMax    | 4096  | 16      |
| FileNotFoundCacheEntriesMax | 32 768 | 128     |
| MaxCmds                     | 32 768 | 15      |



A partir de Windows 8, puede configurar muchos de estos valores de SMB si usa los cmdlets **Set-SmbClientConfiguration** y **Set-SmbServerConfiguration** de Windows PowerShell. Los valores que solo son del Registro también se pueden configurar con Windows PowerShell.

```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```