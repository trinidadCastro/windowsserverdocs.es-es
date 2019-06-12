---
title: Optimizar el rendimiento de los servidores de archivos SMB
description: Optimizar el rendimiento de los servidores de archivos SMB
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: 87ad8058f7353c938087b1211e0f17820f0bd2ae
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435650"
---
# <a name="performance-tuning-for-smb-file-servers"></a>Optimizar el rendimiento de los servidores de archivos SMB

## <a name="smb-configuration-considerations"></a>Consideraciones sobre la configuración de SMB
No habilite los servicios o características que no requieren el servidor de archivos y los clientes. Estos pueden incluir la firma SMB, almacenamiento en caché del lado cliente, mini-filtros de archivo del sistema, servicio de búsqueda, las tareas programadas, cifrado de NTFS, la compresión NTFS, IPSEC, filtros de firewall, Teredo y SMB cifrado.

Asegúrese de que el BIOS y modos de administración de energía del sistema operativo se establecen según sea necesario, que puede incluir el modo de alto rendimiento o modifica el estado C. Asegúrese de que el almacenamiento más resistente, más rápido y más reciente y controladores de dispositivos de red estén instalados.

Copia de archivos es una operación común que puede realizada en un servidor de archivos. Windows Server tiene varias utilidades de copia de archivos integrada que se pueden ejecutar mediante el uso de un símbolo del sistema. Se recomienda Robocopy. Introducido en Windows Server 2008 R2, el **/MT** opción de Robocopy puede mejorar significativamente la velocidad de transferencia de archivos remoto mediante el uso de varios subprocesos al copiar varios archivos pequeños. También se recomienda usar la **/log** opción para reducir la salida de la consola mediante el redireccionamiento de los registros a un dispositivo NUL o a un archivo. Al usar Xcopy, se recomienda agregar el **/q** y **/k** opciones a los parámetros existentes. La opción anterior reduce la sobrecarga CPU reduciendo la salida de la consola y éste reduce el tráfico de red.

## <a name="smb-performance-tuning"></a>Optimización del rendimiento de SMB


Rendimiento del servidor de archivos y los ajustes disponibles dependen en el protocolo SMB se negocia entre cada cliente y el servidor y las características de servidor de archivos implementados. La versión del protocolo más alta disponible actualmente es 3.1.1 SMB en Windows Server 2016 y Windows 10. Puede comprobar qué versión de SMB está en uso en la red mediante Windows PowerShell **Get SMBConnection** en los clientes y **Get SMBSession | FL** en servidores.

### <a name="smb-30-protocol-family"></a>Familia del protocolo SMB 3.0

SMB 3.0 se introdujo en Windows Server 2012 y aún más mejorado en Windows Server 2012 R2 (3.02 SMB) y Windows Server 2016 (SMB 3.1.1). Esta versión presenta tecnologías que pueden mejorar significativamente el rendimiento y la disponibilidad del servidor de archivos. Para obtener más información, consulte [SMB en Windows Server 2012 y 2012 R2 2012](https://aka.ms/smb3plus) y [cuáles son las novedades de SMB 3.1.1](https://aka.ms/smb311).



### <a name="smb-direct"></a>SMB directo

SMB directo se introdujo la capacidad para usar las interfaces de red RDMA de alto rendimiento con baja latencia y el uso de CPU reducido.

Cada vez que SMB detecta una red compatibles con RDMA, intenta automáticamente utilizar la funcionalidad RDMA. Sin embargo, si por alguna razón el cliente SMB no se puede conectar con la ruta de acceso RDMA, simplemente continuará usar conexiones TCP/IP en su lugar. Todas las interfaces RDMA que son compatibles con SMB directo son necesarios para implementar también una pila de TCP/IP, y es consciente de SMB multicanal.

SMB directo no se requiere en cualquier configuración de SMB, pero ' s siempre se recomienda para quienes desean una menor latencia y el uso de CPU inferior.

Para obtener más información acerca de SMB directo, consulte [mejorar el rendimiento de un servidor de archivos con SMB directo](https://aka.ms/smbdirect).

### <a name="smb-multichannel"></a>SMB multicanal

SMB multicanal permite que los servidores de archivos pueden utilizar simultáneamente varias conexiones de red y proporciona un mayor rendimiento.

Para obtener más información acerca de SMB multicanal, consulte [implementar SMB multicanal](https://aka.ms/smbmulti).

### <a name="smb-scale-out"></a>Escalabilidad horizontal SMB

Escalabilidad horizontal SMB permite que SMB 3.0 en una configuración de clúster para mostrar un recurso compartido en todos los nodos de un clúster. Esta configuración activa/activa permite a los clústeres de servidor de archivos de escala más allá, sin una configuración compleja con varios volúmenes, recursos compartidos y recursos de clúster. El ancho de banda de recurso compartido máximo es el ancho de banda total de todos los nodos de clúster de servidor de archivos. El ancho de banda total ya no está limitado por el ancho de banda de un solo nodo de clúster, pero en su lugar depende de la capacidad del sistema de almacenamiento de respaldo. Puede aumentar el ancho de banda total al agregar nodos.

Para obtener más información acerca de la escalabilidad horizontal SMB, consulte [Scale-Out File Server para información general sobre datos de aplicación](https://technet.microsoft.com/library/hh831349.aspx) y la entrada de blog [para escalar horizontalmente o no escalar horizontalmente, esa es la cuestión](http://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx).

### <a name="performance-counters-for-smb-30"></a>Contadores de rendimiento de SMB 3.0

Los siguientes contadores de rendimiento de SMB se introdujeron en Windows Server 2012 y se consideran un conjunto básico de contadores al supervisar el uso de recursos de SMB 2 y versiones posteriores. Iniciar los contadores de rendimiento en local, registro de contador de rendimiento sin procesar (.blg). Resulta menos costoso recopilar todas las instancias mediante el carácter comodín (\*) y, a continuación, extraiga determinadas instancias durante el procesamiento posterior mediante Relog.exe.

-   **Recursos compartidos de cliente SMB**

    Estos contadores muestran información sobre los recursos compartidos de archivos en el servidor que se está accediendo a un cliente que está usando SMB 2.0 o versiones posteriores.

    Si es ' familiarizado con los contadores de disco regulares en Windows, es posible que observe cierta algo. Que ' s no por accidente. Los contadores de rendimiento de los recursos compartidos de cliente SMB se diseñaron para coincidir exactamente con los contadores de disco. De este modo puede reutilizar fácilmente cualquier orientación sobre la optimización de rendimiento de disco de aplicación tiene actualmente. Para obtener más información sobre la asignación de contador, vea [por blog de los contadores de rendimiento de recurso compartido de cliente](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).

-   **Recursos compartidos del servidor SMB**

    Estos contadores muestran información sobre el SMB 2.0 o posterior de recursos compartidos de archivos en el servidor.

-   **Sesiones del servidor de SMB**

    Estos contadores muestran información acerca de las sesiones del servidor SMB que usan SMB 2.0 o superior.

    Activar contadores en el lado del servidor (recursos compartidos de servidor o las sesiones del servidor) puede tener una repercusión importante para altas cargas de trabajo de E/S.

-   **Filtro de clave de reanudación**

    Estos contadores muestran información sobre el filtro de clave de reanudación.

-   **Conexión directa de SMB**

    Estos contadores de medida diferentes aspectos de la actividad de conexión. Un equipo puede tener varias conexiones SMB directo. Los contadores de conexión directa de SMB representan cada conexión como un par de direcciones IP y puertos, donde la primera dirección IP y puerto representan extremo local de la conexión, y la segunda dirección IP y puerto de extremo remoto de la conexión.

-   **Disco físico, SMB, las relaciones de los contadores de rendimiento de FS CSV**

    Para obtener más información sobre cómo se relacionan los contadores (sistema de archivos) de disco físico, SMB y CSV FS, consulte el siguiente blog: [Contadores de rendimiento del volumen compartido del clúster](http://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx).

## <a name="tuning-parameters-for-smb-file-servers"></a>Parámetros de ajuste para los servidores de archivos SMB


El siguiente REG\_valores de DWORD del registro pueden afectar al rendimiento de los servidores de archivos SMB:

- **Smb2CreditsMin** y **Smb2CreditsMax**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
  ```

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
  ```

  Los valores predeterminados son 512 y 8192, respectivamente. Estos parámetros permiten al servidor limitar la simultaneidad de la operación de cliente dinámicamente dentro de los límites especificados. Algunos clientes podrían conseguir aumento del rendimiento con mayores límites de simultaneidad, por ejemplo, copiar archivos a través de vínculos de gran ancho de banda, latencia alta.
    
  > [!TIP]
  > Antes de Windows 10 y Windows Server 2016, el número de créditos concedidos al cliente varía dinámicamente entre Smb2CreditsMin y Smb2CreditsMax según un algoritmo que puede utilizar para determinar el número óptimo de créditos para conceder en función de la latencia de red y uso de crédito. En Windows 10 y Windows Server 2016, se cambió el servidor SMB para conceder incondicionalmente créditos a petición hasta el número máximo configurado de créditos. Como parte de este cambio, se ha quitado el crédito mecanismo, lo que reduce el tamaño de ventana de crédito de la conexión cuando el servidor está bajo presión de memoria, de limitación. Eventos de falta de memoria del kernel que desencadenó la limitación solo está señalado cuando el servidor de poco memoria (< unos MB) que sean inútiles. Puesto que el servidor ya no reduce el crédito windows la configuración de Smb2CreditsMin ya no es necesaria y ahora se omite.
  > 
  > Puede supervisar los recursos compartidos de cliente de SMB \\ /s se detiene de crédito para ver si hay algún problema con los créditos.

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    El valor predeterminado es 0, lo que significa que ningún subproceso de trabajo de kernel críticos adicionales se agrega. Este valor afecta al número de subprocesos que usa la memoria caché del sistema de archivos para las solicitudes de lectura anticipada y de escritura en segundo plano. Aumentar este valor puede permitir más en la cola de E/S en el subsistema de almacenamiento, y puede mejorar el rendimiento de E/S, especialmente en sistemas con muchos procesadores lógicos y hardware de almacenamiento eficaz.

    >[!TIP]
    > El valor es posible que tenga que aumentar si la cantidad de administrador de caché de integridad de datos (contador de rendimiento de caché\\páginas desfasadas) está creciendo a consumir una gran parte (más ~ 25%) de memoria o si el sistema está realizando una gran cantidad de sincrónica las operaciones de E/s de lectura.

- **MaxThreadsPerQueue**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
  ```

  El valor predeterminado es 20. Al aumentar este valor aumenta el número de subprocesos que puede usar el servidor de archivos para atender las solicitudes simultáneas. Cuando debe atender un gran número de conexiones activas y los recursos de hardware, como el ancho de banda de almacenamiento, son suficientes, aumentar el valor puede mejorar los tiempos de respuesta, el rendimiento y escalabilidad del servidor.

  >[!TIP]
  > Una indicación de que es posible que tenga que aumentar el valor es si las colas de trabajo SMB2 están creciendo muy grandes (contador de rendimiento "colas de trabajos de servidor\\longitud de cola\\SMB2 sin bloqueo \*' está constantemente por encima de 100 ~).

  >[!Note]
  >En Windows 10 y Windows Server 2016, MaxThreadsPerQueue no está disponible. El número de subprocesos para un grupo de subprocesos será "20 * el número de procesadores en un nodo NUMA".
     

- **AsynchronousCredits**

  ``` 
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
  ```

  El valor predeterminado es 512. Este parámetro limita el número de comandos SMB asincrónicos simultáneos que se permiten en una sola conexión. Algunos casos (por ejemplo, cuando hay un servidor front-end con un servidor IIS de back-end) requieren una gran cantidad de simultaneidad (por archivo cambiar solicitudes de notificación, en particular). El valor de esta entrada puede aumentarse para admitir estos casos.

### <a name="smb-server-tuning-example"></a>Ejemplo de optimización de servidor SMB

La siguiente configuración puede optimizar un equipo para el rendimiento del servidor de archivos en muchos casos. Los valores no son óptimos ni adecuados para todos los equipos. Debe evaluar el impacto de cada uno de los valores antes de aplicarlos.

| Parámetro                       | Valor | Default |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>Contadores del monitor de rendimiento de cliente SMB

Para obtener más información sobre los contadores del cliente SMB, consulte [sugerencia de servidor de archivos de Windows Server 2012: Nuevos contadores de rendimiento del cliente SMB por recurso compartido proporcionan una idea clara](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).
