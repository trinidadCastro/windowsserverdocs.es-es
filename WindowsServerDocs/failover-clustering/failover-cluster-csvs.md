---
title: Usar volúmenes compartidos de clúster en un clúster de conmutación por error
description: Cómo usar volúmenes compartidos de clúster en un clúster de conmutación por error.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f5bd0ad05bdc2573a5ea0abbe165de2d3e7f5c8f
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233521"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>Usar volúmenes compartidos de clúster en un clúster de conmutación por error

>Se aplica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Volúmenes compartidos de clúster (CSV) habilitar varios nodos en un clúster de conmutación por error al mismo tiempo tener acceso de lectura y escritura para el mismo LUN (disco) que está configurado como un volumen NTFS. (En Windows Server 2012 R2, el disco se puede aprovisionar como sistema de archivos resistente (ReFS) o NTFS.) Con CSV, roles agrupados en clúster pueden conmutar rápidamente desde un nodo a otro nodo sin necesidad de realizar un cambio en la propiedad de unidad, o desmontar y volver a montar un volumen. CSV también ayudan a simplificar la administración de un número potencialmente grande de LUN en un clúster de conmutación por error.

CSV proporcionan un sistema de archivos de propósito general, agrupados en clúster, que se sitúa en capas encima de NTFS (o ReFS en Windows Server 2012 R2). Las aplicaciones de CSV incluyen:

- Clúster de archivos de disco duro virtual (VHD) para máquinas virtuales en clúster Hyper-V
- Recursos compartidos de archivo de escalado horizontal para almacenar datos de aplicación para el rol servidor de archivos de escalado horizontal agrupado. Algunos ejemplos de los datos de aplicación para esta función son los archivos de máquina virtual de Hyper-V y datos de Microsoft SQL Server. (Tenga en cuenta que ReFS no es compatible con un servidor de archivos de escalado horizontal.) Para obtener más información acerca de servidor de archivos de escalado horizontal, vea [Servidor de archivos de escalado horizontal de datos de aplicación](sofs-overview.md).

>[!NOTE]
>CSV no es compatible con la carga de trabajo de clústeres de Microsoft SQL Server en SQL Server 2012 y versiones anteriores de SQL Server.

En Windows Server 2012, se ha mejorado considerablemente la funcionalidad CSV. Por ejemplo, se han eliminado las dependencias de los servicios de dominio de Active Directory. Se agregó compatibilidad para las mejoras funcionales en **chkdsk**, para la interoperabilidad con las aplicaciones de copia de seguridad y antivirus y para la integración con características de almacenamiento general, como los volúmenes cifrados mediante BitLocker y espacios de almacenamiento. Para obtener información general de la funcionalidad CSV que se introdujo en Windows Server 2012, vea [Novedades de agrupación en clústeres de conmutación por error en Windows Server 2012 \[redirected\]](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

Windows Server 2012 R2 presenta funciones adicionales, como la propiedad CSV distribuido, mayor resiliencia a través de la disponibilidad del servicio del servidor, mayor flexibilidad en la cantidad de memoria física que puede asignar a la memoria caché CSV, mejor diagnosibility y mejoras de interoperabilidad que incluyen compatibilidad con ReFS y desduplicación. Para obtener más información, vea [Novedades de agrupación en clústeres de conmutación por error](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

>[!NOTE]
>Para obtener información acerca del uso de desduplicación de datos en CSV para escenarios de infraestructura de Escritorio Virtual (VDI), consulte que el blog de entradas [de desduplicación de datos de implementación para el almacenamiento VDI en Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx) y [Extending la desduplicación de datos para las cargas de trabajo nuevo en Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx).

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>Revise los requisitos y consideraciones para el uso de CSV en un clúster de conmutación por error

Antes de usar CSV en un clúster de conmutación por error, revise la red, almacenamiento y otros requisitos y consideraciones de esta sección.

### <a name="network-configuration-considerations"></a>Consideraciones sobre la configuración de red

Tenga en cuenta lo siguiente al configurar las redes que admiten CSV.

- **Varias redes y varios adaptadores de red**. Para habilitar la tolerancia a errores en caso de error de red, se recomienda que varias redes de clúster transportar el tráfico de CSV o que configure colaborado adaptadores de red.
    
    Si los nodos del clúster están conectados a las redes que no se deben usar el clúster, debe deshabilitarlas. Por ejemplo, se recomienda que deshabilitar redes iSCSI para uso del clúster evitar que el tráfico CSV en esas redes. Para deshabilitar una red, en el Administrador de clúster de conmutación por error, seleccione **redes**, seleccione la red y, a continuación, seleccione la acción de **Propiedades** y, a continuación, seleccione **no permitir la comunicación de red de clúster en esta red**. Como alternativa, puede configurar la propiedad de la **función** de la red mediante el cmdlet de Windows PowerShell [Get-ClusterNetwork](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps) .
- **Propiedades del adaptador de red**. En las propiedades para todos los adaptadores que realizan la comunicación de los clústeres, asegúrese de que estén habilitadas las siguientes opciones:

  - **Cliente para redes Microsoft** y **archivos e impresoras para redes Microsoft**. Esta configuración admite mensaje bloque servidor (SMB) 3.0, que se usa de forma predeterminada para transportar el tráfico de CSV entre los nodos. Para habilitar SMB, asegúrese también de que se están ejecutando el servicio del servidor y el servicio de estación de trabajo y que están configurados para iniciarse automáticamente en cada nodo de clúster.

    >[!NOTE]
    >En Windows Server 2012 R2, hay varias instancias de servicio de servidor por cada nodo de clúster de conmutación por error. No hay la instancia predeterminada que controla el tráfico entrante de los clientes SMB que tienen acceso a recursos compartidos de archivos normales y una segunda instancia CSV que administra únicamente el tráfico CSV entre nodo. Además, si el servicio de servidor en un nodo se convierte en mal estado, propiedad CSV transiciones automáticamente a otro nodo.

    SMB 3.0 incluye las características multicanal de SMB y directo de SMB, que permiten el tráfico CSV en secuencia a través de varias redes del clúster y para sacar provecho de adaptadores de red que admiten la memoria de acceso directo remoto (RDMA). De forma predeterminada, multicanal de SMB se usa para el tráfico CSV. Para obtener más información, vea [información general de bloque de mensajes del servidor](../storage/file-server/file-server-smb-overview.md).
  - **Filtro de rendimiento de adaptador Virtual de clúster de conmutación por error de Microsoft**. Esta opción mejora la capacidad de nodos para llevar a cabo la redirección de E/S cuando es necesario ponerse en contacto con CSV, por ejemplo, cuando un error de conectividad impide que un nodo se conecten directamente en el disco CSV. Para obtener más información, vea [E/S acerca de la sincronización y la redirección de E/S en la comunicación de CSV](#about-i/o-synchronization-and-i/o-redirection-in-csv-communication) más adelante en este tema.
- **Asignación de prioridades de red de clúster**. Por lo general, se recomienda que no cambie las preferencias de clúster configurado para las redes.
- **Configuración de subred IP**. No es necesaria para los nodos en una red que use CSV ninguna configuración de subred específica. CSV puede admitir clústeres con varias subredes.
- **Calidad de servicio (QoS) basada en directiva**. Se recomienda que configure una directiva de prioridad de QoS y una directiva de ancho de banda mínimo para el tráfico de red a cada nodo cuando utilice CSV. Para obtener más información, vea [Calidad de servicio (QoS)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>).
- **Red de almacenamiento**. Para obtener recomendaciones de red de almacenamiento, revise las instrucciones proporcionadas por el proveedor de almacenamiento. Para obtener consideraciones adicionales acerca del almacenamiento para CSV, vea [requisitos de configuración de almacenamiento y de disco](#storage-and-disk-configuration-requirements) más adelante en este tema.

Para obtener información general del hardware, red y los requisitos de almacenamiento para los clústeres de conmutación por error, vea [requisitos de Hardware de agrupación en clústeres de conmutación por error y opciones de almacenamiento](clustering-requirements.md).

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>Acerca de la sincronización de E/S y la redirección de E/S en la comunicación de CSV

- **Sincronización de E/S**: CSV permite varios nodos que tengan acceso de lectura y escritura simultáneo para el mismo almacenamiento compartido. Cuando un nodo realiza entrada/salida (E/S) de disco en un volumen CSV, el nodo se comunica directamente con el almacenamiento, por ejemplo, a través de una red de área de almacenamiento (SAN). Sin embargo, en cualquier momento, un único nodo (denominado el nodo coordinador) "posee" el recurso de disco físico que está asociado con el LUN. Se muestra en el Administrador de clúster de conmutación por error en el nodo de coordinador para un volumen CSV como **Nodo propietario** en **discos**. También aparece en el resultado del cmdlet [Get-ClusterSharedVolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) Windows PowerShell.

  >[!NOTE]
  >En Windows Server 2012 R2, la propiedad CSV se distribuye uniformemente entre los nodos de clúster de conmutación por error en función del número de volúmenes CSV que posee cada nodo. Además, la propiedad automáticamente es reequilibrando cuando existen condiciones como CSV de conmutación por error, un nodo vuelve a unirse al clúster, agregar un nuevo nodo al clúster, reinicie un nodo de clúster o iniciar el clúster de conmutación por error después de que se ha cerrado.

  Cuando se producen determinados pequeños cambios en el sistema de archivos en un volumen CSV, se deben sincronizar estos metadatos en cada uno de los nodos físicos que tienen acceso a LUN, no sólo en el nodo de coordinador único. Por ejemplo, cuando se inicia, crea o elimina una máquina virtual en un volumen CSV, o cuando se migra una máquina virtual, esta información se necesita para que esté sincronizada en cada uno de los nodos físicos que tienen acceso a la máquina virtual. Estas operaciones de actualización de metadatos se producen en paralelo a través de las redes de clúster mediante el uso de SMB 3.0. Estas operaciones no requieren todos los nodos físicos para comunicarse con el almacenamiento compartido.

- **Redirección de E/S**: errores de conectividad de almacenamiento de información y determinadas operaciones de almacenamiento pueden impedir que un nodo determinado de comunicarse directamente con el almacenamiento de información. Para mantener la función mientras el nodo no se está comunicando con el almacenamiento, el nodo redirige la E/S de disco a través de una red de clúster para el nodo de coordinador donde está montado el disco. Si el nodo actual del coordinador experimenta un error de conectividad de almacenamiento, todas las operaciones de E/S de disco se ponen en cola temporalmente mientras se establece un nuevo nodo como un nodo de coordinador.

El servidor usa uno de los siguientes modos de redirección de E/S, dependiendo de la situación:

- **Redirección del sistema de archivos** La redirección es por volumen, por ejemplo, cuando las instantáneas CSV se toman por una aplicación de copia de seguridad cuando un volumen CSV manualmente se pone en modo de E/S redirigido.
- **Redirección de bloque** Redirección está en el nivel de bloqueo de archivos, por ejemplo, cuando se pierde a un volumen de la conectividad de almacenamiento de información. Redirección de bloque es mucho más rápido que la redirección del sistema de archivos.

En Windows Server 2012 R2, puede ver el estado de un volumen CSV en por nodos. Por ejemplo, puede ver si E/S es directa o redirigida, o si el volumen CSV no está disponible. Si un volumen CSV está en modo redirigido de E/S, también puede ver el motivo. Use el cmdlet **Get-ClusterSharedVolumeState** de Windows PowerShell para ver esta información.

>[!NOTE]
> * En Windows Server 2012, debido a las mejoras en el diseño CSV, CSV realizar más operaciones en el modo de E/S directa que ocurría en Windows Server 2008 R2.
> * Debido a la integración de CSV con características de SMB 3.0 como multicanal de SMB y directo de SMB, puede transmitir el tráfico de E/S redirigido a través de varias redes de clúster.
> * Debe planear las redes del clúster para permitir el aumento exponencial de tráfico de red para el nodo de coordinador durante la redirección de E/S.

### <a name="storage-and-disk-configuration-requirements"></a>Requisitos de configuración de almacenamiento y disco

Para usar CSV, el almacenamiento de información y los discos deben cumplir los siguientes requisitos:

- **Formato de archivo de sistema**. En Windows Server 2012 R2, un espacio de disco o de almacenamiento para un volumen CSV debe ser un disco básico con particiones con NTFS o ReFS. En Windows Server 2012, un espacio de disco o de almacenamiento para un volumen CSV debe ser un disco básico con particiones con NTFS.

  Un CSV tiene los siguientes requisitos adicionales:
    
  - En Windows Server 2012 R2, no puede usar un disco para un CSV que se ha dado formato con FAT o FAT32.
  - En Windows Server 2012, no puede usar un disco para un CSV que se ha dado formato con FAT, FAT32 o ReFS.
  - Si desea usar un espacio de almacenamiento para un CSV, puede configurar un espacio simple o un espacio de reflejo. En Windows Server 2012 R2, también puede configurar un espacio de paridad. (En Windows Server 2012, CSV no admite espacios paridad.)
  - No se puede usar un CSV como un disco de quórum testigo. Para obtener más información sobre el quórum del clúster, consulte [Understanding quórum en espacios de almacenamiento directo](../storage/storage-spaces/understand-quorum.md).
  - Después de agregar un disco como un CSV, se designa con el formato CSVFS (para el sistema de archivos CSV). Esto permite que el clúster y otro software para diferenciar el almacenamiento CSV desde otros NTFS o almacenamiento ReFS. Por lo general, CSVFS es compatible con la misma funcionalidad que NTFS o ReFS. Sin embargo, no se admiten determinadas características. Por ejemplo, en Windows Server 2012 R2, no se puede habilitar la compresión en CSV. En Windows Server 2012, no se puede habilitar la desduplicación de datos o la compresión en CSV.
- **Tipo de recurso en el clúster**. Para un volumen CSV, debe usar el tipo de recurso de disco físico. De forma predeterminada, se configura automáticamente un espacio de disco o de almacenamiento de información que se agrega al almacenamiento de clúster de este modo.
- **Opción de CSV discos u otros discos de almacenamiento del clúster**. Al elegir uno o más discos para una máquina virtual en clúster, tenga en cuenta cómo se usará cada disco. Si un disco se usará para almacenar los archivos que se crean mediante Hyper-V, como archivos VHD o los archivos de configuración, puede elegir entre los discos CSV o los otros discos disponibles en el almacenamiento del clúster. Si un disco será un disco físico que está directamente conectado a la máquina virtual (llamada también un disco de paso a través), no puede elegir un disco CSV y se debe elegir entre los otros discos disponibles en el almacenamiento de clúster de.
- **Nombre de ruta de acceso para la identificación de discos**. Discos en CSV se identifican con un nombre de ruta de acceso. Cada ruta de acceso parece estar en la unidad de sistema del nodo como un volumen numerado en la carpeta **\\ClusterStorage** . Esta ruta de acceso es la misma cuando se ve desde cualquier nodo del clúster. Puede cambiar el nombre de los volúmenes si es necesario.

Los requisitos de almacenamiento para CSV, revise las instrucciones proporcionadas por el proveedor de almacenamiento. Para las consideraciones de planeación de almacenamiento adicional para CSV, vea [Planear el uso de CSV en un clúster de conmutación por error](#plan-to-use-csv-in-a-failover-cluster) , más adelante en este tema.

### <a name="node-requirements"></a>Requisitos de nodo

Para usar CSV, los nodos deben cumplir los siguientes requisitos:

- **Letra de unidad de disco del sistema**. En todos los nodos, la letra de la unidad de disco del sistema debe ser el mismo.
- **Protocolo de autenticación**. El protocolo NTLM debe estar habilitado en todos los nodos. Esto está habilitado de forma predeterminada.

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>Planear el uso de CSV en un clúster de conmutación por error

En esta sección se enumera las recomendaciones para el uso de CSV en un clúster de conmutación por error que ejecuta Windows Server 2012 R2 o Windows Server 2012 y consideraciones de planeación.

>[!IMPORTANT]
>Pregunte al proveedor de almacenamiento para obtener recomendaciones acerca de cómo configurar su unidad de almacenamiento específico para CSV. Si las recomendaciones del proveedor de almacenamiento difieren de la información de este tema, siga las recomendaciones del proveedor de almacenamiento.

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>Disposición de LUN, volúmenes y archivos VHD

Para sacar el máximo provecho de CSV para proporcionar almacenamiento para máquinas virtuales en clúster, es útil revisar cómo se organizan los LUN (discos) al configurar los servidores físicos. Al configurar las máquinas virtuales correspondientes, pruebe a organizar los archivos VHD de manera similar.

Considere la posibilidad de un servidor físico para el que se organiza los discos y los archivos de la siguiente manera:

- Archivos de sistema, incluido un archivo de página, en un disco físico
- Archivos de datos en otro disco físico

Para una máquina virtual en clúster equivalente, debe organizar los volúmenes y archivos de forma similar:

- Archivos del sistema, incluyendo un archivo de página, en un archivo VHD en uno CSV
- Archivos de datos en un archivo VHD en otra CSV

Si agrega otra máquina virtual, siempre que sea posible, debe mantener la misma disposición para los archivos VHD de esa máquina virtual.

### <a name="number-and-size-of-luns-and-volumes"></a>Número y tamaño de LUN y volúmenes

Al planear la configuración de almacenamiento para un clúster de conmutación por error que usa CSV, tenga en cuenta las siguientes recomendaciones:

- Para decidir cuántos LUN para configurar, consulte a su proveedor de almacenamiento. Por ejemplo, es posible que recomienda el proveedor de almacenamiento que configurar cada LUN con una partición y colocar un volumen CSV en él.
- No existen limitaciones para el número de máquinas virtuales que se pueden admitir en un único volumen CSV. Sin embargo, debe tener en cuenta el número de máquinas virtuales que se va a tener en el clúster y la carga de trabajo (operaciones de E/S por segundo) para cada máquina virtual. Revisa los siguientes ejemplos:

  - Una organización va a implementar máquinas virtuales que sea compatible con una infraestructura de escritorio virtual (VDI), que es una carga de trabajo relativamente claro. El clúster utiliza almacenamiento de alto rendimiento. El Administrador de clústeres, después de consultar con el proveedor de almacenamiento, decide poner un número relativamente elevado de máquinas virtuales por volumen CSV.
  - Otra organización va a implementar un gran número de máquinas virtuales que sea compatible con una aplicación de base de datos muy utilizados, que es una mayor carga de trabajo. El clúster utiliza almacenamiento realizando inferior. El Administrador de clústeres, después de consultar con el proveedor de almacenamiento, decide poner un número relativamente pequeño de máquinas virtuales por volumen CSV.
- Al planear la configuración de almacenamiento para una máquina virtual determinada, tenga en cuenta los requisitos de disco del servicio, aplicación o rol que sea compatible con la máquina virtual. Descripción de estos requisitos, le ayuda a evitar la contención de disco que se puede producir un rendimiento deficiente. La configuración de almacenamiento para la máquina virtual estrechamente debería parecerse a la configuración de almacenamiento que usaría para un servidor físico que ejecuta el mismo servicio, aplicación o rol. Para obtener más información, vea [organización por conversaciones de LUN, volúmenes y los archivos VHD](#arrangement-of-luns,-volumes,-and-vhd-files) anteriormente en este tema.

    También puede mitigar contención del disco haciendo que el almacenamiento de información con un gran número de discos duros físicos independientes. Elija el hardware de almacenamiento según corresponda y póngase en contacto con su proveedor para optimizar el rendimiento de su almacenamiento de información.
- Dependiendo de las cargas de trabajo de clúster y su necesidad de operaciones de E/S, tenga en cuenta configurar sólo un porcentaje de las máquinas virtuales para tener acceso a cada LUN, mientras que otras máquinas virtuales no dispone de conectividad y en su lugar se dedicada para calcular las operaciones.

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>Agregar un disco a CSV en un clúster de conmutación por error

La característica CSV está habilitada de forma predeterminada en el clúster de conmutación por error. Para agregar un disco a CSV, debe agregar un disco al grupo de **Almacenamiento disponibles** del clúster (si no se ha añadido) y, a continuación, agregue el disco a CSV en el clúster. Puede usar el Administrador de clúster de conmutación por error o los cmdlets de Windows PowerShell de clústeres de conmutación por error para llevar a cabo estos procedimientos.

### <a name="add-a-disk-to-available-storage"></a>Agregar un disco a almacenamiento disponible

1. En el administrador del clúster de conmutación por error, en el árbol de consola, expanda el nombre del clúster y, a continuación, expanda **almacenamiento**.
2. Haga clic en **discos**y, a continuación, seleccione **Agregar disco**. Aparece una lista que muestra los discos que se pueden agregar para su uso en un clúster de conmutación por error.
3. Seleccione el disco o los discos que desea agregar y, a continuación, seleccione **Aceptar**.

    Los discos ahora se asignan al grupo de **Almacenamiento disponibles** .

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Comandos de Windows PowerShell equivalentes (agregar un disco a almacenamiento disponible)

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque es posible que aparezcan word ajustado en varias líneas aquí debido a las restricciones de formato.

En el ejemplo siguiente se identifica los discos que están listos para ser agregado al clúster y, a continuación, se agrega al grupo de **Almacenamiento disponibles** .

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>Agregar un disco de almacenamiento disponible a CSV

1. En el administrador del clúster de conmutación por error, en el árbol de consola, expanda el nombre del clúster, expanda **almacenamiento**y, a continuación, seleccione **discos**.
2. Seleccione uno o más discos que están asignados al **Espacio de almacenamiento disponible**, haga clic en la selección y, a continuación, seleccione **Agregar a volúmenes compartidos de clúster**.

    Los discos ahora se asignan al grupo de **Volumen compartido de clúster** del clúster. Los discos se exponen en cada nodo de clúster como volúmenes numerados (puntos de montaje) en la carpeta % SystemDisk % ClusterStorage. Los volúmenes aparecen en el sistema de archivos CSVFS.

>[!NOTE]
>Puede cambiar el nombre de los volúmenes CSV en la carpeta % SystemDisk % ClusterStorage.

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Comandos de Windows PowerShell equivalentes (agregar un disco a CSV)

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque es posible que aparezcan word ajustado en varias líneas aquí debido a las restricciones de formato.

El siguiente ejemplo agrega *1 de disco de clúster* en **Almacenamiento disponible** a CSV en el clúster local.

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>Habilitar la memoria caché CSV para cargas de trabajo de lectura intensiva (opcionales)

La memoria caché de CSV proporciona almacenamiento en caché en el nivel de bloque de operaciones de E/S sin búfer de sólo lectura mediante la asignación de memoria (RAM) del sistema como una memoria caché de escritura a través. (Las operaciones de E/S sin búfer no almacena en caché el Administrador de caché.) Esto puede mejorar el rendimiento para aplicaciones como Hyper-V, que lleva a cabo operaciones de E/S sin búfer al obtener acceso a un disco duro virtual. La memoria caché de CSV puede aumentar el rendimiento de las solicitudes de lectura sin almacenamiento en caché de las solicitudes de escritura. Habilitación de la memoria caché CSV también es útil para escenarios de servidor de archivos de escalado horizontal.

>[!NOTE]
>Se recomienda que habilite la memoria caché de CSV para implementaciones agrupados en clústeres de Hyper-V y servidor de archivos de escalado horizontal.

De forma predeterminada en Windows Server 2012, se deshabilita la memoria caché de CSV. En Windows Server 2012 R2, la memoria caché de CSV está habilitada de forma predeterminada. Sin embargo, aún debe asignar el tamaño de la memoria caché de bloque para reservar.

En la siguiente tabla se describe las dos opciones de configuración que controlan la memoria caché de CSV.

<table>
<thead>
<tr class="header">
<th>Nombre de la propiedad en Windows Server 2012 R2</th>
<th>Nombre de la propiedad en Windows Server 2012</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>BlockCacheSize</strong></td>
<td><strong>SharedVolumeBlockCacheSizeInMB</strong></td>
<td>Se trata de una propiedad común de clúster que le permite definir la cantidad de memoria (en megabytes) para reservar para la memoria caché de CSV en cada nodo del clúster. Por ejemplo, si se define un valor de 512, 512 MB de memoria del sistema está reservado en cada nodo. (En muchos de los clústeres, 512 MB es un valor recomendado). El valor predeterminado es 0 (para deshabilitada).</td>
</tr>
<tr class="even">
<td><strong>EnableBlockCache</strong></td>
<td><strong>CsvEnableBlockCache</strong></td>
<td>Esto es una propiedad privada del recurso de disco físico de clúster. Permite habilitar la caché CSV en un disco individual que se agrega a CSV. En Windows Server 2012, el valor predeterminado es 0 (para deshabilitada). Para habilitar la caché CSV en un disco, configure un valor de 1. De forma predeterminada, en Windows Server 2012 R2, esta opción está habilitada.</td>
</tr>
</tbody>
</table>

Puede supervisar la memoria caché de CSV en Monitor de rendimiento mediante la adición de los contadores de **Caché del clúster CSV volumen**.

#### <a name="configure-the-csv-cache"></a>Configurar la memoria caché CSV

1. Inicie Windows PowerShell como administrador.
2. Para definir una memoria caché de *512* MB para reservar en cada nodo, escriba lo siguiente:

    - Para Windows Server 2012 R2:

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - Para Windows Server 2012:

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. En Windows Server 2012, para habilitar la memoria caché de CSV en un CSV denominado *1 de disco del clúster*, escriba lo siguiente:

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * En Windows Server 2012, puede asignar sólo el 20% de la RAM física total en la memoria caché CSV. En Windows Server 2012 R2, puede asignar hasta un 80%. Debido a que los servidores de archivos de escalado horizontal no suelen memoria limitada, puede lograr ganancias en el rendimiento de gran tamaño mediante el uso de la memoria adicional para la memoria caché de CSV.
> * Para evitar la contención de recursos, debe reiniciar cada nodo del clúster después de modificar la memoria que se asigna a la memoria caché CSV. En Windows Server 2012 R2, ya no se requiere un reinicio.
> * Después de habilitar o deshabilitar la memoria caché CSV en un disco individual, para que la configuración surta efecto, debe desconectar el recurso de disco físico y ponerlo en línea. (De forma predeterminada, en Windows Server 2012 R2, se habilita la memoria caché de CSV). 
> * Para obtener más información acerca de la memoria caché CSV que incluye información acerca de los contadores de rendimiento, vea la entrada de blog [cómo habilitar la memoria caché de CSV](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/).

## <a name="back-up-csv"></a>Copia de seguridad de CSV

Existen varios métodos para realizar una copia de seguridad de la información que se almacena en CSV en un clúster de conmutación por error. Puede usar una aplicación de copia de seguridad de Microsoft o una aplicación que no es de Microsoft. En general, CSV imponer requisitos especiales de copia de seguridad más allá de los para el almacenamiento agrupado con formato NTFS o ReFS. Las copias de seguridad CSV no interrumpe también otras operaciones de almacenamiento CSV.

Cuando seleccione una aplicación de copia de seguridad y la programación de copia de seguridad para CSV se deben tener en cuenta los siguientes factores:

- Copia de seguridad de nivel de volumen de un volumen CSV se puede ejecutar desde cualquier nodo que se conecta al volumen CSV.
- La aplicación de copia de seguridad puede usar instantáneas de software o instantáneas de hardware. Dependiendo de la capacidad de la aplicación de copia de seguridad para admitir dichas, las copias de seguridad pueden usar instantáneas de servicio de instantáneas de volumen (VSS) intensivo coherente y consistente con las aplicaciones.
- Si hace una copia de CSV que tienen varias máquinas virtuales que se está ejecutando, por lo general debe elegir un método de copia de seguridad basado en el sistema operativo de administración. Si lo admite la aplicación de copia de seguridad, varias máquinas virtuales se copia de seguridad puede llamar simultáneamente.
- CSV admite la copia de seguridad solicitantes que ejecutan Windows Server 2012 R2 copia de seguridad, copia de seguridad de Windows Server 2012 o Windows Server 2008 R2 copia de seguridad. Sin embargo, copia de seguridad de Windows Server generalmente, ofrece sólo una copia de seguridad solución básica que puede no ser adecuada para las organizaciones con clústeres de mayor tamaño. Copia de seguridad de Windows Server no admite la copia de seguridad de máquina virtual consistente con las aplicaciones en CSV. Admite intensivo coherente copia de seguridad de nivel de volumen sólo. (Si restaura una copia de seguridad intensivo coherente, la máquina virtual estará en el mismo estado era si tenía se bloqueó la máquina virtual en el momento exacto que se ha realizado la copia de seguridad.) Una copia de seguridad de una máquina virtual en un volumen CSV se realizará correctamente, pero se registrará un evento de error que indica que no es compatible.
- Es posible que requieran credenciales administrativas cuando la copia de seguridad de un clúster de conmutación por error.

>[!IMPORTANT]
>Asegúrese de revisar cuidadosamente qué datos realiza una copia de la aplicación de copia de seguridad y restaura las características que CSV que admite, y los requisitos de recursos para la aplicación en cada nodo de clúster.

>[!WARNING]
>Si necesita restaurar los datos de copia de seguridad en un volumen CSV, tenga en cuenta las capacidades y limitaciones de la aplicación de copia de seguridad para mantener y restaurar datos consistentes con las aplicaciones a través de los nodos del clúster. Por ejemplo, con algunas aplicaciones, si se restaura el CSV en un nodo que es diferente del nodo donde se copia el volumen CSV, podría sin darse cuenta sobrescribir datos importantes sobre el estado de la aplicación en el nodo donde está produciendo la restauración.

## <a name="more-information"></a>Más información

- [Clúster de conmutación por error](failover-clustering.md)
- [Implementación de espacios de almacenamiento en clúster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)