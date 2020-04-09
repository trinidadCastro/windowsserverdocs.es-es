---
title: Optimización del rendimiento para servidores de archivos SMB
description: Optimización del rendimiento para servidores de archivos SMB
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: nedpyle; danlo; dkruse
ms.date: 4/14/2017
ms.openlocfilehash: 89017686801501593c51245d44bf88a6ecf4baf6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851828"
---
# <a name="performance-tuning-for-smb-file-servers"></a>Optimización del rendimiento para servidores de archivos SMB

## <a name="smb-configuration-considerations"></a>Consideraciones sobre la configuración de SMB
No habilite ningún servicio o característica que no requieran los clientes y el servidor de archivos. Esto puede incluir la firma SMB, el almacenamiento en caché del lado cliente, los filtros minis del sistema de archivos, el servicio de búsqueda, las tareas programadas, el cifrado NTFS, la compresión NTFS, IPSEC, filtros de firewall, Teredo y cifrado SMB.

Asegúrese de que los modos de administración de energía del sistema operativo y BIOS estén configurados según sea necesario, lo que puede incluir el modo de alto rendimiento o el estado de C modificado. Asegúrese de que estén instalados los controladores de dispositivos de red y de almacenamiento más recientes, resistentes y más rápidos.

La copia de archivos es una operación común que se realiza en un servidor de archivos. Windows Server tiene varias utilidades de copia de archivos integradas que se pueden ejecutar mediante un símbolo del sistema. Se recomienda Robocopy. Introducida en Windows Server 2008 R2, la opción **/MT** de Robocopy puede mejorar significativamente la velocidad de las transferencias de archivos remotos mediante el uso de varios subprocesos cuando se copian varios archivos pequeños. También se recomienda usar la opción **/log** para reducir la salida de la consola redirigiendo los registros a un dispositivo NUL o a un archivo. Al usar xcopy, se recomienda agregar las opciones **/q** y **/k** a los parámetros existentes. La primera opción reduce la sobrecarga de la CPU al reducir la salida de la consola y el último reduce el tráfico de red.

## <a name="smb-performance-tuning"></a>Optimización del rendimiento de SMB


El rendimiento de los servidores de archivos y las optimizaciones disponibles dependen del protocolo SMB que se negocia entre cada cliente y el servidor, y en las características del servidor de archivos implementadas. La versión de protocolo más alta disponible actualmente es SMB 3.1.1 en Windows Server 2016 y Windows 10. Puede comprobar qué versión de SMB se usa en la red mediante el uso de Windows PowerShell **Get-SMBConnection** en los clientes y **Get-SMBSession | FL** en servidores.

### <a name="smb-30-protocol-family"></a>Familia del protocolo SMB 3,0

SMB 3,0 se presentó en Windows Server 2012 y se mejoró en Windows Server 2012 R2 (SMB 3,02) y Windows Server 2016 (SMB 3.1.1). Esta versión presentó tecnologías que pueden mejorar significativamente el rendimiento y la disponibilidad del servidor de archivos. Para obtener más información, vea [SMB en Windows Server 2012 y 2012 R2 2012](https://aka.ms/smb3plus) y [novedades de SMB 3.1.1](https://aka.ms/smb311).



### <a name="smb-direct"></a>SMB directo

SMB directo presentó la capacidad de usar interfaces de red RDMA para un alto rendimiento con baja latencia y uso de CPU bajo.

Siempre que SMB detecta una red compatible con RDMA, intenta usar automáticamente la capacidad RDMA. Sin embargo, si por alguna razón el cliente SMB no puede conectarse mediante la ruta de acceso RDMA, simplemente seguirá usando conexiones TCP/IP en su lugar. Todas las interfaces RDMA que son compatibles con SMB directo también son necesarias para implementar una pila TCP/IP y SMB multicanal es consciente de eso.

SMB directo no es necesario en ninguna configuración de SMB, pero siempre se recomienda para aquellos que desean una latencia más baja y un menor uso de la CPU.

Para obtener más información sobre SMB directo, consulte [mejorar el rendimiento de un servidor de archivos con SMB directo](https://aka.ms/smbdirect).

### <a name="smb-multichannel"></a>SMB multicanal

SMB multicanal permite que los servidores de archivos usen varias conexiones de red simultáneamente y proporciona un mayor rendimiento.

Para obtener más información sobre SMB multicanal, consulte [implementar SMB multicanal](https://aka.ms/smbmulti).

### <a name="smb-scale-out"></a>Escalado horizontal de SMB

El escalado horizontal de SMB permite a SMB 3,0 en una configuración de clúster mostrar un recurso compartido en todos los nodos de un clúster. Esta configuración activa/activa permite escalar los clústeres de servidores de archivos aún más, sin una configuración compleja con varios volúmenes, recursos compartidos y recursos de clúster. El ancho de banda máximo del recurso compartido es el ancho de banda total de todos los nodos del clúster de servidores de archivos. El ancho de banda total ya no está limitado por el ancho de banda de un solo nodo de clúster, sino que depende de la capacidad del sistema de almacenamiento de respaldo. Puede aumentar el ancho de banda total al agregar nodos.

Para obtener más información sobre el escalado horizontal de SMB, consulte [servidor de archivos de escalabilidad horizontal para información general sobre los datos de aplicación](https://technet.microsoft.com/library/hh831349.aspx) y la entrada de blog [para escalar horizontalmente o no escalar horizontalmente, esa es la cuestión](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx).

### <a name="performance-counters-for-smb-30"></a>Contadores de rendimiento para SMB 3,0

Los siguientes contadores de rendimiento de SMB se introdujeron en Windows Server 2012 y se consideran un conjunto básico de contadores al supervisar el uso de recursos de SMB 2 y versiones posteriores. Registre los contadores de rendimiento en un registro de contador de rendimiento local, sin formato (. BLG). Es más económico recopilar todas las instancias mediante el carácter comodín (\*) y, a continuación, extraer instancias concretas durante el procesamiento posterior mediante relog. exe.

-   **Recursos compartidos de cliente SMB**

    Estos contadores muestran información sobre los recursos compartidos de archivos del servidor a los que tiene acceso un cliente que usa SMB 2,0 o versiones posteriores.

    Si está familiarizado con los contadores de disco normales en Windows, es posible que observe una determinada similitud. Eso no es accidental. Los contadores de rendimiento de recursos compartidos de cliente SMB se diseñaron para coincidir exactamente con los contadores de disco. De esta manera, puede volver a usar fácilmente cualquier orientación sobre el ajuste del rendimiento del disco de aplicación que tiene actualmente. Para obtener más información sobre la asignación de contadores, consulte [blog de contadores de rendimiento por recurso compartido](https://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).

-   **Recursos compartidos de servidor SMB**

    Estos contadores muestran información sobre los recursos compartidos de archivos SMB 2,0 o superior en el servidor.

-   **Sesiones del servidor SMB**

    Estos contadores muestran información acerca de las sesiones del servidor SMB que usan SMB 2,0 o superior.

    La activación de contadores en el lado del servidor (recursos compartidos de servidor o sesiones de servidor) puede tener un impacto significativo en el rendimiento de las cargas de trabajo de e/s altas.

-   **Reanudar filtro de clave**

    Estos contadores muestran información sobre el filtro de clave de reanudación.

-   **Conexión SMB directo**

    Estos contadores miden distintos aspectos de la actividad de conexión. Un equipo puede tener varias conexiones de SMB directo. Los contadores de conexión SMB directo representan cada conexión como un par de direcciones IP y puertos, donde la primera dirección IP y el puerto representan el extremo local de la conexión y la segunda dirección IP y Puerto representan el extremo remoto de la conexión.

-   **Relaciones de contador de rendimiento de disco físico, SMB y CSV FS**

    Para obtener más información sobre cómo se relacionan los contadores de disco físico, SMB y CSV FS (sistema de archivos), vea la siguiente entrada de blog: [volumen compartido de clúster contadores de rendimiento](https://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx).

## <a name="tuning-parameters-for-smb-file-servers"></a>Parámetros de optimización para servidores de archivos SMB


La siguiente configuración del registro de REG\_DWORD puede afectar al rendimiento de los servidores de archivos SMB:

- **Smb2CreditsMin** y **Smb2CreditsMax**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
  ```

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
  ```

  Los valores predeterminados son 512 y 8192, respectivamente. Estos parámetros permiten que el servidor limite la simultaneidad de operaciones de cliente dinámicamente dentro de los límites especificados. Algunos clientes pueden lograr un mayor rendimiento con límites de simultaneidad más altos, por ejemplo, la copia de archivos a través de vínculos de gran ancho de banda y alta latencia.
    
  > [!TIP]
  > Antes de Windows 10 y Windows Server 2016, el número de créditos concedidos al cliente variaba dinámicamente entre Smb2CreditsMin y Smb2CreditsMax basándose en un algoritmo que intentaba determinar el número óptimo de créditos que se conceden en función de la latencia de red y el uso de crédito. En Windows 10 y Windows Server 2016, se cambió el servidor SMB para conceder de forma incondicional créditos al solicitar el número máximo de créditos configurado. Como parte de este cambio, el mecanismo de limitación de crédito, que reduce el tamaño de la ventana de crédito de cada conexión cuando el servidor está bajo presión de memoria, se quitó. El evento de memoria insuficiente del kernel que activó la limitación solo se señala cuando el servidor está tan bajo en la memoria (< unos pocos MB) para que no sea útil. Dado que el servidor ya no reduce las ventanas de crédito, el valor Smb2CreditsMin ya no es necesario y ahora se omite.
  > 
  > Puede supervisar recursos compartidos de cliente SMB\\paradas de crédito en/S para ver si hay algún problema con los créditos.

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    El valor predeterminado es 0, lo que significa que no se agrega ningún subproceso de trabajo de kernel crítico adicional. Este valor afecta al número de subprocesos que la memoria caché del sistema de archivos utiliza para las solicitudes de lectura y escritura previa. Aumentar este valor puede permitir más e/s en cola en el subsistema de almacenamiento, y puede mejorar el rendimiento de e/s, especialmente en sistemas con muchos procesadores lógicos y hardware de almacenamiento eficaz.

    >[!TIP]
    > Es posible que sea necesario aumentar el valor si la cantidad de datos modificados del administrador de caché (caché del contador de rendimiento\\páginas desfasadas) está creciendo para consumir una gran parte (más de un 25%). de memoria o si el sistema está realizando una gran cantidad de operaciones de e/s de lectura sincrónicas.

- **MaxThreadsPerQueue**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
  ```

  El valor predeterminado es 20. Al aumentar este valor, se eleva el número de subprocesos que el servidor de archivos puede usar para atender las solicitudes simultáneas. Cuando es necesario atender un gran número de conexiones activas, y los recursos de hardware, como el ancho de banda de almacenamiento, son suficientes, el aumento del valor puede mejorar la escalabilidad, el rendimiento y los tiempos de respuesta del servidor.

  >[!TIP]
  > Es una indicación de que es posible que sea necesario aumentar el valor si las colas de trabajo de SMB2 están creciendo muy grandes (el contador de rendimiento "colas de trabajo de servidor\\longitud de cola\\SMB2 de \*sin bloqueo" es constantemente superior a ~ 100).

  >[!Note]
  >En Windows 10 y Windows Server 2016, MaxThreadsPerQueue no está disponible. El número de subprocesos de un grupo de subprocesos será "20 * el número de procesadores en un nodo NUMA".
     

- **AsynchronousCredits**

  ``` 
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
  ```

  El valor predeterminado es 512. Este parámetro limita el número de comandos SMB asincrónicos simultáneos que se permiten en una sola conexión. Algunos casos (por ejemplo, cuando hay un servidor front-end con un servidor IIS de back-end) requieren una gran cantidad de simultaneidad (en concreto, para las solicitudes de notificación de cambios de archivo). El valor de esta entrada puede aumentarse para admitir estos casos.

### <a name="smb-server-tuning-example"></a>Ejemplo de optimización del servidor SMB

La siguiente configuración puede optimizar un equipo para el rendimiento del servidor de archivos en muchos casos. Los valores no son óptimos ni adecuados para todos los equipos. Debe evaluar el impacto de cada uno de los valores antes de aplicarlos.

| Parámetro                       | Valor | Default |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>Contadores del monitor de rendimiento del cliente SMB

Para obtener más información acerca de los contadores del cliente SMB, vea [información del servidor de archivos de Windows Server 2012: los nuevos contadores de rendimiento de cliente SMB por recurso compartido proporcionan una visión excelente](https://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).
