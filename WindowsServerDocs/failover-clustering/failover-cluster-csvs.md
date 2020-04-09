---
title: Usar volúmenes compartidos de clúster en un clúster de conmutación por error
description: Cómo usar volúmenes compartidos de clúster en un clúster de conmutación por error.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.technology: storage-failover-clustering
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: 1d275e0379b5374899437bcf1f0387b304350840
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827748"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>Usar volúmenes compartidos de clúster en un clúster de conmutación por error

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Los volúmenes compartidos de clúster (CSV) permiten a varios nodos de un clúster de conmutación por error tener simultáneamente acceso de lectura y escritura al mismo LUN (disco) que se aprovisiona como volumen NTFS. (En Windows Server 2012 R2, el disco se puede aprovisionar como NTFS o sistema de archivos resistente (ReFS)). Con CSV, los roles en clúster pueden conmutar por error rápidamente de un nodo a otro sin necesidad de un cambio en la propiedad de la unidad o de desmontar y volver a montar un volumen. CSV también puede ayudar a simplificar la administración de una cantidad potencialmente grande de LUN en un clúster de conmutación por error.

CSV proporciona un sistema de archivos en clúster de uso general, que se encuentra superpuesto a NTFS (o ReFS en Windows Server 2012 R2). CSV se puede aplicar en los siguientes casos:

- Archivos de disco duro virtual (VHD) en clúster para máquinas virtuales de Hyper-V en clúster.
- Recursos compartidos de archivos de escalabilidad horizontal para almacenar datos de aplicación para el rol en clúster Servidor de archivos de escalabilidad horizontal. Son ejemplos de los datos de aplicación para este rol los archivos de máquinas virtuales de Hyper-V y datos de Microsoft SQL Server. (Tenga en cuenta que no se admite ReFS para un Servidor de archivos de escalabilidad horizontal). Para obtener más información acerca de Servidor de archivos de escalabilidad horizontal, vea [servidor de archivos de escalabilidad horizontal de datos de aplicaciones](sofs-overview.md).

> [!NOTE]
> CSV no admite la carga de trabajo en clúster Microsoft SQL Server en SQL Server 2012 y versiones anteriores de SQL Server.

En Windows Server 2012, la funcionalidad de CSV se mejoró considerablemente. Por ejemplo, se quitaron las dependencias en Servicios de dominio de Active Directory. Se agregó compatibilidad con las mejoras funcionales de **chkdsk**, con la interoperabilidad con aplicaciones antivirus y de copia de seguridad, y con la integración con características de almacenamiento generales como los volúmenes cifrados con BitLocker y Espacios de almacenamiento. Para obtener información general sobre la funcionalidad de CSV que se presentó en Windows Server 2012, consulte [novedades de los clústeres de conmutación por error en Windows server 2012 \[redirigido\]](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

Windows Server 2012 R2 incorpora funcionalidad adicional, como la propiedad de CSV distribuida, el aumento de la resistencia a través de la disponibilidad del servicio de servidor, mayor flexibilidad en la cantidad de memoria física que se puede asignar a la caché de CSV, una mejor capacidad y una interoperabilidad mejorada que incluye compatibilidad con ReFS y desduplicación. Para obtener más información, consulte [novedades de los clústeres de conmutación por error](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

> [!NOTE]
> Para obtener información acerca del uso de desduplicación de datos en CSV para escenarios de infraestructura de Escritorio Virtual (VDI), consulte las publicaciones del blog [Implementar la desduplicación de datos para el almacenamiento VDI en Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx) y [Ampliación de desduplicación de datos para nuevas cargas de trabajo en Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx).

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>Revisar los requisitos y las consideraciones para usar CSV en un clúster de conmutación por error

Antes de usar CSV en un clúster de conmutación por error, revisa los requisitos y las consideraciones relativos a la red, el almacenamiento y otros aspectos que se incluyen en esta sección.

### <a name="network-configuration-considerations"></a>Consideraciones de configuración de red

Ten en cuenta lo siguiente al configurar las redes compatibles con CSV.

- **Varias redes y varios adaptadores de red**. Para habilitar la tolerancia a errores en caso de error de red, te recomendamos que varias redes en clúster transporten el tráfico de CSV o que configures adaptadores de red combinados.
    
    Si los nodos del clúster están conectados a redes que no debería usar el clúster, debes deshabilitarlos. Por ejemplo, te recomendamos que deshabilites las redes iSCSI para el uso del clúster a fin de impedir el tráfico de CSV en esas redes. Para deshabilitar una red, en Administrador de clústeres de conmutación por error, seleccione **redes**, seleccione la red, seleccione la acción **propiedades** y, a continuación, seleccione no **permitir la comunicación de red de clústeres en esta red**. Como alternativa, puede configurar la propiedad **role** de la red mediante el cmdlet [Get-ClusterNetwork](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps) de Windows PowerShell.
- **Propiedades de los adaptadores de red**. En las propiedades de todos los adaptadores que transporten comunicaciones del clúster, asegúrate de habilitar las siguientes opciones de configuración:

  - **Cliente para redes Microsoft** y **Compartir impresoras y archivos para redes Microsoft**. Estas opciones de configuración son compatibles con la versión 3.0 del Bloque de mensajes del servidor (SMB), que se usa de manera predeterminada para transportar el tráfico de CSV entre nodos. Para habilitar SMB, asegúrate también de que los servicios Servidor y Estación de trabajo se estén ejecutando y que estén configurados para iniciarse automáticamente en cada nodo del clúster.

    >[!NOTE]
    >En Windows Server 2012 R2, hay varias instancias del servicio servidor por nodo de clúster de conmutación por error. Tenemos la instancia predeterminada que se encarga del tráfico entrante de los clientes SMB que tienen acceso a recursos compartidos de archivos normales y una segunda instancia de CSV que solo se encarga del tráfico CSV entre nodos. Además, si el servicio Servidor de un nodo tiene un estado incorrecto, la propiedad de CSV pasa automáticamente a otro nodo.

    SMB 3.0 incluye las características SMB multicanal y SMB directo, lo que permite que el tráfico de CSV se transmita en secuencias entre varias redes del clúster y que se aprovechen los adaptadores de red compatibles con el acceso directo a memoria remota (RDMA). De manera predeterminada, SMB multicanal se usa para el tráfico de CSV. Para obtener más información, consulta [Información general de Bloque de mensajes del servidor](../storage/file-server/file-server-smb-overview.md).
  - **Filtro de rendimiento del adaptador virtual de clúster de conmutación por error de Microsoft**. Esta opción de configuración mejora la capacidad de los nodos de realizar la redirección de E/S cuando es necesaria para comunicarse con CSV, por ejemplo, cuando un error de conectividad impide a un nodo conectarse directamente al disco CSV. Para obtener más información, vea acerca de la [sincronización de e/s y la redirección de e/s en la comunicación de CSV](#about-io-synchronization-and-io-redirection-in-csv-communication) más adelante en este tema.
- **Prioridades de las redes en clúster**. Por lo general, recomendamos que no cambie las preferencias configuradas en el clúster para las redes.
- **Configuración de subred IP**. No se necesita ninguna configuración de subred específica para los nodos de una red que usen CSV. CSV puede admitir clústeres de varias subredes.
- **Calidad de servicio (QoS) basada en directivas**. Te recomendamos que configures una directiva de prioridad de QoS y una directiva de ancho de banda mínimo para el tráfico de red a cada nodo cuando uses CSV. Para obtener más información, consulte [calidad de servicio (QoS)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>).
- **Red de almacenamiento**. Para obtener recomendaciones sobre la red de almacenamiento, repasa las instrucciones proporcionadas por el proveedor de almacenamiento. Para obtener consideraciones adicionales sobre el almacenamiento para CSV, consulte [requisitos de configuración de almacenamiento y disco](#storage-and-disk-configuration-requirements) más adelante en este tema.

Para obtener información general de los requisitos de hardware, red y almacenamiento para los clústeres de conmutación por error, consulte [Failover Clustering Hardware Requirements and Storage Options](clustering-requirements.md).

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>Acerca de la sincronización de E/S y la redirección de E/S en la comunicación de CSV

- **Sincronización de e/s**: CSV permite que varios nodos tengan acceso de lectura y escritura simultáneo al mismo almacenamiento compartido. Cuando un nodo realiza tareas de entrada/salida (E/S) de disco en un volumen CSV, el nodo se comunica directamente con el almacenamiento, por ejemplo, por medio de una red de área de almacenamiento (SAN). Sin embargo, en cualquier momento, un solo nodo (denominado el nodo Coordinador) "posee" el recurso de disco físico que está asociado con el LUN. El nodo coordinador de un volumen CSV se muestra en el Administrador de clústeres de conmutación por error como **Nodo propietario** en **Discos**. También aparece en la salida del cmdlet [Get-ClusterSharedVolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) de Windows PowerShell.

  >[!NOTE]
  >En Windows Server 2012 R2, la propiedad de CSV se distribuye uniformemente entre los nodos de clúster de conmutación por error en función del número de volúmenes CSV que posee cada nodo. Además, la propiedad se reequilibra automáticamente cuando hay condiciones como la conmutación por error de CSV, un nodo que se vuelve a unir al clúster, la adición de un nodo nuevo al clúster, el reinicio de un nodo del clúster o el inicio del clúster de conmutación por error después de que se haya apagado.

  Cuando se producen ciertos pequeños cambios en el sistema de archivos de un volumen CSV, estos metadatos se deben sincronizar en cada uno de los nodos físicos que obtienen acceso al LUN, no solo en el nodo coordinador. Por ejemplo, cuando se inicia, se crea o se elimina una máquina virtual en un volumen CSV o cuando se migra una máquina virtual, esta información se debe sincronizar en cada uno de los nodos físicos que tienen acceso a la máquina virtual. Estas operaciones de actualización de metadatos se producen en paralelo en las redes en clúster mediante SMB 3.0. Estas operaciones no requieren que todos los nodos físicos se comuniquen con el almacenamiento compartido.

- **Redirección de e/s**: los errores de conectividad de almacenamiento y ciertas operaciones de almacenamiento pueden impedir que un nodo determinado se comunique directamente con el almacenamiento. Para mantener el funcionamiento mientras el nodo no se comunica con el almacenamiento, el nodo redirige la E/S del disco a través de una red en clúster al nodo coordinador en el que está montado el disco actualmente. Si el nodo coordinador actual experimenta un error de conectividad de almacenamiento, todas las operaciones de E/S de disco se ponen temporalmente en cola mientras se establece un nuevo nodo como nodo coordinador.

El servidor usa uno de los modos de redirección de E/S siguientes, en función de la situación:

- **Redirección del sistema de archivos** La redirección se realiza por volumen, por ejemplo, cuando una aplicación de copia de seguridad toma instantáneas de CSV cuando se coloca un volumen CSV manualmente en modo de E/S redirigida.
- **Redirección de bloque** La redirección se realiza en el nivel de bloque de archivos, por ejemplo, cuando se pierde la conectividad de almacenamiento a un volumen. La redirección de bloque es significativamente más rápida que la redirección del sistema de archivos.

En Windows Server 2012 R2, puede ver el estado de un volumen CSV por cada nodo. Por ejemplo, puedes ver si la E/S es directa o redirigida o si el volumen CSV no está disponible. Si un volumen CSV está en modo de E/S redirigida, también puedes ver el motivo. Usa el cmdlet **Get-ClusterSharedVolumeState** de Windows PowerShell para ver esta información.

> [!NOTE]
> * En Windows Server 2012, debido a las mejoras en el diseño de CSV, CSV realiza más operaciones en modo de e/s directa que en Windows Server 2008 R2.
> * Debido a la integración de CSV con características de SMB 3.0 como SMB multicanal y SMB directo, el tráfico de E/S redirigida se puede emitir en secuencias a través de varias redes en clúster.
> * Deberías planear las redes en clúster de manera que permitan un posible aumento del tráfico de red al nodo coordinador durante la redirección de E/S.

### <a name="storage-and-disk-configuration-requirements"></a>Requisitos de configuración de almacenamiento y disco

Para usar CSV, el almacenamiento y los discos deben cumplir con los siguientes requisitos:

- **Formato del sistema de archivos**. En Windows Server 2012 R2, un disco o espacio de almacenamiento para un volumen CSV debe ser un disco básico con particiones NTFS o ReFS. En Windows Server 2012, un disco o espacio de almacenamiento para un volumen CSV debe ser un disco básico con particiones NTFS.

  Un CSV tiene los siguientes requisitos adicionales:
    
  - En Windows Server 2012 R2, no puede usar un disco para un CSV que tenga el formato FAT o FAT32.
  - En Windows Server 2012, no se puede usar un disco para un CSV que tenga el formato FAT, FAT32 o ReFS.
  - Si deseas un espacio de almacenamiento para un CSV, puedes configurar un espacio simple o un espacio reflejado. En Windows Server 2012 R2, también puede configurar un espacio de paridad. (En Windows Server 2012, CSV no admite los espacios de paridad).
  - Un CSV no se puede usar como disco testigo de cuórum. Para obtener más información acerca del cuórum de clúster, consulte [Descripción del cuórum en espacios de almacenamiento directo](../storage/storage-spaces/understand-quorum.md).
  - Después de agregar un disco como CSV, se designa en el formato CSVFS (Sistema de archivos CSV). Esto permite al clúster y a otros programas de software diferenciar el almacenamiento CSV de otro almacenamiento NTFS o ReFS. Por lo general, CSVFS es compatible con la misma funcionalidad que NTFS o ReFS. No obstante, hay ciertas características que no son compatibles. Por ejemplo, en Windows Server 2012 R2, no se puede habilitar la compresión en CSV. En Windows Server 2012, no se puede habilitar la desduplicación de datos o la compresión en CSV.
- **Tipo de recursos del clúster**. Para un volumen CSV, debes usar el tipo de recursos de disco físico. De manera predeterminada, un disco o espacio de almacenamiento que se agregue al almacenamiento de clúster se configura automáticamente de este modo.
- **Elección de discos CSV u otros discos en el almacenamiento de clúster**. Al elegir uno o más discos para una máquina virtual en clúster, ten en cuenta el uso que se dará a cada disco. Si un disco se va a usar para almacenar archivos creados por Hyper-V, como archivos VHD o archivos de configuración, puedes elegir entre los discos CSV o los otros discos disponibles en el almacenamiento de clúster. Si un disco va a ser el disco físico conectado directamente a la máquina virtual (también conocido como disco de acceso directo), no puedes elegir un disco CSV y debes elegir uno de los otros discos disponibles en el almacenamiento de clúster.
- **Nombre de ruta de acceso para identificar discos**. Los discos en CSV se identifican con un nombre de ruta de acceso. Cada ruta de acceso aparece en la unidad del sistema del nodo como volumen numerado en la carpeta **\\ClusterStorage** Esta ruta de acceso es la misma cuando se ve desde cualquier nodo del clúster. Puedes cambiar de nombre los volúmenes si es necesario.

Para obtener los requisitos de almacenamiento para CSV, repasa las instrucciones proporcionadas por el proveedor de almacenamiento. Para ver consideraciones adicionales de planificación del almacenamiento para CSV, consulta [Planear el uso de CSV en un clúster de conmutación por error](#plan-to-use-csv-in-a-failover-cluster) más adelante en este tema.

### <a name="node-requirements"></a>Requisitos de nodos

Para usar CSV, los nodos deben cumplir con los siguientes requisitos:

- **Letra de unidad del disco del sistema**. La letra de unidad del disco del sistema debe ser la misma en todos los nodos.
- **Protocolo de autenticación**. El protocolo NTLM debe estar habilitado en todos los nodos. Este valor está habilitado de forma predeterminada.

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>Planear el uso de CSV en un clúster de conmutación por error

En esta sección se enumeran las consideraciones y recomendaciones de planificación para usar CSV en un clúster de conmutación por error que ejecuta Windows Server 2012 R2 o Windows Server 2012.

> [!IMPORTANT]
> Solicita al proveedor de almacenamiento recomendaciones sobre cómo configurar tu unidad de almacenamiento concreta para CSV. Si las recomendaciones del proveedor de almacenamiento difieren de la información de este tema, sigue las recomendaciones del proveedor de almacenamiento.

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>Organización de LUN, volúmenes y archivos VHD

Para aprovechar al máximo CSV a fin de proporcionar almacenamiento para máquinas virtuales en clúster, resulta útil repasar cómo organizarías los LUN (discos) al configurar servidores físicos. Cuando configures las máquinas virtuales correspondientes, intenta organizar los archivos VHD de manera parecida.

Considera un servidor físico para el que organizarías los discos y archivos de la siguiente manera:

- Archivos del sistema, incluido un archivo de paginación, en un disco físico
- Archivos de datos en otro disco físico

En una máquina virtual en clúster equivalente, deberías organizar los volúmenes y archivos de manera parecida:

- Archivos del sistema, incluido un archivo de paginación, en un archivo VHD en un CSV
- Archivos de datos en un archivo VHD en otro CSV

Si agregas otra máquina virtual, siempre que sea posible, deberías organizar los VHD de la misma manera que en las otras.

### <a name="number-and-size-of-luns-and-volumes"></a>Cantidad y tamaño de LUN y volúmenes

Cuando planees la configuración del almacenamiento de un clúster de conmutación por error que use CSV, ten en cuenta las recomendaciones siguientes:

- Para decidir cuántos LUN configurarás, consulta al proveedor de almacenamiento. Por ejemplo, el proveedor de almacenamiento te podría recomendar configurar cada LUN con una partición y colocar un volumen CSV en ella.
- No hay limitaciones en cuanto a la cantidad de máquinas virtuales que se pueden admitir en un solo volumen CSV. No obstante, debes tener en cuenta la cantidad de máquinas virtuales que tienes previsto tener en el clúster y la carga de trabajo (operaciones de E/S por segundo) para cada máquina virtual. Revisa los siguientes ejemplos:

  - Una organización implementa máquinas virtuales que admitirán una infraestructura de escritorio virtual (VDI), lo que supone una carga de trabajo relativamente ligera. El clúster usa almacenamiento de alto rendimiento. El administrador del clúster, después de consultar al proveedor de almacenamiento, decide colocar una cantidad relativamente alta de máquinas virtuales por volumen CSV.
  - Otra organización implementa una gran cantidad de máquinas virtuales que admitirán una aplicación de base de datos que registra un uso muy elevado, lo cual supone una carga de trabajo más pesada. El clúster usa almacenamiento de bajo rendimiento. El administrador del clúster, después de consultar al proveedor de almacenamiento, decide colocar una cantidad relativamente baja de máquinas virtuales por volumen CSV.
- Al planear la configuración del almacenamiento de una máquina virtual concreta, ten en cuenta los requisitos de disco del servicio, la aplicación o el rol que admitirá la máquina virtual. Comprender estos requisitos te ayudará a evitar la contención de disco, que puede causar un bajo rendimiento. La configuración de almacenamiento de la máquina virtual debe parecerse mucho a la configuración de almacenamiento que usarías para un servidor físico que ejecute el mismo servicio, la misma aplicación o el mismo rol. Para obtener más información, vea [organización de LUN, volúmenes y archivos VHD](#arrangement-of-luns-volumes-and-vhd-files) anteriormente en este tema.

    También puedes mitigar la contención de disco si tienes almacenamiento con gran cantidad de discos duros físicos independientes. Elige el hardware de almacenamiento en consecuencia y consulta al proveedor cómo puedes optimizar el rendimiento del almacenamiento.
- En función de las cargas de trabajo del clúster y las operaciones de E/S que necesiten, puedes considerar la opción de configurar solamente un porcentaje de las máquinas virtuales para tener acceso a cada LUN, mientras que otras máquinas virtuales no tendrán conectividad y se dedicarán a operaciones de cálculo.

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>Agregar un disco a CSV en un clúster de conmutación por error

La característica CSV está habilitada de manera predeterminada en clústeres de conmutación por error. Para agregar un disco a CSV, debes agregarlo al grupo **Almacenamiento disponible** del clúster (si todavía no se ha agregado) y, después, agregar el disco a CSV en el clúster. Puede usar Administrador de clústeres de conmutación por error o los cmdlets de Windows PowerShell de clústeres de conmutación por error para realizar estos procedimientos.

### <a name="add-a-disk-to-available-storage"></a>Agregar un disco al almacenamiento disponible

1. En el árbol de la consola del Administrador de clústeres de conmutación por error, expande el nombre del clúster y, luego, **Almacenamiento**.
2. Haga clic con el botón secundario en **discos**y seleccione **Agregar disco**. Aparece una lista en la que se muestran los discos que se pueden agregar para usarse en un clúster de conmutación por error.
3. Seleccione el disco o los discos que desea agregar y, a continuación, seleccione **Aceptar**.

    Ahora, los discos están asignados al grupo **Almacenamiento disponible** .

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Comandos equivalentes de Windows PowerShell (agregar un disco al almacenamiento disponible)

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.

En el ejemplo siguiente se identifican los discos que están preparados para agregarse al clúster y, después, se agregan estos discos al grupo **Almacenamiento disponible**.

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>Agregar un disco en almacenamiento disponible a CSV

1. En Administrador de clústeres de conmutación por error, en el árbol de consola, expanda el nombre del clúster, expanda **almacenamiento**y, a continuación, seleccione **discos**.
2. Seleccione uno o varios discos asignados a **almacenamiento disponible**, haga clic con el botón derecho en la selección y, a continuación, seleccione **Agregar a volúmenes compartidos de clúster**.

    Los discos se asignan al grupo **Volumen compartido de clúster** del clúster. Los discos se muestran en cada nodo del clúster como volúmenes numerados (puntos de montaje), en la carpeta %SystemDisk%ClusterStorage. Los volúmenes aparecen en el sistema de archivos CSVFS.

>[!NOTE]
>Puedes cambiar los nombres de los volúmenes CSV en la carpeta %SystemDisk%ClusterStorage.

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Comandos equivalentes de Windows PowerShell (agregar un disco a CSV)

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.

En el ejemplo siguiente se agrega *Cluster Disk 1* en **Almacenamiento disponible** a CSV en el clúster local.

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>Habilitar la memoria caché de CSV para cargas de trabajo con muchas operaciones de lectura (opcional)

La memoria caché de CSV proporciona almacenamiento en caché en el nivel de bloque de operaciones de E/S no almacenadas en búfer de solo lectura por medio de la asignación de memoria del sistema (RAM) como caché de escritura a través. (El administrador de caché no almacena en caché las operaciones de e/s no almacenadas en búfer). Esto puede mejorar el rendimiento de aplicaciones como Hyper-V, que realiza operaciones de e/s no almacenadas en búfer al obtener acceso a un disco duro virtual. La memoria caché de CSV puede mejorar el rendimiento de las solicitudes de lectura sin almacenar en caché las solicitudes de escritura. Habilitar la memoria caché de CSV también es útil para escenarios de servidor de archivos de escalabilidad horizontal.

>[!NOTE]
>Te recomendamos que habilites la memoria caché de CSV para todas las implementaciones de Hyper-V y servidor de archivos de escalabilidad horizontal en clúster.

De forma predeterminada en Windows Server 2012, la memoria caché de CSV está deshabilitada. En Windows Server 2012 R2 y versiones posteriores, la caché de CSV está habilitada de forma predeterminada. No obstante, debes asignar el tamaño de la memoria caché de bloque que deseas reservar.

En la tabla siguiente se describen las dos opciones de configuración que controlan la memoria caché de CSV.

| Windows Server 2012 R2 y versiones posteriores |  Windows Server 2012                 | Descripción |
| -------------------------------- | ------------------------------------ | ----------- |
| BlockCacheSize                   | SharedVolumeBlockCacheSizeInMB       | Se trata de una propiedad común del clúster que permite definir cuánta memoria (en megabytes) deseas reservar para la memoria caché de CSV en cada nodo del clúster. Por ejemplo, si se define un valor de 512, se reservan 512 MB de memoria del sistema en cada nodo. (En muchos clústeres, 512 MB es un valor recomendado). La configuración predeterminada es 0 (deshabilitada). |
| EnableBlockCache                 | CsvEnableBlockCache                  | Se trata de una propiedad privada del recurso de disco físico del clúster. Permite habilitar la memoria caché de CSV en un disco individual que se agrega a CSV. En Windows Server 2012, la configuración predeterminada es 0 (deshabilitada). Para habilitar la memoria caché de CSV en un disco, configura un valor de 1. De forma predeterminada, en Windows Server 2012 R2, esta configuración está habilitada. |

Puedes supervisar la memoria caché de CSV en el monitor de rendimiento si agregas los contadores en **Caché de volumen CSV de clúster**.

#### <a name="configure-the-csv-cache"></a>Configuración de la memoria caché de CSV

1. Inicie Windows PowerShell como administrador.
2. Para definir una memoria caché de *512* MB reservados en cada nodo, escribe lo siguiente:

    - En Windows Server 2012 R2 y versiones posteriores:

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - Para Windows Server 2012:

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. En Windows Server 2012, para habilitar la memoria caché de CSV en un CSV denominado *cluster Disk 1*, escriba lo siguiente:

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * En Windows Server 2012, solo puede asignar el 20% del total de RAM física a la caché de CSV. En Windows Server 2012 R2 y versiones posteriores, puede asignar hasta el 80%. Como los servidores de archivos de escalabilidad horizontal no suelen tener restricciones de memoria, puedes aumentar significativamente el rendimiento si usas la memoria adicional para la memoria caché de CSV.
> * Para evitar la contención de recursos, debe reiniciar cada nodo del clúster después de modificar la memoria asignada a la memoria caché de CSV. En Windows Server 2012 R2 y versiones posteriores, ya no se requiere un reinicio.
> * Después de habilitar o deshabilitar la caché de CSV en un disco individual, para que la configuración surta efecto, debes dejar el recurso de disco físico sin conexión y volver a conectarlo en línea. (De forma predeterminada, en Windows Server 2012 R2 y versiones posteriores, la memoria caché de CSV está habilitada). 
> * Para obtener más información sobre la caché de CSV que incluye información sobre los contadores de rendimiento, consulte la entrada del blog [Cómo habilitar la memoria caché de CSV](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/).

## <a name="backing-up-csvs"></a>Copia de seguridad de CSV

Hay varios métodos para hacer una copia de seguridad de la información que se almacena en CSV en un clúster de conmutación por error. Puedes usar una aplicación de copia de seguridad de Microsoft o una aplicación que no sea de Microsoft. En general, CSV no impone requisitos de copia de seguridad especiales, aparte de los del almacenamiento en clúster formateado con NTFS o ReFS. Las copias de seguridad de CSV tampoco interrumpen otras operaciones de almacenamiento de CSV.

Debes tener en cuenta los siguientes factores al seleccionar una aplicación de copia de seguridad y una programación de copia de seguridad para CSV:

- La copia de seguridad de nivel de volumen de un volumen CSV se puede ejecutar desde cualquier nodo que se conecte al volumen CSV.
- La aplicación de copia de seguridad puede usar instantáneas de software o de hardware. Si la aplicación de copia de seguridad lo permite, las copias de seguridad pueden usar instantáneas del Servicio de instantáneas de volumen (VSS) coherentes con la aplicación y preparadas para bloqueos.
- Si haces una copia de seguridad de un CSV con varias máquinas virtuales en ejecución, por lo general deberías elegir un método de copia de seguridad basado en un sistema operativo de administración. Si la aplicación de copia de seguridad lo admite, se puede hacer copia de seguridad de varias máquinas virtuales simultáneamente.
- CSV admite los solicitantes de copia de seguridad que ejecutan copia de seguridad de Windows Server 2012 R2, copia de seguridad de Windows Server 2012 o copia de seguridad de Windows Server 2008 R2. No obstante, por lo general, Copias de seguridad de Windows Server solo proporciona una solución de copia de seguridad básica que es posible que no sea apta para organizaciones con clústeres grandes. Copias de seguridad de Windows Server no admite la copia de seguridad de máquinas virtuales coherente con la aplicación en CSV. Solo admite copias de seguridad de nivel de volumen preparadas para bloqueos. (Si restaura una copia de seguridad coherente con el bloqueo, la máquina virtual estará en el mismo estado que tenía si la máquina virtual se hubiera bloqueado en el momento exacto en que se realizó la copia de seguridad). Una copia de seguridad de una máquina virtual en un volumen CSV se realizará correctamente, pero se registrará un evento de error que indica que no se admite.
- Es posible que necesites credenciales administrativas al hacer una copia de seguridad de un clúster de conmutación por error.

> [!IMPORTANT]
>Asegúrate de revisar detenidamente los datos que la aplicación de copia de seguridad incluye en la copia de seguridad y restaura, qué características de CSV admite y cuáles son los requisitos de recursos para la aplicación en cada nodo del clúster.

> [!WARNING]
> Si necesitas restaurar los datos de la copia de seguridad a un volumen CSV, ten en cuenta las capacidades y limitaciones de la aplicación de copia de seguridad para mantener y restaurar datos coherentes con la aplicación en los nodos del clúster. Por ejemplo, con algunas aplicaciones, si el volumen CSV se restaura en un nodo distinto del nodo del que se obtuvo su copia de seguridad, es posible que sobrescribas sin darte cuenta datos importantes sobre el estado de la aplicación en el nodo en el que se realiza la restauración.

## <a name="more-information"></a>Más información

- [Clúster de conmutación por error](failover-clustering.md)
- [Implementar espacios de almacenamiento en clúster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)