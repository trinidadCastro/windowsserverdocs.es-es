---
title: Replicación de almacenamiento de clúster a clúster
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
ms.assetid: 834e8542-a67a-4ba0-9841-8a57727ef876
author: nedpyle
ms.date: 10/11/2017
description: Cómo usar la opción Réplica de almacenamiento para replicar volúmenes de un clúster a otro clúster con Windows Server 2016 Datacenter Edition.
ms.openlocfilehash: 46bd5a53ff0e704844f10264a9f3a6fbe0e4d512
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838666"
---
# <a name="cluster-to-cluster-storage-replication"></a>Replicación de almacenamiento de clúster a clúster

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

La replicación de clúster a clúster ahora está disponible en Windows Server 2016 Datacenter Edition, incluida la replicación de los clústeres con espacios de almacenamiento directo (es decir, nada compartido, almacenamiento conectado directo). La administración y la configuración es similar a la replicación de servidor a servidor.  

Configurará estos equipos y el almacenamiento en una configuración de clúster a clúster, donde un clúster replica su propio conjunto de almacenamiento con otro clúster y su conjunto de almacenamiento. Estos nodos y su almacenamiento deberían encontrarse en distintos sitios físicos, aunque no es necesario.  

No existen herramientas gráficas en Windows Server 2016 Datacenter Edition, que puedan configurar la réplica de almacenamiento para la replicación de clúster a clúster, aunque Azure Site Recovery será capaz de configurar este escenario en el futuro.

> [!IMPORTANT]
> En esta prueba, los cuatro servidores son un ejemplo. Puede usar cualquier número de servidores compatibles con Microsoft en cada clúster, que actualmente es 8 para un clúster de espacios de almacenamiento directo y 64 para un clúster de almacenamiento compartido.  
>   
> Esta guía no trata la configuración de Espacios de almacenamiento directo. Para obtener información sobre cómo configurar Espacios de almacenamiento directo, vea [Espacios de almacenamiento directo en Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md).  

En este tutorial se utiliza como ejemplo el siguiente entorno:  

-   Dos servidores miembros, denominados **SR-SRV01** y **SR-SRV02**, que más adelante forman un clúster denominado **SR-SRVCLUSA**.  

-   Dos servidores miembros, denominados **SR-SRV03** y **SR-SRV04**, que más adelante forman un clúster denominado **SR-SRVCLUSB**.  

-   Un par de "sitios" lógicos que representan dos centros de datos diferentes, uno llamado **Redmond** y otro llamado **Bellevue**.  

![Diagrama que muestra un entorno de ejemplo con un clúster en el sitio de Redmond que se replica con un clúster en el sitio de Bellevue](./media/Cluster-to-Cluster-Storage-Replication/SR_ClustertoCluster.png)  

**FIGURA 1: Clúster al clúster de replicación**  

## <a name="prerequisites"></a>Requisitos previos  

* Bosque de Active Directory Domain Services (no es necesario ejecutar Windows Server 2016).  
* Al menos cuatro servidores (dos servidores en dos clústeres) que tengan instalado Windows Server 2016 Datacenter Edition. Admite hasta dos clústeres de 64 nodos.  
* Dos conjuntos de almacenamiento, mediante JBOD de SAS, SAN de canal de fibra, VHDX compartido, espacios de almacenamiento directo o destino iSCSI. El almacenamiento debe contener una combinación de medios de disco duro (HDD) y unidades de estado sólido (SSD). Hará que cada conjunto de almacenamiento esté disponible solo para cada uno de los clústeres, sin acceso compartido entre clústeres.  
* Cada conjunto de almacenamiento debe permitir la creación de al menos dos discos virtuales, uno para datos replicados y otro para registros. El almacenamiento físico debe tener los mismos tamaños de sector en todos los discos de datos. El almacenamiento físico debe tener los mismos tamaños de sector en todos los discos de registro.  
* Al menos una conexión de Ethernet/TCP en cada servidor para replicación sincrónica, pero preferiblemente RDMA.   
* Reglas de firewall y enrutador correspondientes para permitir tráfico bidireccional ICMP, SMB (puerto 445, más 5445 para SMB directo) y WS-MAN (puerto 5985) entre todos los nodos.  
* Una red entre servidores con ancho de banda suficiente para contener la carga de trabajo de escritura de E/S y un promedio de =5 ms de latencia de ida y vuelta para la replicación sincrónica. La replicación asincrónica no tiene una recomendación de latencia.  
* El almacenamiento de información replicada no se encuentra en la unidad que contiene la carpeta del sistema operativo Windows.
* Hay consideraciones importantes y limitaciones para la replicación de espacios de almacenamiento directo, revise la información detallada a continuación.

Muchos de estos requisitos se pueden determinar mediante el cmdlet `Test-SRTopology`. Puede obtener acceso a esta herramienta si instala las características Réplica de almacenamiento o Herramientas de administración de réplica de almacenamiento en al menos un servidor. No es necesario configurar Réplica de almacenamiento para utilizar esta herramienta, solo para instalar el cmdlet. Se incluye más información en los pasos siguientes.  

## <a name="step-1-provision-operating-system-features-roles-storage-and-network"></a>Paso 1: Aprovisionamiento de sistema operativo, características, roles, almacenamiento y red

1.  Instala Windows Server 2016 en los cuatro nodos de servidor con un tipo de instalación de Windows Server 2016 Datacenter **(Experiencia de escritorio)**. No elijas la edición Standard si está disponible, ya que no contiene Réplica de almacenamiento.  

2.  Agregue información de red y una los nodos al dominio; a continuación, reinícielos.  

    > [!IMPORTANT]  
    > A partir de este punto, inicie sesión siempre como un usuario de dominio que sea miembro del grupo de administradores integrado en todos los servidores. Recuerde siempre elevar sus solicitudes de CMD y Windows PowerShell en el futuro cuando se ejecute en una instalación de servidor gráfico o en un equipo de Windows 10.  

3.  Conecte el primer conjunto de contenedores de almacenamiento JBOD, el destino de iSCSI, la SAN de FC o el almacenamiento de disco local fijo (DAS) al servidor del sitio **Redmond**.  

4.  Conecte el segundo conjunto de almacenamiento al servidor del sitio **Bellevue**.  

5.  Según corresponda, instale el firmware y los controladores más recientes de almacenamiento y alojamiento de proveedores, los controladores HBA más recientes de los proveedores, el firmware UEFI y BIOS más reciente de los proveedores, los controladores de red más recientes de los proveedores y los controladores más recientes de conjunto de chips de placa base en los cuatro nodos. Reinicie los nodos según sea necesario.  

    > [!NOTE]  
    > Consulte la documentación del proveedor de hardware para configurar el almacenamiento compartido y el hardware de red.  

6.  Asegúrese de que la configuración de BIOS o UEFI para los servidores permite un alto rendimiento; por ejemplo, deshabilite el estado C, establezca la velocidad de QPI, habilite NUMA y configure la frecuencia de la memoria en el valor más elevado. Asegúrese de que la administración de energía en Windows Server se establezca en alto rendimiento. Reinicie si es necesario.  

7.  Configure los roles de la manera siguiente:  

    -   **Método gráfico**  

        1.  Ejecute **ServerManager.exe** y cree un grupo de servidores, agregando todos los nodos de servidor.  

        2.  Instale los roles y las características de **Servidor de archivos** y **Réplica de almacenamiento** en cada uno de los nodos y reinícielos.  

    -   **Método de Windows PowerShell**  

        En SR-SRV04 o un equipo de administración remota, ejecute el siguiente comando en una consola de Windows PowerShell para instalar las características y los roles necesarios para un clúster extendido en los cuatro nodos de clúster y reinícielos:  

        ```PowerShell
        $Servers = 'SR-SRV01','SR-SRV02','SR-SRV03','SR-SRV04'  

        $Servers | ForEach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,Failover-Clustering,FS-FileServer -IncludeManagementTools -restart }  
        ```  

        Para más información sobre estos pasos, consulte [Instalación o desinstalación de roles, servicios de rol o características](https://technet.microsoft.com/library/hh831809.aspx).  

9. Configure el almacenamiento como sigue:  

    > [!IMPORTANT]  
    > -   Debe crear dos volúmenes en cada contenedor: uno para datos y otro para registros.  
    > -   Los discos de datos y de registros deben inicializarse como **GPT**, no **MBR**.  
    > -   Los dos volúmenes de datos deben tener un tamaño idéntico.  
    > -   Los dos volúmenes de registros deben tener un tamaño idéntico.  
    > -   Todos los discos de datos replicados deben tener los mismos tamaños de sector.  
    > -   Todos los discos de registro deben tener los mismos tamaños de sector.  
    > -   Los volúmenes de registro deben usar almacenamiento basado en flash, como SSD.  Microsoft recomienda que el almacenamiento de registro sea más rápido que el almacenamiento de datos. Nunca se debe utilizar volúmenes de registro para otras cargas de trabajo.
    > -   Los discos de datos pueden usar HDD, SSD o una combinación en niveles, y pueden usar tanto espacios de paridad o reflejados como RAID 1 o 10, o RAID 5 o RAID 50.  
    > -   El volumen del registro debe tener al menos 8GB de forma predeterminada y puede ser mayor o menor en función de los requisitos de registro.
    > -   Al utilizar espacios de almacenamiento directo (S2D) con una caché NVME o SSD, verá una mayor que el aumento previsto de latencia al configurar la replicación de réplica de almacenamiento entre clústeres de S2D. El cambio en la latencia es proporcionalmente mucho mayor de ver cuando se usa NVME y SSD en un rendimiento + la configuración de capacidad y ninguna capa HDD ni el nivel de capacidad.

Este problema se produce debido a limitaciones de arquitectura de mecanismo de registro del SR combinada con la latencia extremadamente baja de NVME en comparación con los medios más lentos. Cuando se usa la caché de S2D, todos los registros de E/S de SR, junto con todos los recientes lectura/escritura de aplicaciones, se producen en la memoria caché y nunca en los niveles de rendimiento o capacidad. Esto significa que toda la actividad SR ocurre en el mismo medio de velocidad: no se admite esta configuración no se recomienda (consulte https://aka.ms/srfaq para obtener recomendaciones de registro). 

Al usar S2D con unidades de disco duro, no se puede deshabilitar o evitar la memoria caché. Como alternativa, si usa solo SSD y NVME, puede configurar solo a los niveles de capacidad y rendimiento. Si usa dicha configuración y mediante la colocación de los registros de SR en el nivel de rendimiento solo con los volúmenes de datos de servicio que está en el nivel de capacidad solo, evitará el problema de latencia elevada se ha descrito anteriormente. Lo mismo se podría hacer con una combinación de SSDs más rápidos y más lentos y no NVME.

Esta solución obviamente no es ideal y algunos clientes no podrá hacer uso de ella. El equipo SR está trabajando en el mecanismo de registro actualizada para el futuro reducir estos cuellos de botella artificiales que se producen y optimizaciones. No hay ningún tiempo estimado para esto, pero cuando esté disponible para puntear en los clientes para las pruebas, estas preguntas más frecuentes se actualizará. 

    -   **Para contenedores JBOD:**  

        1.  Asegúrese de que cada clúster pueda ver solo contenedores de almacenamiento de ese sitio y que las conexiones de SAS estén configuradas correctamente.  

        2.  Aprovisione el almacenamiento con Espacios de almacenamiento siguiendo **los pasos 1 a 3** especificados en [Implementar Espacios de almacenamiento en un servidor independiente](https://technet.microsoft.com/library/jj822938.aspx) por medio de Windows PowerShell o el Administrador del servidor.  

    -   **Para el almacenamiento de destino iSCSI:**  

        1.  Asegúrese de que cada clúster pueda ver solo los contenedores de almacenamiento de ese sitio. Debe usar más de un único adaptador de red si utiliza iSCSI.  

        2.  Aprovisiona el almacenamiento mediante la documentación del proveedor. Si usa destinos iSCSI basados en Windows, consulte [Procedimientos de almacenamiento de bloque de destino iSCSI](https://technet.microsoft.com/library/hh848268.aspx).  

    -   **Para el almacenamiento SAN de FC:**  

        1.  Asegúrese de que cada clúster pueda ver solo los contenedores de almacenamiento de ese sitio y que esté en la zona correcta en los hosts.  

        2.  Aprovisiona el almacenamiento mediante la documentación del proveedor.  

    -   **Espacios de almacenamiento directo:**  

        1.  Asegúrate de que cada clúster pueda ver solo contenedores de almacenamiento de ese sitio implementando espacios de almacenamiento directo. (https://docs.microsoft.com/windows-server/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct) 

        2.  Asegúrate de que los volúmenes de registro SR siempre estarán en el almacenamiento flash más rápido y los volúmenes de datos en el almacenamiento de gran capacidad más lento.

10. Inicie Windows PowerShell y use el cmdlet `Test-SRTopology` para determinar si se cumplen todos los requisitos de Réplica de almacenamiento. Puede usar el cmdlet en modo de solo requisitos para una prueba rápida, o en modo de evaluación de rendimiento de ejecución más larga.  
Por ejemplo,  

   ```PowerShell
   MD c:\temp

   Test-SRTopology -SourceComputerName SR-SRV01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV03 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp        
   ```

      > [!IMPORTANT]
      > Al usar un servidor de prueba sin carga de E/S de escritura en el volumen de origen especificado durante el período de evaluación, considere la posibilidad de agregar una carga de trabajo o no se generará un informe útil. Pruebe con cargas de trabajo del estilo de producción para ver números reales y tamaños de registro recomendados. Otra alternativa es copiar algunos archivos en el volumen de origen durante la prueba o descargar y ejecutar [DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) para generar E/S de escritura. Por ejemplo, una muestra con una carga de trabajo de E/S de escritura baja durante cinco minutos en el volumen D:.  
      > `Diskspd.exe -c1g -d300 -W5 -C5 -b8k -t2 -o2 -r -w5 -h d:\test.dat`  

11. Examine el informe **TestSrTopologyReport.html** para asegurarse de que cumple los requisitos de la Réplica de almacenamiento.  

    ![Pantalla donde se muestran los resultados de informes de topología de replicación](./media/Cluster-to-Cluster-Storage-Replication/SRTestSRTopologyReport.png)      

## <a name="step-2-configure-two-scale-out-file-server-failover-clusters"></a>Paso 2: Configuración de dos clústeres de conmutación por error en Servidor de archivos de escalabilidad horizontal  
Ahora creará dos clústeres de conmutación por error normal. Después de la configuración, la validación y las pruebas, los replicará con Réplica de almacenamiento. Puedes realizar todos los pasos siguientes en los nodos del clúster directamente o desde un equipo de administración remota que contenga las herramientas de administración de RSAT de Windows Server 2016.  

### <a name="graphical-method"></a>Método gráfico  

1.  Ejecute **cluadmin.msc** en un nodo de cada sitio.  

2.  Valida el clúster propuesto y analice los resultados para asegurarse de que puedes continuar. En el ejemplo usado a continuación son **SR-SRVCLUSA** y **SR-SRVCLUSB**.  

3.  Cree los dos clústeres. Asegúrese de que los nombres de clúster contiene 15 caracteres o menos.  

4.  Configure un testigo de recurso compartido de archivos o un testigo en la nube.  

    > [!NOTE]  
    > Windows Server 2016 incluye ahora una opción para un testigo basado en la nube (Azure). Puedes elegir esta opción de cuórum en lugar del testigo de recurso compartido de archivos.  

    > [!WARNING]  
    > Para más información sobre la configuración de cuórum, consulte la sección **Configuración de testigos**, en [Configurar y administrar el cuórum en un clúster de conmutación por error de Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx). Para más información sobre el cmdlet `Set-ClusterQuorum`, consulta [Set-ClusterQuorum](https://technet.microsoft.com/library/hh847275.aspx).  

5.  Agregue un disco del sitio de **Redmond** al CSV de clúster. Para ello, haga clic con el botón derecho en un disco de origen el nodo **Discos** de la sección **Almacenamiento** y, a continuación, haga clic en **Agregar a volúmenes compartidos de clúster**.  

6.  Cree los servidores de archivos de escalabilidad horizontal en clúster en ambos clústeres con las instrucciones del artículo sobre [configuración de un servidor de archivos de escalabilidad horizontal](https://technet.microsoft.com/library/hh831718.aspx).  

### <a name="windows-powershell-method"></a>Método de Windows PowerShell  

1.  Prueba el clúster propuesto y analiza los resultados para asegurarse de que puedes continuar:  

    ```PowerShell
    Test-Cluster SR-SRV01,SR-SRV02  
    Test-Cluster SR-SRV03,SR-SRV04  
    ```  

2.  Cree los clústeres (debe especificar sus propias direcciones IP estáticas para los clústeres). Asegúrese de que cada nombre de clúster contiene 15 caracteres o menos:  

    ```PowerShell
    New-Cluster -Name SR-SRVCLUSA -Node SR-SRV01,SR-SRV02 -StaticAddress <your IP here>  
    New-Cluster -Name SR-SRVCLUSB -Node SR-SRV03,SR-SRV04 -StaticAddress <your IP here>  
    ```  

3.  Configure un testigo de recurso compartido de archivos o un testigo en la nube (Azure) en cada clúster que apunta a un recurso compartido hospedado en el controlador de dominio o algún otro tipo de servidor independiente. Por ejemplo:  

    ```PowerShell  
    Set-ClusterQuorum -FileShareWitness \\someserver\someshare  
    ```  

    > [!NOTE]  
    > Windows Server 2016 incluye ahora una opción para un testigo basado en la nube (Azure). Puedes elegir esta opción de cuórum en lugar del testigo de recurso compartido de archivos.  

    > [!WARNING]  
    > Para más información sobre la configuración de cuórum, consulte la sección **Configuración de testigos**, en [Configurar y administrar el cuórum en un clúster de conmutación por error de Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx). Para más información sobre el cmdlet `Set-ClusterQuorum`, consulta [Set-ClusterQuorum](https://technet.microsoft.com/library/hh847275.aspx).  

4.  Cree los servidores de archivos de escalabilidad horizontal en clúster en ambos clústeres con las instrucciones del artículo sobre [configuración de un servidor de archivos de escalabilidad horizontal](https://technet.microsoft.com/library/hh831718.aspx).  

## <a name="step-3-set-up-cluster-to-cluster-replication-using-windows-powershell"></a>Paso 3: Configurar la replicación de clúster a clúster mediante Windows PowerShell  
Ahora configurarás la replicación de clúster a clúster mediante Windows PowerShell. Puede realizar todos los pasos siguientes en los nodos directamente o desde un equipo de administración remota que contenga las herramientas de administración de RSAT de Windows Server 2016  

1.  Conceda el primer acceso completo de clúster al otro clúster mediante la ejecución del **Grant SRAccess** cmdlet en cualquier nodo en el primer clúster, o de forma remota.  

    ```PowerShell
    Grant-SRAccess -ComputerName SR-SRV01 -Cluster SR-SRVCLUSB  
    ```  

2.  Conceda el segundo clúster acceso completo al otro clúster mediante la ejecución del **Grant SRAccess** cmdlet en cualquier nodo en el segundo clúster, o de forma remota.  

    ```PowerShell
    Grant-SRAccess -ComputerName SR-SRV03 -Cluster SR-SRVCLUSA  
    ```  

3.  Configure la replicación de clúster a clúster y especifique los discos de origen y destino, los registros de origen y destino, los nombres de clúster de origen y de destino y el tamaño del registro. Puede realizar este comando localmente en el servidor o mediante un equipo de administración remota.  

    ```powershell  
    New-SRPartnership -SourceComputerName SR-SRVCLUSA -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\Volume2 -SourceLogVolumeName f: -DestinationComputerName SR-SRVCLUSB -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\Volume2 -DestinationLogVolumeName f:  
    ```  

    > [!WARNING]  
    > El tamaño de registro predeterminado es 8 GB. Según los resultados del cmdlet **Test-SRTopology**, puede decidir usar -**LogSizeInBytes** con un valor superior o inferior.  

4.  Para obtener el estado de origen y destino de replicación, utilice **Get-SRGroup** y **Get-SRPartnership** como sigue:  

    ```powershell
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```  

5.  Determine el progreso de la replicación del modo siguiente:  

    1.  En el servidor de origen, ejecuta el siguiente comando y examina los eventos 5015, 5002, 5004, 1237, 5001 y 2200:
        
        ```PowerShell
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20
        ```
    2.  En el servidor de destino, ejecute el siguiente comando para ver los eventos de Réplica de almacenamiento que muestran la creación de la asociación. Este evento indica el número de bytes copiados y el tiempo necesario. Por ejemplo:  
        
        ```powershell
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | Format-List
        ```
        A continuación presentamos un ejemplo de la salida:
        
        ```
        TimeCreated  : 4/8/2016 4:12:37 PM  
        ProviderName : Microsoft-Windows-StorageReplica  
        Id           : 1215  
        Message      : Block copy completed for replica.  
            ReplicationGroupName: rg02  
            ReplicationGroupId:  
            {616F1E00-5A68-4447-830F-B0B0EFBD359C}  
            ReplicaName: f:\  
            ReplicaId: {00000000-0000-0000-0000-000000000000}  
            End LSN in bitmap:  
            LogGeneration: {00000000-0000-0000-0000-000000000000}  
            LogFileId: 0  
            CLSFLsn: 0xFFFFFFFF  
            Number of Bytes Recovered: 68583161856  
            Elapsed Time (seconds): 117  
        ```
    3. También es posible que el grupo de servidores de destino de la réplica indique el número de bytes que quedan por copiar en todo momento, lo cual se puede consultar mediante PowerShell. Por ejemplo:

       ```PowerShell
       (Get-SRGroup).Replicas | Select-Object numofbytesremaining
       ```

       Como ejemplo de progreso (que no terminará):  

       ```PowerShell
         while($true) {  
         $v = (Get-SRGroup -Name "Replication 2").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`n", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }
        ```

6. En el servidor de destino del clúster de destino, ejecute el siguiente comando y examine los eventos 5009, 1237, 5001, 5015, 5005 y 2200 para entender el progreso del procesamiento. No debería haber ninguna advertencia de errores en esta secuencia. Habrá muchos eventos 1237; estos indican el progreso.  
    
   ```PowerShell
   Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
   ```
   > [!NOTE]  
        > El disco de clúster de destino siempre se mostrará como **En línea (sin acceso)** cuando se replica.  

## <a name="step-4-manage-replication"></a>Paso 4: Administrar la replicación

Ahora podrá administrar y operar la replicación de clúster a clúster. Puedes realizar todos los pasos siguientes en los nodos del clúster directamente o desde un equipo de administración remota que contenga las herramientas de administración de RSAT de Windows Server 2016.  

1.  Use **Get-ClusterGroup** o **Administrador de clústeres de conmutación por error** para determinar el origen y el destino actuales de la replicación y su estado.  

2.  Para medir el rendimiento de la replicación, utilice el cmdlet **Get-Counter** en nodos de origen y de destino. Los nombres de contador son:  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de veces que se pausó el vaciado  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de E/S de vaciado pendientes  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de solicitudes para la última escritura del registro  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Promedio de longitud de cola de vaciado  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Longitud actual de la cola de vaciado  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de solicitudes de escritura en aplicación  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Promedio de Número de solicitudes por escritura en registro  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Promedio de latencia de escritura de aplicación  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Promedio de latencia de lectura de aplicación  

    -   \Estadísticas de Réplica de almacenamiento(*)\RPO de destino  

    -   \Estadísticas de Réplica de almacenamiento(*)\RPO actual  

    -   \Estadísticas de Réplica de almacenamiento(*)\Promedio de longitud de cola de registro  

    -   \Estadísticas de Réplica de almacenamiento(*)\Longitud de cola del registro actual  

    -   \Estadísticas de Réplica de almacenamiento(*)\Nº total de bytes recibidos  

    -   \Estadísticas de Réplica de almacenamiento(*)\Nº total de bytes enviados  

    -   \Estadísticas de Réplica de almacenamiento(*)\Promedio de latencia de envío de red  

    -   \Estadísticas de Réplica de almacenamiento(*)\Estado de la replicación  

    -   \Estadísticas de Réplica de almacenamiento(*)\Promedio de latencia del recorrido de ida y vuelta de mensaje  

    -   \Estadísticas de Réplica de almacenamiento(*)\Tiempo transcurrido de la última recuperación  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de recuperación vaciadas  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de recuperación  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de replicación vaciadas  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de replicación  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número máximo de secuencia de registro  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de mensajes recibidos  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de mensajes enviados  

    Para más información sobre los contadores de rendimiento en Windows PowerShell, consulte [Get-Counter](https://technet.microsoft.com/library/hh849685.aspx).  

3.  Para mover la dirección de replicación de un sitio, use el cmdlet **Set-SRPartnership**.  

    ```PowerShell
    Set-SRPartnership -NewSourceComputerName SR-SRVCLUSB -SourceRGName rg02 -DestinationComputerName SR-SRVCLUSA -DestinationRGName rg01  
    ```  

    > [!NOTE]  
    > Windows Server 2016 impide el cambio de rol cuando la sincronización inicial está en curso, lo que puede dar lugar a pérdida de datos si intenta cambiar antes de dejar que finalice la replicación inicial. No fuerce el cambio de dirección hasta que la sincronización inicial se haya completado.

    Compruebe los registros de eventos para ver la dirección en que se producen el cambio de replicación y el modo de recuperación y luego concílielos. Las E/S de escritura se pueden escribir luego en el almacenamiento propiedad del nuevo servidor de origen. Al cambiar la dirección de replicación se bloquearán las E/S de escritura en el equipo de origen anterior.  

    > [!NOTE]  
    > El disco de clúster de destino siempre se mostrará como **En línea (sin acceso)** cuando se replica.  

4.  Para cambiar el tamaño del registro del valor predeterminado de 8 GB en Windows Server 2016, utilice **Set-SRGroup** en grupos de Réplica de almacenamiento de origen y de destino.  

    > [!IMPORTANT]  
    > El tamaño de registro predeterminado es 8 GB. Según los resultados del cmdlet **Test-SRTopology**, puede decidir usar -LogSizeInBytes con un valor superior o inferior.  

5.  Para quitar la replicación, use **Get-SRGroup**, **Get-SRPartnership**, **Remove-SRGroup** y **Remove-SRPartnership** en cada clúster.  

    ```powershell
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

    > [!NOTE]  
    > La réplica de almacenamiento desmonta los volúmenes de destino. Esto es así por diseño.

## <a name="see-also"></a>Vea también

-   [Información general sobre réplica de almacenamiento](storage-replica-overview.md) 
-   [Replicación de clúster extendido con almacenamiento compartido](stretch-cluster-replication-using-shared-storage.md)  
-   [Replicación de almacenamiento de servidor a servidor](server-to-server-storage-replication.md)  
-   [Réplica de almacenamiento: Problemas conocidos](storage-replica-known-issues.md)  
-   [Réplica de almacenamiento: Preguntas más frecuentes](storage-replica-frequently-asked-questions.md)  
-   [Espacios de almacenamiento directo en Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  