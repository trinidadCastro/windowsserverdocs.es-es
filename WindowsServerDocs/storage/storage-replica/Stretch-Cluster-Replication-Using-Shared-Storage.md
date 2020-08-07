---
title: Replicación de clúster extendido con almacenamiento compartido
manager: eldenc
ms.author: nedpyle
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 6c5b9431-ede3-4438-8cf5-a0091a8633b0
ms.openlocfilehash: fa5246aad79b9441b973cf864233ca8cfe0da7fa
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950549"
---
# <a name="stretch-cluster-replication-using-shared-storage"></a>Replicación de clúster extendido con almacenamiento compartido

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

En este ejemplo de evaluación, configurará estos equipos y su almacenamiento en un único clúster extendido, donde dos nodos comparten un conjunto de almacenamiento y dos nodos comparten otro conjunto de almacenamiento; luego, la replicación conserva ambos conjuntos de almacenamiento reflejados en el clúster para permitir la conmutación por error inmediata. Estos nodos y su almacenamiento deberían encontrarse en distintos sitios físicos, aunque no es necesario. Existen pasos independientes para crear clústeres de Hyper-V y de servidor de archivos como escenarios de ejemplo.

> [!IMPORTANT]
> En esta evaluación, los servidores de distintos sitios deben ser capaces de comunicarse con los otros servidores a través de una red, pero sin tener ninguna conectividad física en el almacenamiento compartido del otro sitio. En este escenario no se usa Espacios de almacenamiento directo.

## <a name="terms"></a>Términos
En este tutorial se utiliza como ejemplo el siguiente entorno:

-   Cuatro servidores, denominados **SR-SRV01**, **SR-SRV02**, **SR-SRV03** y **SR-SRV04**, que forman un clúster único llamado **SR-SRVCLUS**.

-   Un par de "sitios" lógicos que representan dos centros de datos diferentes, uno llamado **Redmond** y el otro **Bellevue.**

> [!NOTE]
> Puede usar solo dos nodos, donde cada nodo se encuentra en cada uno de los sitios. Sin embargo, no podrá realizar conmutación por error de dentro del sitio con solo dos servidores. Puede usar hasta 64 nodos.

![Diagrama que muestra dos nodos en Redmond que se replican con dos nodos del mismo clúster en el sitio de Bellevue.](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_StretchClusterExample.png)

**ILUSTRACIÓN 1: replicación de almacenamiento en un clúster extendido**

## <a name="prerequisites"></a>Requisitos previos
-   Bosque de Active Directory Domain Services (no es necesario ejecutar Windows Server 2016).
-   2-64 servidores que ejecutan Windows Server 2019 o Windows Server 2016, Datacenter Edition. Si está ejecutando Windows Server 2019, en su lugar, puede usar la edición Standard si es correcto replicando un solo volumen de hasta 2 TB de tamaño.
-   Dos conjuntos de almacenamiento compartido, con JBOD de SAS (como con espacios de almacenamiento), Canal de fibra SAN, VHDX compartido o destino iSCSI. El almacenamiento debe contener una combinación de medios de HDD y SSD y debe ser compatible con la reserva persistente. Pondrá cada conjunto de almacenamiento a disposición de solo dos de los servidores (modo asimétrico).
-   Cada conjunto de almacenamiento debe permitir la creación de al menos dos discos virtuales, uno para datos replicados y otro para registros. El almacenamiento físico debe tener los mismos tamaños de sector en todos los discos de datos. El almacenamiento físico debe tener los mismos tamaños de sector en todos los discos de registro.
-   Al menos una conexión de 1 GbE en cada servidor para replicación sincrónica, pero preferiblemente RDMA.
-   Al menos 2GB de RAM y dos núcleos por servidor. Necesitará más memoria y núcleos para más máquinas virtuales.
-   Reglas de firewall y enrutador correspondientes para permitir tráfico bidireccional ICMP, SMB (puerto 445, más 5445 para SMB directo) y WS-MAN (puerto 5985) entre todos los nodos.
-   Una red entre servidores con ancho de banda suficiente para contener la carga de trabajo de escritura de E/S y un promedio de =5 ms de latencia de ida y vuelta para la replicación sincrónica. La replicación asincrónica no tiene una recomendación de latencia.
-   El almacenamiento de información replicada no se encuentra en la unidad que contiene la carpeta del sistema operativo Windows.

Muchos de estos requisitos se pueden determinar mediante el cmdlet `Test-SRTopology`. Puede obtener acceso a esta herramienta si instala las características Réplica de almacenamiento o Herramientas de administración de réplica de almacenamiento en al menos un servidor. No es necesario configurar Réplica de almacenamiento para utilizar esta herramienta, solo para instalar el cmdlet. Se incluye más información en los pasos siguientes.

## <a name="provision-operating-system-features-roles-storage-and-network"></a>Aprovisionamiento de sistema operativo, características, roles, almacenamiento y red

1.  Instale Windows Server en todos los nodos de servidor mediante las opciones de instalación Server Core o Server with Desktop Experience.
    > [!IMPORTANT]
    > A partir de este punto, inicie sesión siempre como un usuario de dominio que sea miembro del grupo de administradores integrado en todos los servidores. Recuerde siempre elevar sus solicitudes de CMD y PowerShell en el futuro cuando se ejecute en una instalación de servidor gráfico o en un equipo de Windows 10.

2.  Agregue información de red y una los nodos en el dominio; a continuación, reinícielos.
    > [!NOTE]
    > A partir de este punto, la guía supone que tiene dos pares de servidores para su uso en un clúster extendido. Una red WAN o LAN separa a los servidores y estos pertenecen a sitios físicos o lógicos. En la guía se considera que **SR-SRV01** y **SR-SRV02** están en el sitio Redmond y **SR-SRV03** y **SRV04 SR** están en el sitio **Bellevue**.

3.  Conecte el primer conjunto de contenedores de almacenamiento JBOD compartido, el VHDX compartido, el destino iSCSI o SAN de FC a los servidores del sitio **Redmond**.

4.  Conecte el segundo conjunto de almacenamiento a los servidores del sitio **Bellevue**.

5.  Según corresponda, instale el firmware y los controladores más recientes de almacenamiento y alojamiento de proveedores, los controladores HBA más recientes de los proveedores, el firmware UEFI y BIOS más reciente de los proveedores, los controladores de red más recientes de los proveedores y los controladores más recientes de conjunto de chips de placa base en los cuatro nodos. Reinicie los nodos según sea necesario.

    > [!NOTE]
    > Consulte la documentación del proveedor de hardware para configurar el almacenamiento compartido y el hardware de red.

6.  Asegúrese de que la configuración de BIOS o UEFI para los servidores permite un alto rendimiento; por ejemplo, deshabilite el estado C, establezca la velocidad de QPI, habilite NUMA y configure la frecuencia de la memoria en el valor más elevado. Asegúrese de que la administración de energía en Windows Server se establezca en alto rendimiento. Reinicie si es necesario.

7.  Configure los roles de la manera siguiente:

    -   **Método gráfico**

        Ejecute **ServerManager.exe** y agregue todos los nodos de servidor haciendo clic en **Administrar** y **Agregar servidores**.

        > [!IMPORTANT]
        > Instale los **Clústeres de conmutación por error** y los roles y características de **Réplica de almacenamiento** en cada uno de los nodos y, luego, reinícielos. Si planea usar otros roles, como Hyper-V, el Servidor de archivos, etc., puede instalarlos ahora también.

    -   **Uso del método de Windows PowerShell**

        En **SR-SRV04** o un equipo de administración remota, ejecute el siguiente comando en una consola de Windows PowerShell para instalar las características y los roles necesarios para un clúster extendido en los cuatro nodos de clúster y reinícielos:

        ```PowerShell
        $Servers = 'SR-SRV01','SR-SRV02','SR-SRV03','SR-SRV04'

        $Servers | foreach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,Failover-Clustering,FS-FileServer -IncludeManagementTools -restart }

        ```

        Para más información sobre estos pasos, consulte [Instalación o desinstalación de roles, servicios de rol o características](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md).


8. Configure el almacenamiento como sigue:

    > [!IMPORTANT]
    > -   Debe crear dos volúmenes en cada contenedor: uno para datos y otro para registros.
    > -   Los discos de datos y de registros deben inicializarse como GPT, no MBR.
    > -   Los dos volúmenes de datos deben tener un tamaño idéntico.
    > -   Los dos volúmenes de registros deben tener un tamaño idéntico.
    > -   Todos los discos de datos replicados deben tener los mismos tamaños de sector.
    > -   Todos los discos de registros deben tener los mismos tamaños de sector.
    > -   Los volúmenes de registros deben usar almacenamiento basado en flash y configuración de resistencia de alto rendimiento. Microsoft recomienda que el almacenamiento de registros sea tan rápido como el almacenamiento de datos. Los volúmenes de registro nunca deben usarse para otras cargas de trabajo.
    > -   Los discos de datos pueden usar HDD, SSD o una combinación en niveles, y pueden usar tanto espacios de paridad o reflejados como RAID 1 o 10, o RAID 5 o RAID 50.
    > -  El volumen del registro debe ser al menos de 9 GB de forma predeterminada, aunque puede ser mayor o menor en función de los requisitos de registro.
    > - Los volúmenes deben tener formato NTFS o ReFS.
    > - El rol Servidor de archivos solo es necesario para el funcionamiento de Test-SRTopology, ya que abre los puertos de firewall necesarios para las pruebas.

    -   **Para contenedores JBOD:**

        1.  Asegúrese de que cada conjunto de nodos de servidor emparejados pueda ver solo contenedores de almacenamiento de ese sitio (es decir, que tenga almacenamiento asimétrico) y que las conexiones de SAS estén configuradas correctamente.

        2.  Aprovisione el almacenamiento con Espacios de almacenamiento siguiendo **los pasos 1 a 3** especificados en [Implementar Espacios de almacenamiento en un servidor independiente](../storage-spaces/deploy-standalone-storage-spaces.md) por medio de Windows PowerShell o el Administrador del servidor.

    -   **Para el almacenamiento iSCSI:**

        1.  Asegúrese de que cada conjunto de nodos de servidor emparejados puedan ver solo contenedores de almacenamiento de ese sitio (es decir, que tengan almacenamiento asimétrico). Debe usar más de un único adaptador de red si utiliza iSCSI.

        2.  Aprovisione el almacenamiento mediante la documentación del proveedor. Si usa destinos iSCSI basados en Windows, consulte [Procedimientos de almacenamiento de bloque de destino iSCSI](../iscsi/iscsi-target-server.md).

    -   **Para el almacenamiento SAN de FC:**

        1.  Asegúrese de que cada conjunto de nodos de servidor emparejados pueda ver solo contenedores de almacenamiento de ese sitio (es decir, que tenga almacenamiento asimétrico) y que ha dividido en zonas correctamente los hosts.

        2.  Aprovisione el almacenamiento mediante la documentación del proveedor.

## <a name="configure-a-hyper-v-failover-cluster-or-a-file-server-for-a-general-use-cluster"></a>Configuración de un clúster de conmutación por error de Hyper-V o un servidor de archivos para un clúster de uso general

Después de configurar los nodos del servidor, el siguiente paso es crear uno de los siguientes tipos de clústeres:
*  [Clúster de conmutación por error de Hyper-V](#BKMK_HyperV)
*  [Servidor de archivos para clúster de uso general](#BKMK_FileServer)

### <a name="configure-a-hyper-v-failover-cluster"></a><a name="BKMK_HyperV"></a>Configurar un clúster de conmutación por error de Hyper-V

>[!NOTE]
> Omita esta sección y vaya a [Configuración de un servidor de archivos para clúster de uso general](#BKMK_FileServer) si desea crear un clúster de servidor de archivos y no un clúster de Hyper-V.

Ahora creará un clúster de conmutación por error normal. Después de la configuración, la validación y las pruebas, lo extenderá mediante Réplica de almacenamiento. Puede realizar todos los pasos siguientes en los nodos del clúster directamente o desde un equipo de administración remota que contenga el Herramientas de administración remota del servidor de Windows Server.

#### <a name="graphical-method"></a>Método gráfico

1. Ejecute **cluadmin.msc**.

2. Valide el clúster propuesto y analice los resultados para asegurarse de que puede continuar.

   > [!NOTE]
   > Es de esperar que aparezcan errores de almacenamiento en la validación del clúster, debido al uso del almacenamiento asimétrico.

3. Cree el clúster de proceso de Hyper-V. Asegúrese de que el nombre del clúster contiene 15 caracteres o menos. En el ejemplo a continuación se usa SR-SRVCLUS. Si los nodos van a residir en subredes diferentes, debe crear una dirección IP para el nombre de clúster para cada subred y usar la dependencia "OR".  Puede encontrar más información en [configuración de direcciones IP y dependencias para clústeres de varias subredes, parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).

4. Configure un testigo de recurso compartido de archivos o un testigo de nube para proporcionar el cuórum en caso de pérdida del sitio.

   > [!NOTE]
   > WIndows Server incluye ahora una opción para el testigo basado en la nube (Azure). Puede elegir esta opción de cuórum en lugar del testigo de recurso compartido de archivos.

   > [!WARNING]
   > Para más información sobre la configuración de cuórum, consulte la sección Configuración de testigos de la guía [Configurar y administrar el cuórum en un clúster de conmutación por error de Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612870(v=ws.11)). Para más información sobre el cmdlet `Set-ClusterQuorum`, consulte [Set-ClusterQuorum](/powershell/module/failoverclusters/set-clusterquorum).

5. Revise [Recomendaciones de red para un clúster de Hyper-V en Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn550728(v=ws.11)) y asegúrese de que ha configurado las redes en clúster de forma óptima.

6. Agregue un disco del sitio de Redmond al CSV de clúster. Para ello, haga clic con el botón derecho en un disco de origen el nodo **Discos** de la sección **Almacenamiento** y, a continuación, haga clic en **Agregar a volúmenes compartidos de clúster**.

7. Mediante la guía [Implementar un clúster de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj863389(v=ws.11)), siga los pasos de 7 a 10 en el sitio de **Redmond** para crear una máquina virtual de prueba solo para asegurarse de que el clúster funciona con normalidad dentro de los dos nodos que comparten el almacenamiento en el primer sitio de prueba.

8. Si va a crear un clúster extendido de dos nodos, debe agregar todo el almacenamiento antes de continuar. Para ello, abra una sesión de PowerShell con permisos administrativos en los nodos del clúster y ejecute el siguiente comando: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Este es el comportamiento predeterminado en Windows Server 2016.

9. Inicie Windows PowerShell y use el cmdlet `Test-SRTopology` para determinar si se cumplen todos los requisitos de Réplica de almacenamiento.

    Por ejemplo, para validar dos de los nodos de clúster extendido propuestos, donde cada uno tiene un volumen **F:** y **G:**, y ejecutar la prueba durante 30 minutos:
   1. Mueva todo el almacenamiento disponible a **SR-SRV01**.
   2. Haga clic en **Crear rol vacío** en la sección **Roles** del Administrador de clústeres de conmutación por error.
   3. Agregue el almacenamiento en línea a ese rol vacío denominado **Nuevo rol**.
   4. Mueva todo el almacenamiento disponible a **SR-SRV03**.
   5. Haga clic en **Crear rol vacío** en la sección **Roles** del Administrador de clústeres de conmutación por error.
   6. Mueva el **Nuevo rol (2)** vacío a **SR-SRV03**.
   7. Agregue el almacenamiento en línea a ese rol vacío denominado **Nuevo rol (2)**.
   8. Ahora ha montado todo el almacenamiento con letras de unidad, y puede evaluar el clúster con `Test-SRTopology`.

        Por ejemplo:

        ```
        MD c:\temp

        Test-SRTopology -SourceComputerName SR-SRV01 -SourceVolumeName D: -SourceLogVolumeName E: -DestinationComputerName SR-SRV03 -DestinationVolumeName D: -DestinationLogVolumeName E: -DurationInMinutes 30 -ResultPath c:\temp
        ```

      > [!IMPORTANT]
      > Al usar un servidor de prueba sin carga de E/S de escritura en el volumen de origen especificado durante el período de evaluación, considere la posibilidad de agregar una carga de trabajo o Test-SRTopology no generará un informe útil. Pruebe con cargas de trabajo del estilo de producción para ver números reales y tamaños de registro recomendados. Otra alternativa es copiar algunos archivos en el volumen de origen durante la prueba o descargar y ejecutar DISKSPD para generar E/S de escritura. Por ejemplo, una muestra con una carga de trabajo de e/s de escritura baja durante diez minutos en el volumen D:`Diskspd.exe -c1g -d600 -W5 -C5 -b4k -t2 -o2 -r -w5 -i100 d:\test.dat`

1.  Examine el informe **TestSrTopologyReport-< date >.html** para asegurarse de que cumple los requisitos de Réplica de almacenamiento y observe la predicción de tiempo de sincronización inicial y las recomendaciones de registro.

      ![Pantalla que muestra el informe de replicación](./media/Stretch-Cluster-Replication-Using-Shared-Storage/SRTestSRTopologyReport.png)

2.  Devuelva los discos al almacenamiento disponible y quite los roles vacíos temporales.

3.  Una vez satisfecho, quite la máquina virtual de prueba. Agregue las máquinas virtuales de prueba reales que se necesitan para la evaluación posterior de un nodo de origen propuesto.

4.  Configure el reconocimiento de sitios de clústeres extendidos para que los servidores **Sr-SRV01** y **Sr-SRV02** estén en el sitio **Redmond**, **Sr-SRV03** y **Sr-SRV04** estén en el sitio **Bellevue**, y **Redmond** es el preferido para la propiedad del nodo del almacenamiento de origen y las máquinas virtuales:

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue

    (Get-Cluster).PreferredSite="Seattle"
    ```

    > [!NOTE]
    > No hay ninguna opción para configurar el reconocimiento de sitios mediante el Administrador de clústeres de conmutación por error en Windows Server 2016.

5.  **(Opcional)** Configure redes en clúster y Active Directory para una conmutación por error de sitio DNS más rápida. Puede utilizar redes definidas por software de Hyper-V, VLAN extendidas, dispositivos de abstracción de red, TTL de DNS reducido y otras técnicas habituales.

    Para más información, revise la sesión de Microsoft Ignite: [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext (Extensión de clústeres de conmutación por error y uso de la réplica de almacenamiento en Windows Server vNext](https://channel9.msdn.com/Events/Ignite/2015/BRK3487) y la entrada de blog [Enable Change Notifications between Sites - How and Why? (Habilitar las notificaciones de cambio entre sitios: ¿cómo y por qué?)](/archive/blogs/qzaidi/enable-change-notifications-between-sites-how-and-why).

6.  **(Opcional)** Configure la resistencia de la máquina virtual para que los invitados no se detengan durante mucho tiempo durante errores de nodo. En su lugar, estos aplican la conmutación por error al nuevo almacenamiento del origen de la replicación en el plazo de 10 segundos.

    ```PowerShell
    (Get-Cluster).ResiliencyDefaultPeriod=10
    ```

    > [!NOTE]
    >  No hay ninguna opción para configurar la resistencia de la máquina virtual mediante el Administrador de clústeres de conmutación por error en Windows Server 2016.

#### <a name="windows-powershell-method"></a>Método de Windows PowerShell

1. Pruebe el clúster propuesto y analice los resultados para asegurarse de que puede continuar:

   ```PowerShell
   Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04
   ```

   > [!NOTE]
   >  Es de esperar que aparezcan errores de almacenamiento en la validación del clúster, debido al uso del almacenamiento asimétrico.

2. Cree el servidor de archivos para el clúster de almacenamiento de uso general (debe especificar su propia dirección IP estática que usará el clúster). Asegúrese de que el nombre del clúster contiene 15 caracteres o menos.  Si los nodos residen en subredes diferentes, se debe crear una dirección IP para el sitio adicional mediante la dependencia "OR". Puede encontrar más información en [configuración de direcciones IP y dependencias para clústeres de varias subredes, parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).
   ```PowerShell
   New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here>
   Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”
   Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
   ```

3. Configure un testigo de recurso compartido de archivos o un testigo en la nube (Azure) en el clúster que apunta a un recurso compartido hospedado en el controlador de dominio o algún otro tipo de servidor independiente. Por ejemplo:

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\someserver\someshare
   ```

   > [!NOTE]
   > WIndows Server incluye ahora una opción para el testigo basado en la nube (Azure). Puede elegir esta opción de cuórum en lugar del testigo de recurso compartido de archivos.

   Para más información sobre la configuración de cuórum, consulte la sección Configuración de testigos de la guía [Configurar y administrar el cuórum en un clúster de conmutación por error de Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612870(v=ws.11)). Para más información sobre el cmdlet `Set-ClusterQuorum`, consulte [Set-ClusterQuorum](/powershell/module/failoverclusters/set-clusterquorum).

4. Revise [Recomendaciones de red para un clúster de Hyper-V en Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn550728(v=ws.11)) y asegúrese de que ha configurado las redes en clúster de forma óptima.

5. Si va a crear un clúster extendido de dos nodos, debe agregar todo el almacenamiento antes de continuar. Para ello, abra una sesión de PowerShell con permisos administrativos en los nodos del clúster y ejecute el siguiente comando: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Este es el comportamiento predeterminado en Windows Server 2016.

6. Mediante la guía [Implementar un clúster de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj863389(v=ws.11)), siga los pasos de 7 a 10 en el sitio de **Redmond** para crear una máquina virtual de prueba solo para asegurarse de que el clúster funciona con normalidad dentro de los dos nodos que comparten el almacenamiento en el primer sitio de prueba.

7. Una vez satisfecho, quite la máquina virtual de prueba. Agregue las máquinas virtuales de prueba reales que se necesitan para la evaluación posterior de un nodo de origen propuesto.

8. Configure el reconocimiento de sitios de clústeres extendidos para que los servidores **Sr-SRV01** y **Sr-SRV02** estén en el sitio **Redmond**, **Sr-SRV03** y **Sr-SRV04** estén en el sitio **Bellevue**, y **Redmond** es el preferido para la propiedad de nodo del almacenamiento de origen y las máquinas virtuales:

   ```PowerShell
   New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"

   New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"

   Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle
   Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle
   Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue
   Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue

   (Get-Cluster).PreferredSite="Seattle"
   ```

9. **(Opcional)** Configure redes en clúster y Active Directory para una conmutación por error de sitio DNS más rápida. Puede utilizar redes definidas por software de Hyper-V, VLAN extendidas, dispositivos de abstracción de red, TTL de DNS reducido y otras técnicas habituales.

   Para más información, revise la sesión de Microsoft Ignite: [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext (Extensión de clústeres de conmutación por error y uso de la Réplica de almacenamiento en Windows Server vNext](https://channel9.msdn.com/Events/Ignite/2015/BRK3487) y [Enable Change Notifications between Sites - How and Why? (Habilitar las notificaciones de cambio entre sitios: ¿cómo y por qué?)](/archive/blogs/qzaidi/enable-change-notifications-between-sites-how-and-why).

10. **(Opcional)** Configure la resistencia de la máquina virtual para que los invitados no entren en pausas prolongadas durante los errores en nodos. En su lugar, estos aplican la conmutación por error al nuevo almacenamiento del origen de la replicación en el plazo de 10 segundos.

    ```PowerShell
    (Get-Cluster).ResiliencyDefaultPeriod=10
    ```

    > [!NOTE]
    > No hay ninguna opción para la resistencia de la máquina virtual mediante el Administrador de clústeres de conmutación por error en Windows Server 2016.



### <a name="configure-a-file-server-for-general-use-cluster"></a><a name="BKMK_FileServer"></a>Configuración de un servidor de archivos para clúster de uso general

>[!NOTE]
> Omita esta sección si ya ha configurado un clúster de conmutación por error de Hyper-V como se describe en [Configuración de un clúster de conmutación por error de Hyper-V](#BKMK_HyperV).

Ahora creará un clúster de conmutación por error normal. Después de la configuración, la validación y las pruebas, lo extenderá mediante Réplica de almacenamiento. Puede realizar todos los pasos siguientes en los nodos del clúster directamente o desde un equipo de administración remota que contenga el Herramientas de administración remota del servidor de Windows Server.

#### <a name="graphical-method"></a>Método gráfico

1. Ejecute cluadmin.msc.

2. Valide el clúster propuesto y analice los resultados para asegurarse de que puede continuar.
   >[!NOTE]
   >Es de esperar que aparezcan errores de almacenamiento en la validación del clúster, debido al uso del almacenamiento asimétrico.
3. Cree el servidor de archivos para el clúster de almacenamiento de uso general. Asegúrese de que el nombre del clúster contiene 15 caracteres o menos. En el ejemplo a continuación se usa SR-SRVCLUS.  Si los nodos van a residir en subredes diferentes, debe crear una dirección IP para el nombre de clúster para cada subred y usar la dependencia "OR".  Puede encontrar más información en [configuración de direcciones IP y dependencias para clústeres de varias subredes, parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).

4. Configure un testigo de recurso compartido de archivos o un testigo de nube para proporcionar el cuórum en caso de pérdida del sitio.
   >[!NOTE]
   > WIndows Server incluye ahora una opción para el testigo basado en la nube (Azure). Puede elegir esta opción de cuórum en lugar del testigo de recurso compartido de archivos.
   >[!NOTE]
   >  Para más información sobre la configuración de cuórum, consulte la sección Configuración de testigos de la guía [Configurar y administrar el cuórum en un clúster de conmutación por error de Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612870(v=ws.11)). Para más información sobre el cmdlet Set-ClusterQuorum, consulte [Set-ClusterQuorum](/powershell/module/failoverclusters/set-clusterquorum).

5. Si va a crear un clúster extendido de dos nodos, debe agregar todo el almacenamiento antes de continuar. Para ello, abra una sesión de PowerShell con permisos administrativos en los nodos del clúster y ejecute el siguiente comando: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Este es el comportamiento predeterminado en Windows Server 2016.

6. Asegúrese de que ha configurado las redes en clúster de forma óptima.
    >[!NOTE]
    > El rol Servidor de archivos debe instalarse en todos los nodos antes de continuar con el paso siguiente.   |

7. En **Roles**, haga clic en **Configurar rol**. Revise **Antes de comenzar** y haga clic en **Siguiente**.

8. Seleccione **Servidor de archivos** y haga clic en **Siguiente**.

9. Deje seleccionado **Servidor de archivos para uso general** y haga clic en **Siguiente**.

10. Proporcione un nombre para el **Punto de acceso cliente** (de 15 caracteres o menos) y haga clic en **Siguiente**.

11. Seleccione un disco para que sea el volumen de datos y haga clic en **Siguiente**.

12. Revise la configuración y haga clic en **Siguiente**. Haga clic en **Finalizar**

13. Haga clic con el botón derecho en el rol Servidor de archivos y, a continuación, haga clic en **Agregar recurso compartido de archivos**. Continúe con el asistente para configurar recursos compartidos.

14. Opcional: agregue otro rol Servidor de archivos que use el otro almacenamiento en este sitio.

15. Configure el reconocimiento de sitios de clúster extendido para que los servidores SR-SRV01 y SR-SRV02 estén en el sitio Redmond, SR-SRV03 y SRV04 SR estén en el sitio Bellevue, y Redmond sea el preferido para tener la propiedad del nodo de almacenamiento de origen y las máquinas virtuales:

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue

    (Get-Cluster).PreferredSite="Seattle"
    ```

      >[!NOTE]
      > No hay ninguna opción para configurar el reconocimiento de sitios mediante el Administrador de clústeres de conmutación por error en Windows Server 2016.

16. (Opcional) Configure redes en clúster y Active Directory para una conmutación por error de sitio DNS más rápida. Puede utilizar VLAN extendidas, dispositivos de abstracción de red, TTL de DNS reducido y otras técnicas habituales.

Para más información, revise la sesión de Microsoft Ignite: [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext (Extensión de clústeres de conmutación por error y uso de la réplica de almacenamiento en Windows Server vNext](https://channel9.msdn.com/events/ignite/2015/brk3487) y [Enable Change Notifications between Sites - How and Why? (Habilitar las notificaciones de cambio entre sitios: ¿cómo y por qué?)](/archive/blogs/qzaidi/enable-change-notifications-between-sites-how-and-why).

#### <a name="powershell-method"></a>Método de PowerShell

1. Pruebe el clúster propuesto y analice los resultados para asegurarse de que puede continuar:

    ```PowerShell
    Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04
    ```

    > [!NOTE]
    >  Es de esperar que aparezcan errores de almacenamiento en la validación del clúster, debido al uso del almacenamiento asimétrico.

2.  Cree el clúster de proceso de Hyper-V (debe especificar su propia dirección IP estática que usará el clúster). Asegúrese de que el nombre del clúster contiene 15 caracteres o menos.  Si los nodos residen en subredes diferentes, se debe crear una dirección IP para el sitio adicional mediante la dependencia "OR". Puede encontrar más información en [configuración de direcciones IP y dependencias para clústeres de varias subredes, parte III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).

    ```PowerShell
    New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here>

    Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”

    Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
    ```


3. Configure un testigo de recurso compartido de archivos o un testigo en la nube (Azure) en el clúster que apunta a un recurso compartido hospedado en el controlador de dominio o algún otro tipo de servidor independiente. Por ejemplo:

    ```PowerShell
    Set-ClusterQuorum -FileShareWitness \\someserver\someshare
    ```

    >[!NOTE]
    > Windows Server incluye ahora una opción para el testigo en la nube con Azure. Puede elegir esta opción de cuórum en lugar del testigo de recurso compartido de archivos.

   Para obtener más información sobre la configuración de cuórum, vea el tema [Descripción del cuórum del clúster y del grupo](../storage-spaces/understand-quorum.md). Para más información sobre el cmdlet Set-ClusterQuorum, consulte [Set-ClusterQuorum](/powershell/module/failoverclusters/set-clusterquorum).

4.  Si va a crear un clúster extendido de dos nodos, debe agregar todo el almacenamiento antes de continuar. Para ello, abra una sesión de PowerShell con permisos administrativos en los nodos del clúster y ejecute el siguiente comando: `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

    Este es el comportamiento predeterminado en Windows Server 2016.

5. Asegúrese de que ha configurado las redes en clúster de forma óptima.

6.  Configure un rol Servidor de archivos. Por ejemplo:

    ```PowerShell
    Get-ClusterResource
    Add-ClusterFileServerRole -Name SR-CLU-FS2 -Storage "Cluster Disk 4"

    MD e:\share01

    New-SmbShare -Name Share01 -Path f:\share01 -ContinuouslyAvailable $false
    ```

7. Configure el reconocimiento de sitios de clúster extendido para que los servidores SR-SRV01 y SR-SRV02 estén en el sitio Redmond, SR-SRV03 y SRV04 SR estén en el sitio Bellevue, y Redmond sea el preferido para tener la propiedad del nodo de almacenamiento de origen y las máquinas virtuales:

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue

    (Get-Cluster).PreferredSite="Seattle"
    ```

8.  (Opcional) Configure redes en clúster y Active Directory para una conmutación por error de sitio DNS más rápida. Puede utilizar VLAN extendidas, dispositivos de abstracción de red, TTL de DNS reducido y otras técnicas habituales.

    Para más información, revise la sesión de Microsoft Ignite: [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext (Extensión de clústeres de conmutación por error y uso de la réplica de almacenamiento en Windows Server vNext](https://channel9.msdn.com/events/ignite/2015/brk3487) y [Enable Change Notifications between Sites - How and Why? (Habilitar las notificaciones de cambio entre sitios: ¿cómo y por qué?)](/archive/blogs/qzaidi/enable-change-notifications-between-sites-how-and-why).

### <a name="configure-a-stretch-cluster"></a>Configuración de un clúster extendido
Ahora va a configurar el clúster extendido mediante el Administrador de clústeres de conmutación por error o Windows PowerShell. Puede realizar todos los pasos siguientes en los nodos del clúster directamente o desde un equipo de administración remota que contenga el Herramientas de administración remota del servidor de Windows Server.

#### <a name="failover-cluster-manager-method"></a>Método del Administrador de clústeres de conmutación por error

1.  Para cargas de trabajo de Hyper-V, en un nodo en que tenga los datos que desee replicar, agregue el disco de datos de origen de los discos disponibles a los volúmenes compartidos de clúster si aún no está configurado. No agregue todos los discos; solamente uno de ellos. En este punto, la mitad de los discos aparecerá sin conexión porque se trata de almacenamiento asimétrico.
Si se replica la carga de un recurso de disco físico (PDR) como Servidor de archivos para uso general, ya tiene un disco conectado al rol listo para usarse.

    ![Pantalla que muestra el Administrador de clústeres de conmutación por error.](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_OnlineDisks2.png)

2.  Haga clic con el botón derecho en el disco CSV o el disco conectado al rol, haga clic en **Replicación** y, a continuación, haga clic en **Habilitar**.

3.  Seleccione el volumen de datos de destino adecuado y haga clic en **Siguiente**. Los discos de destino mostrados tendrán un volumen del mismo tamaño que el disco de origen seleccionado. Cuando se mueva entre estos cuadros de diálogo del asistente, el almacenamiento disponible se moverá automáticamente y se conectará en segundo plano según sea necesario.

    ![Pantalla que muestra la página Seleccionar disco de destino del Asistente para configurar la réplica de almacenamiento](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_SelectDestinationDataDisk2.png)

4.  Seleccione el disco de registro de origen adecuado y haga clic en **Siguiente**. El volumen de registro de origen debe estar en un disco que use SSD o medios rápidos similares, no discos de giro.

5.  Seleccione el volumen de registro de destino adecuado y haga clic en Siguiente. Los discos de registro de destino mostrados tendrán un volumen del mismo tamaño que el volumen del disco de registro de origen seleccionado.

6.  Deje el valor de **sobrescribir volumen** en **Sobrescribir volumen de destino** si el volumen de destino no contiene una copia anterior de los datos del servidor de origen. Si el destino contiene datos similares, de una copia de seguridad reciente o de una replicación anterior, seleccione **Disco de destino inicializado** y, a continuación, haga clic en **Siguiente**.

7.  Deje el valor de **modo de replicación** en **Replicación sincrónica** si tiene previsto utilizar la replicación con un RPO de cero. Cámbielo a **Replicación asincrónica** si planea expandir el clúster por redes con una latencia mayor o necesita una menor latencia de E/S en los nodos del sitio principal.
8.  Deje el valor del **grupo de coherencia** en **Mayor rendimiento** si no tiene previsto usar el orden de escritura más adelante con pares de disco adicionales en el grupo de replicación. Si prevé la adición de más discos a este grupo de replicación y necesita un orden de escritura garantizado, seleccione **Habilitar el orden de escritura** y, a continuación, haga clic en **Siguiente**.

9.  Haga clic en **Siguiente** para configurar la replicación y la formación de clúster extendido.

    ![Pantalla que muestra la página Seleccionar disco de destino del Asistente para configurar la réplica de almacenamiento](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ConfigureSR2.png)

10. En la pantalla de resumen, observe los resultados del cuadro de diálogo de finalización. Puede ver el informe en un explorador web.

11. En este punto, ha configurado una asociación de Réplica de almacenamiento entre las dos mitades del clúster, pero la replicación está en curso. Hay varias formas de ver el estado de replicación a través de una herramienta gráfica.

    1.  Use la columna **rol de replicación** y la pestaña **replicación** . Cuando se realiza la sincronización inicial, los discos de origen y de destino tendrán un estado de replicación **replicando continuamente**.

        ![Pantalla que muestra la ficha Replicación de un disco en el Administrador de clústeres de conmutación por error](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ReplicationDetails2.png)

    2.  Inicie **eventvwr.exe**.

        1.  En el servidor de origen, vaya a **Applications and Services \ Microsoft \ Windows \ StorageReplica \ Admin** y examine los eventos 5015, 5002, 5004, 1237, 5001 y 2200.

        2.  En el servidor de destino, vaya a **Applications and Services \ Microsoft \ Windows \ StorageReplica \ Operational** y espere al evento 1215. Este evento indica el número de bytes copiados y el tiempo insumido. Ejemplo:

            ```
            Log Name:      Microsoft-Windows-StorageReplica/Operational
            Source:        Microsoft-Windows-StorageReplica
            Date:          4/6/2016 4:52:23 PM
            Event ID:      1215
            Task Category: (1)
            Level:         Information
            Keywords:      (1)
            User:          SYSTEM
            Computer:      SR-SRV03.Threshold.nttest.microsoft.com
            Description:
            Block copy completed for replica.

            ReplicationGroupName: Replication 2
            ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}
            ReplicaName: \\?\Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}\
            ReplicaId: {00000000-0000-0000-0000-000000000000}
            End LSN in bitmap:
            LogGeneration: {00000000-0000-0000-0000-000000000000}
            LogFileId: 0
            CLSFLsn: 0xFFFFFFFF
            Number of Bytes Recovered: 68583161856
            Elapsed Time (ms): 140
            ```

        3.  En el servidor de destino, vaya a **Applications and Services \ Microsoft \ Windows \ StorageReplica \ Admin** y examine 5009, 1237, 5001, 5015, 5005 y 2200 para entender el progreso del procesamiento de eventos. No debería haber ninguna advertencia de error en esta secuencia. Habrá muchos eventos 1237; estos indican el progreso.

            > [!WARNING]
            > El uso de CPU y de la memoria es probable que sea mayor al normal hasta que finalice la sincronización inicial.

#### <a name="windows-powershell-method"></a>Método de Windows PowerShell

1.  Asegúrese de que la consola de Powershell se ejecuta con una cuenta de administrador con privilegios elevados.
2.  Agregue el almacenamiento de datos de origen solo al clúster como CSV. Para obtener el tamaño, la partición y el diseño de volumen de los discos disponibles, use los siguientes comandos:

    ```PowerShell
    Move-ClusterGroup -Name "available storage" -Node sr-srv01

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }
    $DiskResources | foreach {
        $resource = $_
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining
    } | FT -AutoSize

    Move-ClusterGroup -Name "available storage" -Node sr-srv03

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }
    $DiskResources | foreach {
        $resource = $_
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining
    } | FT -AutoSize
    ```

4.  Establezca el disco correcto en CSV con:

    ```PowerShell
    Add-ClusterSharedVolume -Name "Cluster Disk 4"
    Get-ClusterSharedVolume
    Move-ClusterSharedVolume -Name "Cluster Disk 4" -Node sr-srv01
    ```

5.  Configure el clúster extendido, especificando lo siguiente:

    -   Los nodos de origen y de destino (donde los datos de origen son un disco CSV y el resto de discos no lo son).

    -   Los nombres del grupo de replicación de origen y de destino.

    -   Los discos de origen y de destino, donde los tamaños de partición coinciden.

    -   Los volúmenes de registros de origen y de destino, donde hay suficiente espacio libre para contener el tamaño del registro en ambos discos y el almacenamiento es de tipo SSD o un medio rápido similar.

    -   Los volúmenes de registros de origen y de destino, donde hay suficiente espacio libre para contener el tamaño del registro en ambos discos y el almacenamiento es de tipo SSD o un medio rápido similar.

    -   El tamaño del registro.

    -   El volumen de registro de origen debe estar en un disco que use SSD o medios rápidos similares, no discos de giro.

    ```PowerShell
    New-SRPartnership -SourceComputerName sr-srv01 -SourceRGName rg01 -SourceVolumeName "C:\ClusterStorage\Volume1" -SourceLogVolumeName e: -DestinationComputerName sr-srv03 -DestinationRGName rg02 -DestinationVolumeName d: -DestinationLogVolumeName e:
    ```

    > [!NOTE]
    > También puede utilizar `New-SRGroup` en un nodo en cada sitio y `New-SRPartnership` para crear la replicación en fases, en lugar de todo a la vez.

6.  Determine el progreso de la replicación.

    1.  En el servidor de origen, ejecute el siguiente comando y examine los eventos 5015, 5002, 5004, 1237, 5001 y 2200:

        ```PowerShell
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20
        ```

    2.  En el servidor de destino, ejecute el siguiente comando para ver los eventos de Réplica de almacenamiento que muestran la creación de la asociación. Este evento indica el número de bytes copiados y el tiempo insumido. Ejemplo:

        ```
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl

        TimeCreated  : 4/6/2016 4:52:23 PM
        ProviderName : Microsoft-Windows-StorageReplica
        Id           : 1215
        Message      : Block copy completed for replica.

               ReplicationGroupName: Replication 2
               ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}
               ReplicaName: ?Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}
               ReplicaId: {00000000-0000-0000-0000-000000000000}
               End LSN in bitmap:
               LogGeneration: {00000000-0000-0000-0000-000000000000}
               LogFileId: 0
               CLSFLsn: 0xFFFFFFFF
               Number of Bytes Recovered: 68583161856
               Elapsed Time (ms): 140
        ```

    3.  En el servidor de destino, ejecute el siguiente comando y examine los eventos 5009, 1237, 5001, 5015, 5005 y 2200 para entender el progreso del procesamiento. No debería haber ninguna advertencia de error en esta secuencia. Habrá muchos eventos 1237; estos indican el progreso.

        ```PowerShell
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL
        ```

    4.  También es posible que el grupo de servidores de destino de la réplica indique el número de bytes que quedan por copiar en todo momento, lo cual se puede consultar mediante PowerShell. Por ejemplo:

        ```PowerShell
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining
        ```

        Como ejemplo de progreso (que no terminará):

        ```PowerShell
        while($true) {

         $v = (Get-SRGroup -Name "Replication 2").replicas | Select-Object numofbytesremaining
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)
         Start-Sleep -s 5
        }
        ```

7.  Para obtener el estado de origen y destino de replicación en el clúster extendido, use `Get-SRGroup` y `Get-SRPartnership` para ver el estado configurado de la replicación en el clúster extendido.

    ```PowerShell
    Get-SRGroup
    Get-SRPartnership
    (Get-SRGroup).replicas
    ```

### <a name="manage-stretched-cluster-replication"></a>Administración de la replicación de clúster extendido
Ahora podrá administrar y hacer funcionar su clúster extendido. Puede realizar todos los pasos siguientes en los nodos del clúster directamente o desde un equipo de administración remota que contenga el Herramientas de administración remota del servidor de Windows Server.

#### <a name="graphical-tools-method"></a>Método de herramientas gráficas

1.  Use el Administrador de clústeres de conmutación por error para determinar el origen y el destino actuales de la replicación y su estado.

2.  Para medir el rendimiento de la replicación, ejecute **Perfmon.exe** en los nodos de origen y de destino.

    1.  En el nodo de destino:

        1.  Agregue los objetos de **Estadísticas de Réplica de almacenamiento** con todos los contadores de rendimiento para el volumen de datos.

        2.  Examine los resultados.

    2.  En el nodo de origen:

        1.  Agregue los objetos **Estadísticas de Réplica de almacenamiento** y **Estadísticas de E/S de partición de Réplica de almacenamiento** con todos los contadores de rendimiento para el volumen de datos (el último solo está disponible con los datos en el servidor de origen actual).

        2.  Examine los resultados.

3.  Para modificar el origen y el destino de la replicación en el clúster extendido, use los métodos siguientes:

    1.  Para mover la replicación de origen entre los nodos en el mismo sitio: haga clic con el botón derecho en el CSV de origen, haga clic en **Mover almacenamiento**, haga clic en **Seleccionar nodo** y, a continuación, seleccione un nodo en el mismo sitio. Si utiliza almacenamiento distinto de CSV para un disco asignado de roles, se mueve el rol.

    2.  Para mover la replicación de origen de un sitio a otro: haga clic con el botón derecho en el CSV de origen, haga clic en **Mover almacenamiento**, haga clic en **Seleccionar nodo** y, a continuación, seleccione un nodo en otro sitio. Si configura un sitio preferido, puede utilizar el mejor nodo posible para mover siempre el almacenamiento de origen a un nodo en el sitio preferido. Si utiliza almacenamiento distinto de CSV para un disco asignado de roles, se mueve el rol.

    3.  Para realizar la conmutación por error planeada en la dirección de replicación de un sitio a otro: cierre ambos nodos en un sitio con **ServerManager.exe** o **SConfig**.

    4.  Para realizar la conmutación por error no planeada en la dirección de replicación de un sitio a otro: desconecte ambos nodos de un sitio.

        > [!NOTE]
        > En Windows Server 2016, es posible que tenga que usar el Administrador de clústeres de conmutación por error o Move-ClusterGroup para mover los discos de destino de nuevo al otro sitio manualmente una vez que los nodos vuelvan a estar en línea.

        > [!NOTE]
        > La réplica de almacenamiento desmonta los volúmenes de destino. es así por diseño.

4.  Para cambiar el tamaño del registro del valor predeterminado de 8 GB, haga clic con el botón secundario en los discos de registro de origen y de destino, haga clic en la pestaña **registro de replicación** y, a continuación, cambie los tamaños de los discos para que coincidan.

    > [!NOTE]
    > El tamaño de registro predeterminado es 8 GB. Según los resultados del cmdlet `Test-SRTopology`, puede decidir usar `-LogSizeInBytes` con un valor superior o inferior.

5.  Para agregar otro par de discos replicados al grupo de replicación existente, debe asegurarse de que hay al menos un disco adicional en el almacenamiento disponible. A continuación, puede hacer clic con el botón derecho en el disco de origen y seleccionar **Agregar asociación de replicación**.
    > [!NOTE]
    > Esta necesidad de un disco adicional "de prueba" en el espacio de almacenamiento disponible se debe a una regresión y no es intencionada. Anteriormente, el Administrador de clústeres de conmutación por error permitía la adición de más discos normalmente y lo hará de nuevo en una versión futura.


6.  Para quitar la replicación existente:

    1.  Inicie **cluadmin.msc**.

    2.  Haga clic con el botón derecho en el disco CSV de origen y haga clic en **Replicación**; a continuación, haga clic en **Quitar**. Acepte el mensaje de advertencia.

    3.  Si lo desea, puede quitar el almacenamiento de CSV para devolverlo al almacenamiento disponible para realizar más pruebas.

        > [!NOTE]
        > Puede que necesite usar **DiskMgmt.msc** o **ServerManager.exe** para agregar letras de unidad de nuevo a los volúmenes después de volver al almacenamiento disponible.

#### <a name="windows-powershell-method"></a>Método de Windows PowerShell

1.  Use **Get-SRGroup** y **(Get-SRGroup).Replicas** para determinar el origen y el destino actuales de la replicación y su estado.

2.  Para medir el rendimiento de la replicación, utilice el cmdlet `Get-Counter` en los nodos de origen y de destino. Los nombres de contador son:

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de veces que se pausó el vaciado

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de E/S de vaciado pendientes

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de solicitudes para la última escritura del registro

    -   \ Estadísticas de e/s de partición de réplica (*) \Promedio de longitud de cola de vaciado

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Longitud actual de la cola de vaciado

    -   \Estadísticas de E/S de partición de Réplica de almacenamiento(*)\Número de solicitudes de escritura en aplicación

    -   \ Estadísticas de e/s de partición de réplica (*) \Promedio número de solicitudes por escritura de registro

    -   \ Estadísticas de e/s de partición de réplica (*) \Promedio de la aplicación de la latencia de escritura

    -   \ Estadísticas de e/s de partición de réplica (*) \Promedio latencia de lectura de la aplicación

    -   \Estadísticas de Réplica de almacenamiento(*)\RPO de destino

    -   \Estadísticas de Réplica de almacenamiento(*)\RPO actual

    -   \ Estadísticas de réplica de \Promedio (*) longitud de cola de registro de

    -   \Estadísticas de Réplica de almacenamiento(*)\Longitud de cola del registro actual

    -   \Estadísticas de Réplica de almacenamiento(*)\Nº total de bytes recibidos

    -   \Estadísticas de Réplica de almacenamiento(*)\Nº total de bytes enviados

    -   \ Estadísticas de réplica de \Promedio (*) latencia de envío de red de

    -   \Estadísticas de Réplica de almacenamiento(*)\Estado de la replicación

    -   \ Estadísticas de réplica de \Promedio (*) latencia de recorrido de ida y vuelta de mensajes

    -   \Estadísticas de Réplica de almacenamiento(*)\Tiempo transcurrido de la última recuperación

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de recuperación vaciadas

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de recuperación

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de replicación vaciadas

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de transacciones de replicación

    -   \Estadísticas de Réplica de almacenamiento(*)\Número máximo de secuencia de registro

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de mensajes recibidos

    -   \Estadísticas de Réplica de almacenamiento(*)\Número de mensajes enviados

    Para más información sobre los contadores de rendimiento en Windows PowerShell, consulte [Get-Counter](/powershell/module/microsoft.powershell.diagnostics/get-counter).

3.  Para modificar el origen y el destino de la replicación en el clúster extendido, use los métodos siguientes:

    1.  Para mover el origen de replicación de un nodo a otro en el sitio **Redmond**, mueva el recurso CSV mediante el cmdlet Move-ClusterSharedVolume.

        ```PowerShell
        Get-ClusterSharedVolume | fl *
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv02
        ```

    2.  Para mover la dirección de replicación de un sitio a otro "planificado", mueva el recurso CSV mediante el cmdlet **Move-ClusterSharedVolume**.

        ```PowerShell
        Get-ClusterSharedVolume | fl *
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv04
        ```

        Así se moverán también los registros y datos de forma apropiada para el otro sitio y los nodos.

    3.  Para realizar la conmutación por error no planeada en la dirección de replicación de un sitio a otro: desconecte ambos nodos de un sitio.

        > [!NOTE]
        > La réplica de almacenamiento desmonta los volúmenes de destino. es así por diseño.

4.  Para cambiar el tamaño del registro del valor predeterminado de 8 GB, use **set-SRGroup** en los grupos de réplica de almacenamiento de origen y de destino.   Por ejemplo, para establecer todos los registros en 2 GB:

    ```PowerShell
    Get-SRGroup | Set-SRGroup -LogSizeInBytes 2GB
    Get-SRGroup
    ```

5.  Para agregar otro par de discos replicados al grupo de replicación existente, debe asegurarse de que hay al menos un disco adicional en el almacenamiento disponible. A continuación puede hacer clic con el botón derecho en el disco de origen y seleccionar Agregar asociación de replicación.
       >[!NOTE]
       >Esta necesidad de un disco adicional "de prueba" en el espacio de almacenamiento disponible se debe a una regresión y no es intencionada. Anteriormente, el Administrador de clústeres de conmutación por error permitía la adición de más discos normalmente y lo hará de nuevo en una versión futura.

    Utilice el cmdlet **Set-SRPartnership** con los parámetros **-SourceAddVolumePartnership** y **-DestinationAddVolumePartnership**.
6.  Para quitar la replicación, utilice `Get-SRGroup`, Get-`SRPartnership`, `Remove-SRGroup` y `Remove-SRPartnership` en cualquier nodo.

    ```PowerShell
    Get-SRPartnership | Remove-SRPartnership
    Get-SRGroup | Remove-SRGroup
    ```

    > [!NOTE]
    > Si utiliza un equipo de administración remota, debe especificar el nombre del clúster para estos cmdlets y proporcionar los dos nombres de los grupos de replicación.

### <a name="related-topics"></a>Temas relacionados
- [Información general sobre Réplica de almacenamiento](storage-replica-overview.md)
- [Replicación de almacenamiento de servidor a servidor](server-to-server-storage-replication.md)
- [Replicación de almacenamiento de clúster a clúster](cluster-to-cluster-storage-replication.md)
- [Réplica de almacenamiento: problemas conocidos](storage-replica-known-issues.md)
- [Réplica de almacenamiento: Preguntas más frecuentes](storage-replica-frequently-asked-questions.md)

## <a name="see-also"></a>Consulte también
- [Windows Server 2016](../../get-started/windows-server-2016.md)
- [Espacios de almacenamiento directo en Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)
