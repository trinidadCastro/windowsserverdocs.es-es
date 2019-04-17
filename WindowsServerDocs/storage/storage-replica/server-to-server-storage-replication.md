---
title: "Replicación de almacenamiento de servidor a servidor"
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 10/11/2016
ms.assetid: 61881b52-ee6a-4c8e-85d3-702ab8a2bd8c
ms.openlocfilehash: 9dd2820f641e23dceb5c2ac7110efda0d3345dcf
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="server-to-server-storage-replication"></a>Replicación de almacenamiento de servidor a servidor

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar la Réplica de almacenamiento para configurar dos servidores para sincronizar datos, de forma que cada uno tenga una copia idéntica del mismo volumen. Este tema proporciona cierta información general de esta configuración de replicación de servidor a servidor, además de cómo configurar y administrar el entorno.

Para administrar la Réplica de almacenamiento puede usar PowerShell o las [herramientas de administración de servidor de Azure](https://blogs.technet.microsoft.com/servermanagement/).

> [!IMPORTANT]
>  En este escenario, cada servidor debe estar en otro sitio físico o lógico. Cada servidor debe ser capaz de comunicarse con otro a través de una red.  

## <a name="terms"></a>Términos  
En este tutorial se utiliza como ejemplo el siguiente entorno:  

-   Dos servidores, llamados **SR-SRV05** y **SR SRV06**.  

-   Un par de "sitios" lógicos que representan dos centros de datos diferentes, uno llamado **Redmond** y otro llamado **Bellevue**.  

![Diagrama que muestra un servidor en la replicación de la compilación 5 con un servidor de la compilación 9](media/Server-to-Server-Storage-Replication/Storage_SR_ServertoServer.png)  

**Ilustración 1: Replicación de servidor a servidor**  

## <a name="prerequisites"></a>Requisitos previos  

* Bosque de Active Directory Domain Services (no es necesario ejecutar Windows Server 2016).  
* Dos servidores que tengan instalado Windows Server 2016 Datacenter Edition.  
* Dos conjuntos de almacenamiento, mediante JBOD de SAS, SAN de canal de fibra, destino iSCSI o almacenamiento SCSI/SATA local. El almacenamiento debe contener una combinación de medios de disco duro (HDD) y unidades de estado sólido (SSD). Hará que cada conjunto de almacenamiento esté disponible solo para cada uno de los servidores, sin acceso compartido.  
* Cada conjunto de almacenamiento debe permitir la creación de al menos dos discos virtuales, uno para datos replicados y otro para registros. El almacenamiento físico debe tener los mismos tamaños de sector en todos los discos de datos. El almacenamiento físico debe tener los mismos tamaños de sector en todos los discos de registro.  
* Al menos una conexión de Ethernet/TCP en cada servidor para replicación sincrónica, pero preferiblemente RDMA.   
* Reglas de firewall y enrutador correspondientes para permitir tráfico bidireccional ICMP, SMB (puerto 445, más 5445 para SMB directo) y WS-MAN (puerto 5985) entre todos los nodos.  
* Una red entre servidores con ancho de banda suficiente para contener la carga de trabajo de escritura de E/S y un promedio de =5 ms de latencia de ida y vuelta para la replicación sincrónica. La replicación asincrónica no tiene una recomendación de latencia.  
* El almacenamiento de información replicada no se encuentra en la unidad que contiene la carpeta del sistema operativo Windows.

Muchos de estos requisitos se pueden determinar mediante el cmdlet `Test-SRTopology cmdlet`. Puede obtener acceso a esta herramienta si instala las características Réplica de almacenamiento o Herramientas de administración de réplica de almacenamiento en al menos un servidor. No es necesario configurar Réplica de almacenamiento para utilizar esta herramienta, solo para instalar el cmdlet. Se incluye más información en los pasos siguientes.  

## <a name="provision-operating-system-features-roles-storage-and-network"></a>Aprovisionamiento de sistema operativo, características, roles, almacenamiento y red  
1.  Instala Windows Server 2016 en los dos nodos de servidor con un tipo de instalación de Windows Server 2016 Datacenter **(Experiencia de escritorio)**. No elijas la edición Standard si está disponible, ya que no contiene Réplica de almacenamiento.  

2.  Agregue información de red y una los nodos al dominio; a continuación, reinícielos.  

    > [!NOTE]
    > A partir de este punto, inicie sesión siempre como un usuario de dominio que sea miembro del grupo de administradores integrado en todos los servidores. Recuerde siempre elevar sus solicitudes de CMD y PowerShell en el futuro cuando se ejecute en una instalación de servidor gráfico o en un equipo de Windows 10.  

3.  Conecte el primer conjunto de contenedores de almacenamiento JBOD, el destino de iSCSI, la SAN de FC o el almacenamiento de disco local fijo (DAS) al servidor del sitio **Redmond**.  

4.  Conecte el segundo conjunto de almacenamiento al servidor del sitio **Bellevue**.  

5.  Según corresponda, instale el firmware y los controladores más recientes de almacenamiento y alojamiento de proveedores, los controladores HBA más recientes de los proveedores, el firmware UEFI y BIOS más reciente de los proveedores, los controladores de red más recientes de los proveedores y los controladores más recientes de conjunto de chips de placa base en ambos nodos. Reinicie los nodos según sea necesario.  

    > [!NOTE]
    > Consulte la documentación del proveedor de hardware para configurar el almacenamiento compartido y el hardware de red.  

6.  Asegúrese de que la configuración de BIOS o UEFI para los servidores permite un alto rendimiento; por ejemplo, deshabilite el estado C, establezca la velocidad de QPI, habilite NUMA y configure la frecuencia de la memoria en el valor más elevado. Asegúrese de que la administración de energía en Windows Server se establezca en alto rendimiento. Reinicie si es necesario.  

7.  Configure los roles de la manera siguiente:  

    -   **Método gráfico**  

        1.  Ejecute **ServerManager.exe** y cree un grupo de servidores, agregando todos los nodos de servidor.  

        2.  Instale los roles y las características de **Servidor de archivos** y **Réplica de almacenamiento** en cada uno de los nodos y reinícielos.  

    -   **Método de Windows PowerShell**  

        En SR-SRV06 o un equipo de administración remota, ejecute el siguiente comando en una consola de Windows PowerShell para instalar las características y los roles necesarios y reinícielos:  

        ```  
        $Servers = 'SR-SRV05','SR-SRV06'  

        $Servers | ForEach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,FS-FileServer -IncludeManagementTools -restart }  
        ```  

        Para más información sobre estos pasos, consulte [Instalación o desinstalación de roles, servicios de rol o características](http://technet.microsoft.co/library/hh831809.aspx).  

8.  Configure el almacenamiento como sigue:  

    > [!IMPORTANT]  
    > -   Debe crear dos volúmenes en cada contenedor: uno para datos y otro para registros.  
    > -   Los discos de datos y de registros deben inicializarse como GPT, no MBR.  
    > -   Los dos volúmenes de datos deben tener un tamaño idéntico.  
    > -   Los dos volúmenes de registros deben tener un tamaño idéntico.  
    > -   Todos los discos de datos replicados deben tener los mismos tamaños de sector.  
    > -   Todos los discos de registro deben tener los mismos tamaños de sector.  
    > -   Los volúmenes de registro deben usar almacenamiento basado en flash, como SSD. Microsoft recomienda que el almacenamiento de registro sea más rápido que el almacenamiento de datos. Nunca se debe utilizar volúmenes de registro para otras cargas de trabajo.
    > -   Los discos de datos pueden usar HDD, SSD o una combinación en niveles, y pueden usar tanto espacios de paridad o reflejados como RAID 1 o 10, o RAID 5 o RAID 50.  
    > -   El volumen del registro debe ser al menos de 9 GB de forma predeterminada, aunque puede ser mayor o menor en función de los requisitos de registro.  
    > -   El rol Servidor de archivos solo es necesario para el funcionamiento de Test-SRTopology, ya que abre los puertos de firewall necesarios para las pruebas.
    
    - **Para contenedores JBOD:**  

        1.  Asegúrese de que cada servidor pueda ver solo contenedores de almacenamiento de ese sitio y que las conexiones de SAS estén configuradas correctamente.  

        2.  Aprovisione el almacenamiento con Espacios de almacenamiento siguiendo **los pasos 1 a 3** especificados en [Implementar Espacios de almacenamiento en un servidor independiente](http://technet.microsoft.com/library/jj822938.aspx) por medio de Windows PowerShell o el Administrador del servidor.  

    - **Para el almacenamiento iSCSI:**  

        1.  Asegúrese de que cada clúster pueda ver solo los contenedores de almacenamiento de ese sitio. Debe usar más de un único adaptador de red si utiliza iSCSI.    

        2.  Aprovisione el almacenamiento mediante la documentación del proveedor. Si usa destinos iSCSI basados en Windows, consulte [Procedimientos de almacenamiento de bloque de destino iSCSI](http://technet.microsoft.com/library/hh848268.aspx).  

    - **Para el almacenamiento SAN de FC:**  

        1.  Asegúrese de que cada clúster pueda ver solo los contenedores de almacenamiento de ese sitio y que esté en la zona correcta en los hosts.   

        2.  Aprovisione el almacenamiento mediante la documentación del proveedor.  

    - **Para el almacenamiento de disco fijo local (DAS):**  

        -   Asegúrese de que el almacenamiento no contenga un volumen del sistema, un archivo de paginación o archivos de volcado.  

        -   Aprovisione el almacenamiento mediante la documentación del proveedor.  

9. Inicie Windows PowerShell y use el cmdlet **Test-SRTopology** para determinar si satisface todos los requisitos de la Réplica de almacenamiento. Puede usar el cmdlet en modo de solo requisitos para una prueba rápida, o en modo de evaluación de rendimiento de ejecución más larga.  

    Por ejemplo, para validar los nodos propuestos que tiene cada uno un volumen **F:** y **G:** y ejecutar la prueba durante 30 minutos:  

        MD c:\temp  

        Test-SRTopology -SourceComputerName SR-SRV05 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV06 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp  

    > [!IMPORTANT]
      > Al usar un servidor de prueba sin carga de E/S de escritura en el volumen de origen especificado durante el período de evaluación, considere la posibilidad de agregar una carga de trabajo o no se generará un informe útil. Pruebe con cargas de trabajo del estilo de producción para ver números reales y tamaños de registro recomendados. Otra alternativa es copiar algunos archivos en el volumen de origen durante la prueba o descargar y ejecutar [DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) para generar E/S de escritura. Por ejemplo, una muestra con una carga de trabajo de E/S de escritura baja durante diez minutos en el volumen D:  
      >
      > `Diskspd.exe -c1g -d600 -W5 -C5 -b8k -t2 -o2 -r -w5 -i100 d:\test` 

10. Examine el informe **TestSrTopologyReport.html** para asegurarse de que cumple los requisitos de la Réplica de almacenamiento.  

    ![Pantalla que muestra el informe de topología](media/Server-to-Server-Storage-Replication/SRTestSRTopologyReport.png)  

## <a name="configure-server-to-server-replication-using-windows-powershell"></a>Configuración de la replicación de servidor a servidor mediante Windows PowerShell  
Ahora configurará la replicación de servidor a servidor mediante Windows PowerShell. Debe realizar todos los pasos siguientes en los nodos directamente o desde un equipo de administración remota que contenga las herramientas de administración de RSAT de Windows Server 2016.  

La administración gráfica de Réplica de almacenamiento es compatible con la herramienta de Administrador del servidor (SMT) gratuita. Examine la documentación de Azure para conocer la disponibilidad de la versión preliminar de este conjunto de herramientas de administración. Este documento se actualizará después de que SMT alcance la disponibilidad general.

1. Asegúrese de que está utilizando una consola de Powershell con privilegios elevados como administrador.  
2. Configure la replicación de servidor a servidor y especifique los discos de origen y destino, los registros de origen y destino, los nodos de origen y de destino y el tamaño del registro.  

    ```PowerShell  
    New-SRPartnership -SourceComputerName sr-srv05 -SourceRGName rg01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName sr-srv06 -DestinationRGName rg02 -DestinationVolumeName f: -DestinationLogVolumeName g:  
    ```  

   Output:
   ```PowerShell
   DestinationComputerName : SR-SRV06
   DestinationRGName       : rg02
   SourceComputerName      : SR-SRV05
   PSComputerName          :
   ```

    > [!IMPORTANT]
    > El tamaño de registro predeterminado es 8 GB. Según los resultados del cmdlet `Test-SRTopology`, puede decidir usar - LogSizeInBytes con un valor superior o inferior.  

2.  Para obtener el estado de origen y destino de replicación, use `Get-SRGroup` y `Get-SRPartnership` de la manera siguiente:  

    ```PowerShell  
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```
    Output:

    ```PowerShell
    CurrentLsn             : 0
    DataVolume             : F:\
    LastInSyncTime         :
    LastKnownPrimaryLsn    : 1
    LastOutOfSyncTime      :
    NumOfBytesRecovered    : 37731958784
    NumOfBytesRemaining    : 30851203072
    PartitionId            : c3999f10-dbc9-4a8e-8f9c-dd2ee6ef3e9f
    PartitionSize          : 68583161856
    ReplicationMode        : synchronous
    ReplicationStatus      : InitialBlockCopy
    PSComputerName         :
    ```

3.  Determine el progreso de la replicación del modo siguiente:  

    1.  En el servidor de origen, ejecute el siguiente comando y examine los eventos 5015, 5002, 5004, 1237, 5001 y 2200:  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20  
        ```  

    2.  En el servidor de destino, ejecute el siguiente comando para ver los eventos de Réplica de almacenamiento que muestran la creación de la asociación. Este evento indica el número de bytes copiados y el tiempo necesario. Ejemplo:  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl  

        TimeCreated  : 4/8/2016 4:12:37 PM  
        ProviderName : Microsoft-Windows-StorageReplica  
        Id           : 1215  
        Message      : Block copy completed for replica.  

                ReplicationGroupName: rg02  
                ReplicationGroupId: {616F1E00-5A68-4447-830F-B0B0EFBD359C}  
                ReplicaName: f:\  
                ReplicaId: {00000000-0000-0000-0000-000000000000}  
                End LSN in bitmap:   
                LogGeneration: {00000000-0000-0000-0000-000000000000}  
                LogFileId: 0  
                CLSFLsn: 0xFFFFFFFF  
                Number of Bytes Recovered: 68583161856  
                Elapsed Time (ms): 117  
        ```  

        > [!NOTE]
        > Réplica de almacenamiento desmonta los volúmenes de destino y sus puntos de montaje o letras de unidad. Esto es así por diseño.  

    3.  Como alternativa, el grupo de servidores de destino de la réplica indica el número de bytes que quedan por copiar en todo momento y se puede consultar mediante PowerShell. Por ejemplo:  

        ```  
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining  
        ```  

        Como ejemplo de progreso (que no terminará):  

        ```  
        while($true) {  

         $v = (Get-SRGroup -Name "RG02").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }  
        ```  

    4.  En el servidor de destino, ejecute el siguiente comando y examine los eventos 5009, 1237, 5001, 5015, 5005 y 2200 para entender el progreso del procesamiento. No debería haber ninguna advertencia de errores en esta secuencia. Habrá muchos eventos 1237; estos indican el progreso.  

        ```  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
        ```  

## <a name="manage-replication"></a>Administrar la replicación  
Ahora administrará y usará su infraestructura replicada de servidor a servidor. Puede realizar todos los pasos siguientes en los nodos directamente o desde un equipo de administración remota que contenga las herramientas de administración de RSAT de Windows Server 2016.  

1.  Use `Get-SRPartnership` y `Get-SRGroup` para determinar el origen y el destino actuales de la replicación y su estado.  

2.  Para medir el rendimiento de la replicación, utilice el cmdlet `Get-Counter` en los nodos de origen y de destino. Los nombres de contador son:  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de veces que se pausó el vaciado  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de E/S de vaciado pendientes  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de solicitudes para la última escritura del registro  

    -   \Estadísticas de E/S de partición de réplica de almacenamiento (*)\Longitud media de la cola de vaciado  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Longitud actual de la cola de vaciado  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de solicitudes de escritura en aplicación  

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número promedio de solicitudes por escritura del registro  

    -   \Estadísticas de E/S de partición de réplica de almacenamiento (*)\Latencia media de escritura de la aplicación  

    -   \Estadísticas de E/S de partición de réplica de almacenamiento (*)\Latencia media de lectura de la aplicación  

    -   \Estadísticas de Réplica de almacenamiento(*)\RPO de destino  

    -   \Estadísticas de Réplica de almacenamiento(*)\RPO actual  

    -   \Estadísticas de Réplica de almacenamiento(*)\Longitud media de cola del registro  

    -   \Estadísticas de Réplica de almacenamiento(*)\Longitud de cola del registro actual  

    -   \Estadísticas de Réplica de almacenamiento(*)\Nº total de bytes recibidos  

    -   \Estadísticas de Réplica de almacenamiento(*)\Nº total de bytes enviados  

    -   \Estadísticas de réplica de almacenamiento (*)\Latencia media de envío de red  

    -   \Estadísticas de Réplica de almacenamiento(*)\Estado de la replicación  

    -   \Estadísticas de réplica de almacenamiento(*)\Latencia medida del recorrido de ida y vuelta de mensaje  

    -   \Estadísticas de Réplica de almacenamiento(*)\Tiempo transcurrido de la última recuperación  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de recuperación vaciadas  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de recuperación  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de replicación vaciadas  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de replicación  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número máximo de secuencia de registro  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de mensajes recibidos  

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de mensajes enviados  

    Para más información sobre los contadores de rendimiento en Windows PowerShell, consulte [Get-Counter](http://technet.microsoft.com/library/hh849685.aspx).  

3.  Para mover la dirección de replicación de un sitio, use el cmdlet `Set-SRPartnership`.  

    ```  
    Set-SRPartnership -NewSourceComputerName sr-srv06 -SourceRGName rg02 -DestinationComputerName sr-srv05 -DestinationRGName rg01  
    ```  

    > [!WARNING]  
    > Windows Server 2016 impide el cambio de rol cuando la sincronización inicial está en curso, lo que puede dar lugar a pérdida de datos si intenta cambiar antes de dejar que finalice la replicación inicial. No fuerce el cambio de dirección hasta que la sincronización inicial se haya completado.  

    Compruebe los registros de eventos para ver la dirección en que se producen el cambio de replicación y el modo de recuperación y luego concílielos. Las E/S de escritura se pueden escribir luego en el almacenamiento propiedad del nuevo servidor de origen. Al cambiar la dirección de replicación se bloquearán las E/S de escritura en el equipo de origen anterior.  

4.  Para quitar la replicación, use `Get-SRGroup`, `Get-SRPartnership`, `Remove-SRGroup` y `Remove-SRPartnership` en cada nodo. Asegúrese de ejecutar el cmdlet `Remove-SRPartnership` únicamente en el origen actual de replicación, no en el servidor de destino. Ejecute `Remove-Group` en ambos servidores. Por ejemplo, para quitar toda la replicación de dos servidores:  

    ```  
    Get-SRPartnership  
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

## <a name="replacing-dfs-replication-with-storage-replica"></a>Reemplazo de la Replicación DFS por la Réplica de almacenamiento  
Muchos clientes de Microsoft implementan la Replicación DFS como solución de recuperación ante desastres para los datos de usuario no estructurados como las carpetas particulares y los recursos compartidos de departamentos. Replicación DFS se incluye en Windows Server 2003 R2 y en todos los sistemas operativos posteriores y funciona en redes de ancho de banda bajo, lo que resulta atractivo para entornos de alta latencia y bajo cambio con muchos nodos. Sin embargo, la Replicación DFS tiene limitaciones importantes como una solución de replicación de datos:  
* No se replica en archivos abiertos o en uso.  
* No se replica sincrónicamente.  
* La latencia de replicación asincrónica puede ser de muchos minutos, horas o incluso días.  
* Depende de una base de datos que puede requerir comprobaciones de coherencia tediosas tras una interrupción de la alimentación.  
* Por lo general se configura como varios maestros, lo que permite que cambios fluyan en ambas direcciones, sobrescribiendo posiblemente los datos más recientes.  

La Réplica de almacenamiento no tiene ninguna de estas limitaciones. Sin embargo, tienen varias que podrían hacerla menos interesante en algunos entornos:  

* Solo permite la replicación uno a uno entre volúmenes. Es posible replicar volúmenes distintos entre varios servidores.  
* Aunque admite la replicación asincrónica, no está diseñada para redes con poco ancho de banda y alta latencia.  
* No permite el acceso de usuario a los datos protegidos en el destino mientras la replicación está en curso.  

Si no se trata de factores de bloqueo, la Réplica de almacenamiento permite reemplazar servidores de Replicación DFS por esta tecnología más reciente.   
Se trata de un proceso de alto nivel:  

1.  Instale Windows Server 2016 en dos servidores y configure el almacenamiento. Para ello podría ser necesario actualizar un conjunto existente de servidores o realizar una instalación limpia.  
2.  Asegúrese de que los datos que quiera replicar existan en uno o varios volúmenes de datos y no en la unidad C:.   
a.  También puede inicializar los datos en el otro servidor para ahorrar tiempo, mediante una copia de seguridad o copias de archivos, así como usar almacenamiento con aprovisionamiento fino. No es necesario que la seguridad de metadatos coincida exactamente, a diferencia de la Replicación DFS.  
3.  Comparta los datos en el servidor de origen y permita que sean accesibles a través de un espacio de nombres DFS. Esto es importante para asegurarse de que los usuarios pueden seguir teniendo acceso a él si el nombre del servidor cambia por uno de un sitio de desastres.  
a.  Puede crear recursos compartidos de coincidencias en el servidor de destino no estará disponible durante las operaciones normales,   
b.  No agregue el servidor de destino al espacio de nombres de Espacios de nombres DFS, o si lo hace, asegúrese de que todos sus destinos de carpeta estén deshabilitados.  
4.  Habilita la replicación de réplica de almacenamiento y la sincronización inicial completa. La replicación puede ser sincrónica o asincrónica.   
a.  Sin embargo, se recomienda sincrónica con el fin de garantizar la coherencia de datos de E/S en el servidor de destino.   
b.  Se recomienda firmemente habilitar las instantáneas de volumen y tomar periódicamente instantáneas con VSSADMIN u otras herramientas de su elección. De esta forma se garantiza que las aplicaciones vacían sus archivos de datos en el disco de forma coherente. En caso de desastres, puede recuperar archivos de instantáneas en el servidor de destino que se podrían haber replicado parcialmente de manera asincrónica. Las instantáneas se replican junto con los archivos.  
5.  Funcionan normalmente hasta que hay un desastre.  
6.  Cambie el servidor de destino para que sea el nuevo origen, lo que expone sus volúmenes replicados a los usuarios.  
7.  Si usa replicación sincrónica, no será necesaria ninguna restauración de datos a menos que el usuario estuviera usando una aplicación que estaba escribiendo datos sin protección de transacciones (esto es independiente de la replicación) durante la pérdida del servidor de origen. Si usa replicación asincrónica, la necesidad de un montaje de instantáneas de VSS es mayor, pero considere el uso de VSS en todas las circunstancias para instantáneas coherentes de aplicación.  
8.  Agregue el servidor y sus recursos compartidos como destino de carpeta de espacio de nombres DFS.   
9.  Así los usuarios podrán acceder a sus datos.  

 > [!NOTE]
 > El planeamiento de la recuperación ante desastres es un tema complejo que requiere prestar una gran atención al detalle. Es muy recomendable la creación de Runbooks y la ejecución de exploraciones anuales en vivo de conmutación por error. Cuando se produzca un desastre real, reinará la confusión y puede que no esté disponible personal con experiencia.  

## <a name="related-topics"></a>Temas relacionados  
- [Información general sobre Réplica de almacenamiento](storage-replica-overview.md)  
- [Replicación de clúster extendido con almacenamiento compartido](stretch-cluster-replication-using-shared-storage.md)  
- [Replicación de almacenamiento de clúster a clúster](cluster-to-cluster-storage-replication.md)
- [Réplica de almacenamiento: problemas conocidos](storage-replica-known-issues.md)  
- [Réplica de almacenamiento: preguntas frecuentes](storage-replica-frequently-asked-questions.md)  

## <a name="see-also"></a>Consulta también  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [Espacios de almacenamiento directo en Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
