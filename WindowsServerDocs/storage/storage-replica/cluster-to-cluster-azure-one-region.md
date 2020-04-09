---
title: Réplica de almacenamiento de clúster a clúster en la misma región de Azure
description: Replicación de almacenamiento de clúster a clúster en la misma región de Azure
author: arduppal
ms.author: arduppal
ms.date: 04/26/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 00dbf709139ef245b94a3f083ab83a12503131c2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856298"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>Réplica de almacenamiento de clúster a clúster en la misma región de Azure

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Puede configurar la replicación de clúster a almacenamiento de clúster en la misma región de Azure. En los ejemplos siguientes, usamos un clúster de dos nodos, pero la réplica de almacenamiento de clúster a clúster no está restringida a un clúster de dos nodos. La ilustración siguiente es un clúster de espacio de almacenamiento directo de dos nodos que se puede comunicar entre sí, que se encuentra en el mismo dominio y dentro de la misma región.

Vea los vídeos siguientes para ver un tutorial completo del proceso.

Parte uno
> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE26f2Y]

Parte dos
> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE269Pq]

![El diagrama de arquitectura que muestra la réplica de almacenamiento de clúster a clúster en Azure dentro de la misma región.](media/Cluster-to-cluster-azure-one-region/architecture.png)
> [!IMPORTANT]
> Todos los ejemplos a los que se hace referencia son específicos de la ilustración anterior.

1. Cree un [grupo de recursos](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) en el Azure portal en una región (**Sr-AZ2AZ** en el **oeste de EE. UU. 2**). 
2. Cree dos [conjuntos de disponibilidad](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM) en el grupo de recursos (**Sr-AZ2AZ**) creado anteriormente, uno para cada clúster. 
    a. Conjunto de disponibilidad (**az2azAS1**) b. Conjunto de disponibilidad (**az2azAS2**)
3. Cree una [red virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-Vnet**) en el grupo de recursos creado anteriormente (**Sr-az2az**) y tenga al menos una subred. 
4. Cree un [grupo de seguridad de red](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) y agregue una regla de seguridad de entrada para RDP: 3389. Puede optar por quitar esta regla una vez finalizada la instalación. 
5. Cree [máquinas virtuales](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) de Windows Server en el grupo de recursos creado anteriormente (**Sr-AZ2AZ**). Use la red virtual creada anteriormente (**az2az-Vnet**) y el grupo de seguridad de red (**az2az-NSG**). 
   
   Controlador de dominio (**az2azDC**). Puede optar por crear un tercer conjunto de disponibilidad para el controlador de dominio o agregar el controlador de dominio en uno de los dos conjuntos de disponibilidad. Si va a agregar esto al conjunto de disponibilidad creado para los dos clústeres, asígnele una dirección IP pública estándar durante la creación de la máquina virtual. 
   - Instale el servicio Dominio de Active Directory.
   - Crear un dominio (Contoso.com)
   - Creación de un usuario con privilegios de administrador (contosoadmin) 
   - Cree dos máquinas virtuales (**az2az1**, **az2az2**) en el primer conjunto de disponibilidad (**az2azAS1**). Asigne una dirección IP pública estándar a cada máquina virtual durante la creación propiamente dicha.
   - Agregar al menos 2 discos administrados a cada máquina
   - Instalar la característica de clúster de conmutación por error y réplica de almacenamiento
   - Cree dos máquinas virtuales (**az2az3**, **az2az4**) en el segundo conjunto de disponibilidad (**az2azAS2**). Asigne una dirección IP pública estándar a cada máquina virtual durante la creación propiamente dicha. 
   - Agregue al menos 2 discos administrados a cada equipo. 
   - Instalar la característica de clúster de conmutación por error y réplica de almacenamiento. 
   
6. Conecte todos los nodos al dominio y proporcione privilegios de administrador al usuario creado anteriormente. 

7. Cambie el servidor DNS de la red virtual a la dirección IP privada del controlador de dominio. 
8. En nuestro ejemplo, el controlador de dominio **az2azDC** tiene una dirección IP privada (10.3.0.8). En el Virtual Network (**az2az-Vnet**), cambie el servidor DNS 10.3.0.8. Conecte todos los nodos a "Contoso.com" y proporcione privilegios de administrador a "contosoadmin".
   - Inicie sesión como contosoadmin desde todos los nodos. 
    
9. Cree los clústeres (**SRAZC1**, **SRAZC2**). 
   A continuación se muestran los comandos de PowerShell para nuestro ejemplo
   ```PowerShell
    New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```PowerShell
    New-Cluster -Name SRAZC2 -Node az2az3,az2az4 –StaticAddress 10.3.0.101
   ```
10. Habilitar espacios de almacenamiento directo
    ```PowerShell
    Enable-clusterS2D
    ```   
   
    Para cada clúster, cree un disco virtual y un volumen. Uno para los datos y otro para el registro. 
   
11. Cree una SKU estándar interna [load balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) para cada clúster (**azlbr1**,**azlbr2**). 
   
    Proporcione la dirección IP del clúster como dirección IP privada estática para el equilibrador de carga.
    - azlbr1 = > front-end IP: 10.3.0.100 (seleccione una dirección IP no usada de la subred de red virtual (**az2az-Vnet**))
    - Cree un grupo de back-end para cada equilibrador de carga. Agregue los nodos de clúster asociados.
    - Crear sondeo de estado: Puerto 59999
    - Cree una regla de equilibrio de carga: permita puertos de alta disponibilidad con IP flotante habilitada. 
   
    Proporcione la dirección IP del clúster como dirección IP privada estática para el equilibrador de carga.
    - azlbr2 = > front-end IP: 10.3.0.101 (seleccione una dirección IP no usada de la subred de red virtual (**az2az-Vnet**))
    - Cree un grupo de back-end para cada equilibrador de carga. Agregue los nodos de clúster asociados.
    - Crear sondeo de estado: Puerto 59999
    - Cree una regla de equilibrio de carga: permita puertos de alta disponibilidad con IP flotante habilitada. 
   
12. En cada nodo del clúster, abra el puerto 59999 (sondeo de estado). 
   
    Ejecute el siguiente comando en cada nodo:
    ```PowerShell
    netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```   
13. Indique al clúster que escuche los mensajes de sondeo de estado en el puerto 59999 y que responda desde el nodo que actualmente posee este recurso. 
    Ejecútelo una vez desde cualquier nodo del clúster, para cada clúster. 
    
    En nuestro ejemplo, asegúrese de cambiar "ILBIP" según los valores de configuración. Ejecute el siguiente comando desde un nodo **az2az1**/**az2az2**:

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"ProbeFailureThreshold"=5;"EnableDhcp"=0}
    ```

14. Ejecute el siguiente comando desde un nodo **az2az3**/**az2az4**. 

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"ProbeFailureThreshold"=5;"EnableDhcp"=0}  
    ```   
    Asegúrese de que ambos clústeres pueden conectarse o comunicarse entre sí. 

    Use la característica "conectar con el clúster" en el administrador de clústeres de conmutación por error para conectarse al otro clúster o comprobar si hay otras respuestas de clúster de uno de los nodos del clúster actual.  
   
    ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
    ```
    ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
    ```   

15. Cree testigos en la nube para ambos clústeres. Cree dos [cuentas de almacenamiento](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**, **az2azcw2**) en Azure una para cada clúster del mismo grupo de recursos (**Sr-AZ2AZ**).

    - Copie el nombre y la clave de la cuenta de almacenamiento de "claves de acceso"
    - Cree el testigo en la nube desde el "Administrador de clústeres de conmutación por error" y use el nombre de cuenta y la clave anteriores para crearlo.

16. Ejecute las [pruebas de validación del clúster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) antes de continuar con el siguiente paso.

17. Inicie Windows PowerShell y use el cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) para determinar si satisface todos los requisitos de la Réplica de almacenamiento. Puede usar el cmdlet en un modo de solo requisitos para una prueba rápida, así como un modo de evaluación del rendimiento de ejecución prolongada.

18. Configure la réplica de almacenamiento de clúster a clúster.
   
    Conceder acceso de un clúster a otro en ambas direcciones:

    En nuestro ejemplo:

    ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
    ```
    Si usa Windows Server 2016, ejecute también este comando:

    ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
    ```   
   
19. Cree SRPartnership para los clústeres:</ol>

    - Para el clúster **SRAZC1**.
    - Ubicación del volumen:-c:\ClusterStorage\DataDisk1
    - Ubicación del registro:-g:
    - Para clúster **SRAZC2**
    - Ubicación del volumen:-c:\ClusterStorage\DataDisk2
    - Ubicación del registro:-g:

Ejecuta el siguiente comando:

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```