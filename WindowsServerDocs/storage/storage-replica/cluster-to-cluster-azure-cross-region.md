---
title: Intercambio entre regiones de la Réplica de almacenamiento de clúster a clúster en Azure
description: Replicación de clúster a almacenamiento en clúster entre regiones de Azure
keywords: Réplica de almacenamiento, Administrador del servidor, Windows Server, Azure, clúster, región cruzada, región distinta
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 26eba76c836d1157f4d4c10d7a989a3a7dcc1538
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393830"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Intercambio entre regiones de la Réplica de almacenamiento de clúster a clúster en Azure

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Puede configurar las réplicas de almacenamiento de clúster a clúster para aplicaciones entre regiones de Azure. En los ejemplos siguientes, usamos un clúster de dos nodos, pero la réplica de almacenamiento de clúster a clúster no está restringida a un clúster de dos nodos. La ilustración siguiente es un clúster de espacio de almacenamiento directo de dos nodos que se puede comunicar entre sí, que se encuentra en el mismo dominio y que son entre regiones.

Vea el vídeo siguiente para obtener un tutorial completo del proceso.
> [!video https://www.microsoft.com/en-us/videoplayer/embed/RE26xeW]

![El diagrama de arquitectura que presenta C2C SR en Azure con la misma región.](media/Cluster-to-cluster-azure-cross-region/architecture.png)
> [!IMPORTANT]
> Todos los ejemplos a los que se hace referencia son específicos de la ilustración anterior.


1. En el Azure Portal, cree [grupos de recursos](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) en dos regiones diferentes.

    Por ejemplo, **Sr-AZ2AZ** en el **oeste de EE. UU. 2** y **Sr-AZCROSS** en la región **centro-oeste de EE. UU.** , como se mostró anteriormente.

2. Cree dos [conjuntos de disponibilidad](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM), uno en cada grupo de recursos para cada clúster.
    - Conjunto de disponibilidad (**az2azAS1**) en (**Sr-AZ2AZ**)
    - Conjunto de disponibilidad (**azcross**) en (**Sr-azcross**)

3. Crear dos redes virtuales
   - Cree la [red virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-Vnet**) en el primer grupo de recursos (**Sr-az2az**), con una subred y una subred de puerta de enlace.
   - Cree la [red virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**azcross-VNET**) en el segundo grupo de recursos (**Sr-azcross**), con una subred y una subred de puerta de enlace.

4. Crear dos grupos de seguridad de red
   - Cree el [grupo de seguridad de red](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) en el primer grupo de recursos (**Sr-az2az**).
   - Cree el [grupo de seguridad de red](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**azcross-NSG**) en el segundo grupo de recursos (**Sr-azcross**).

   Agregue una regla de seguridad de entrada para RDP: 3389 a ambos grupos de seguridad de red. Puede optar por quitar esta regla una vez finalizada la instalación.

5. Cree [máquinas virtuales](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) de Windows Server en los grupos de recursos creados previamente.

   Controlador de dominio (**az2azDC**). Puede optar por crear un tercer conjunto de disponibilidad para el controlador de dominio o agregar el controlador de dominio en uno de los dos conjuntos de disponibilidad. Si va a agregar esto al conjunto de disponibilidad creado para los dos clústeres, asígnele una dirección IP pública estándar durante la creación de la máquina virtual.
      - Instale el servicio Dominio de Active Directory.
      - Crear un dominio (contoso.com)
      - Creación de un usuario con privilegios de administrador (contosoadmin)

   Cree dos máquinas virtuales (**az2az1**, **az2az2**) en el grupo de recursos (**Sr-AZ2AZ**) mediante la red virtual (**AZ2AZ-Vnet**) y el grupo de seguridad de red (**AZ2AZ-NSG**) en el conjunto de disponibilidad (**az2azAS1**). Asigne una dirección IP pública estándar a cada máquina virtual durante la creación propiamente dicha.
      - Agregue al menos dos discos administrados a cada equipo.
      - Instalar clústeres de conmutación por error y la característica réplica de almacenamiento

   Cree dos máquinas virtuales (**azcross1**, **azcross2**) en el grupo de recursos (**Sr-AZCROSS**) mediante la red virtual (**AZCROSS-VNET**) y el grupo de seguridad de red (**AZCROSS-NSG**) en el conjunto de disponibilidad (**AZCROSS-as**) . Asignar una dirección IP pública estándar a cada máquina virtual durante la creación propiamente dicha
      - Agregue al menos dos discos administrados a cada equipo.
      - Instalar clústeres de conmutación por error y la característica réplica de almacenamiento

   Conecte todos los nodos al dominio y proporcione privilegios de administrador al usuario creado anteriormente.

   Cambie el servidor DNS de la red virtual a la dirección IP privada del controlador de dominio.
   - En el ejemplo, el controlador de dominio **az2azDC** tiene una dirección IP privada (10.3.0.8). En el Virtual Network (**az2az-Vnet** y **azcross-Vnet**), cambie 10.3.0.8 de servidor DNS. 

     En el ejemplo, conecte todos los nodos a "contoso.com" y proporcione privilegios de administrador a "contosoadmin".
   - Inicie sesión como contosoadmin desde todos los nodos. 
 
6. Cree los clústeres (**SRAZC1**, **SRAZCross**).

   A continuación se muestran los comandos de PowerShell para el ejemplo
   ```powershell
      New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```powershell
      New-Cluster -Name SRAZCross -Node azcross1,azcross2 –StaticAddress 10.0.0.10
   ```

7. Habilitar espacios de almacenamiento directo.

   ```powershell
      Enable-clusterS2D
   ```

   > [!NOTE]
   > Para cada clúster, cree un disco virtual y un volumen. Uno para los datos y otro para el registro.

8. Cree una SKU estándar interna [load balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) para cada clúster (**azlbr1**, **azlbazcross**).

   Proporcione la dirección IP del clúster como dirección IP privada estática para el equilibrador de carga.
      - azlbr1 = > IP de front-end: 10.3.0.100 (seleccione una dirección IP no usada de la subred de la red virtual (**az2az-Vnet**))
      - Cree un grupo de back-end para cada equilibrador de carga. Agregue los nodos de clúster asociados.
      - Crear sondeo de estado: Puerto 59999
      - Cree una regla de equilibrio de carga: Permita puertos de alta disponibilidad con IP flotante habilitada.

   Proporcione la dirección IP del clúster como dirección IP privada estática para el equilibrador de carga. 
      - azlbazcross = > IP de front-end: 10.0.0.10 (seleccione una dirección IP no usada de la subred de la red virtual (**azcross-VNET**))
      - Cree un grupo de back-end para cada equilibrador de carga. Agregue los nodos de clúster asociados.
      - Crear sondeo de estado: Puerto 59999
      - Cree una regla de equilibrio de carga: Permita puertos de alta disponibilidad con IP flotante habilitada. 

9. Cree una [puerta de enlace de red virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM) para la conectividad de Vnet a Vnet.

   - Crear la primera puerta de enlace de red virtual (**az2az-VNetGateway**) en el primer grupo de recursos (**Sr-az2az**)
   - Tipo de puerta de enlace = VPN y tipo de VPN = basado en ruta

   - Crear la segunda puerta de enlace de red virtual (**azcross-VNetGateway**) en el segundo grupo de recursos (**Sr-azcross**)
   - Tipo de puerta de enlace = VPN y tipo de VPN = basado en ruta

   - Cree una conexión de red virtual a red virtual desde la primera puerta de enlace de red virtual a la segunda. Proporcionar una clave compartida

   - Cree una conexión de red virtual a red virtual desde la segunda puerta de enlace de red virtual a la primera puerta de enlace de red virtual. Proporcione la misma clave compartida que se proporciona en el paso anterior. 

10. En cada nodo del clúster, abra el puerto 59999 (sondeo de estado).

    Ejecute el siguiente comando en cada nodo:

    ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```

11. Indique al clúster que escuche los mensajes de sondeo de estado en el puerto 59999 y que responda desde el nodo que actualmente posee este recurso.

    Ejecútelo una vez desde cualquier nodo del clúster, para cada clúster. 
    
    En nuestro ejemplo, asegúrese de cambiar "ILBIP" según los valores de configuración. Ejecute el siguiente comando desde un nodo **az2az1**/**az2az2**

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

12. Ejecute el siguiente comando desde un nodo **azcross1**/**azcross2**
    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

    Asegúrese de que ambos clústeres pueden conectarse o comunicarse entre sí.

    Use la característica "conectar con el clúster" en el administrador de clústeres de conmutación por error para conectarse al otro clúster o comprobar si hay otras respuestas de clúster de uno de los nodos del clúster actual.

    En el ejemplo que hemos usado:
    ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
    ```
    ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
    ```

13. Cree un testigo en la nube para ambos clústeres. Cree dos [cuentas de almacenamiento](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**,**azcrosssa**) en Azure, una para cada clúster en cada grupo de recursos (**Sr-AZ2AZ**, **Sr-AZCROSS**).
   
    - Copie el nombre y la clave de la cuenta de almacenamiento de "claves de acceso"
    - Cree el testigo en la nube desde el "Administrador de clústeres de conmutación por error" y use el nombre de cuenta y la clave anteriores para crearlo. 

14. Ejecutar [pruebas de validación de clústeres](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) antes de continuar con el paso siguiente

15. Inicie Windows PowerShell y use el cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) para determinar si satisface todos los requisitos de la Réplica de almacenamiento. Puede usar el cmdlet en modo de solo requisitos para una prueba rápida, o en modo de evaluación de rendimiento de ejecución más larga.
 
16. Configure la réplica de almacenamiento de clúster a clúster.
    Conceder acceso de un clúster a otro en ambas direcciones:

    En nuestro ejemplo:
    ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
    ```
    Si usa Windows Server 2016, también ejecute este comando:

    ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
    ```

17. Cree una asociación de SR para los dos clústeres:</ol>

    - Para clúster **SRAZC1**
      - Ubicación del volumen:-c:\ClusterStorage\DataDisk1
      - Ubicación del registro:-g:
    - Para clúster **SRAZCross**
      - Ubicación del volumen:-c:\ClusterStorage\DataDiskCross
      - Ubicación del registro:-g:

Ejecute el comando:

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```