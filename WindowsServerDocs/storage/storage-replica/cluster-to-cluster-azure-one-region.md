---
title: Clúster al clúster de réplica de almacenamiento en la misma región de Azure
description: Clúster al clúster de replicación de almacenamiento en la misma región de Azure
keywords: Réplica de almacenamiento, el administrador del servidor, Windows Server, Azure, clúster, la misma región
author: arduppal
ms.author: arduppal
ms.date: 04/26/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 4371192d44878d3c953374b8d307b4d5612869f5
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461979"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>Clúster al clúster de réplica de almacenamiento en la misma región de Azure

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

Puede configurar la replicación de almacenamiento de clúster a clúster dentro de la misma región de Azure. En los ejemplos siguientes, usamos un clúster de dos nodos, pero la réplica de almacenamiento de clúster a clúster no está restringido a un clúster de dos nodos. La ilustración siguiente es un clúster de espacio de almacenamiento directo de dos nodos que puede comunicarse entre sí, se encuentran en el mismo dominio y en la misma región.

Vea los vídeos a continuación para ver un tutorial completo del proceso.

Primera parte
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE26f2Y]

Segunda parte
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE269Pq]

![El diagrama de arquitectura que muestra la réplica de almacenamiento de clúster a clúster de Azure dentro de la misma región.](media\Cluster-to-cluster-azure-one-region\architecture.png)
> [!IMPORTANT]
> Todos los ejemplos que se hace referencia son específicos de la ilustración anterior.

1. Crear un [grupo de recursos](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) en el portal de Azure en una región (**SR-AZ2AZ** en **oeste de EE.UU. 2**). 
2. Cree dos [conjuntos de disponibilidad](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM) en el grupo de recursos (**SR-AZ2AZ**) creado anteriormente, uno para cada clúster. 
    a. Conjunto de disponibilidad (**az2azAS1**) b. Conjunto de disponibilidad (**az2azAS2**)
3. Crear un [red virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az Vnet**) en el grupo de recursos creado anteriormente (**SR-AZ2AZ**), tener al menos una subred. 
4. Crear un [grupo de seguridad de red](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az NSG**) y agregue una regla de seguridad de entrada para RDP:3389. Puede quitar esta regla después de finalizar el programa de instalación. 
5. Creación de Windows Server [máquinas virtuales](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) en el grupo de recursos creado anteriormente (**SR-AZ2AZ**). Usar la red virtual creada previamente (**az2az Vnet**) y el grupo de seguridad de red (**az2az NSG**). 
   
   Controlador de dominio (**az2azDC**). Puede elegir crear un tercer conjunto de disponibilidad para el controlador de dominio o agregar el controlador de dominio en uno de los dos conjuntos de disponibilidad. Si va a agregar al conjunto de disponibilidad creado para los dos clústeres, asignar, una dirección IP pública estándar durante la creación de máquinas virtuales. 
   - Instalar servicio de dominio de Active Directory.
   - Crear un dominio (Contoso.com)
   - Crear un usuario con privilegios de administrador (contosoadmin) 
   - Cree dos máquinas virtuales (**az2az1**, **az2az2**) en la disponibilidad del primer conjunto (**az2azAS1**). Asignar una dirección IP pública estándar para cada máquina virtual durante la creación propia.
   - Agregar al menos 2 discos administrados a cada máquina
   - Instalar la característica de agrupación en clústeres de conmutación por error y réplica de almacenamiento
   - Cree dos máquinas virtuales (**az2az3**, **az2az4**) en la disponibilidad del segundo conjunto (**az2azAS2**). Asignar dirección IP pública estándar para cada máquina virtual durante la creación propia. 
   - Agregue al menos 2 discos administrados a cada máquina. 
   - Instalar la característica de agrupación en clústeres de conmutación por error y réplica de almacenamiento. 
   
6. Conectar todos los nodos al dominio y proporcionar los privilegios de administrador para el usuario creado anteriormente. 

7. Cambie el servidor DNS de la red virtual a la dirección IP privada de controlador de dominio. 
8. En nuestro ejemplo, el controlador de dominio **az2azDC** tiene la dirección IP privada (10.3.0.8). En la red Virtual (**az2az Vnet**) cambiar el servidor DNS 10.3.0.8. Conecte todos los nodos para "Contoso.com" y proporcionar los privilegios de administrador para "contosoadmin".
   - Inicie sesión como contosoadmin de todos los nodos. 
    
9. Crear los clústeres (**SRAZC1**, **SRAZC2**). A continuación encontrará los comandos de PowerShell para nuestro ejemplo
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
   
   Para cada clúster crear volúmenes y discos virtuales. Uno de los datos y otro para el registro. 
   
11. Creación de una SKU estándar interno [equilibrador de carga](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) para cada clúster (**azlbr1**,**azlbr2**). 
   
   Proporcionar la dirección IP del clúster como dirección IP privada estática para el equilibrador de carga.
   - azlbr1 => Frontend IP: 10.3.0.100 (seleccionar una dirección IP no utilizada de la red Virtual (**az2az Vnet**) subred)
   - Crear grupo de back-end para cada equilibrador de carga. Agregue los nodos de clúster asociado.
   - Cree el sondeo de estado: puerto 59999
   - Crear regla de equilibrio de carga: Permitir que los puertos de alta disponibilidad, con IP flotante habilitada. 
   
   Proporcionar la dirección IP del clúster como dirección IP privada estática para el equilibrador de carga.
   - azlbr2 => Frontend IP: 10.3.0.101 (seleccionar una dirección IP no utilizada de la red Virtual (**az2az Vnet**) subred)
   - Crear grupo de back-end para cada equilibrador de carga. Agregue los nodos de clúster asociado.
   - Cree el sondeo de estado: puerto 59999
   - Crear regla de equilibrio de carga: Permitir que los puertos de alta disponibilidad, con IP flotante habilitada. 
   
12. En cada nodo del clúster, abra el puerto 59999 (sondeo de estado). 
   
    Ejecute el siguiente comando en cada nodo:
```PowerShell
netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
```   
13. Indicar al clúster para escuchar mensajes de sondeo de estado en el puerto 59999 y responder desde el nodo que posee actualmente este recurso. Ejecútelo una vez desde cualquier nodo de clúster, para cada clúster. 
    
    En nuestro ejemplo, asegúrese de cambiar el "ILBIP" según los valores de configuración. Ejecute el siguiente comando desde cualquier uno nodo **az2az1**/**az2az2**:

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}
    ```

14. Ejecute el siguiente comando desde cualquier uno nodo **az2az3**/**az2az4**. 

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```   
   Asegúrese de que ambos clústeres pueden conectarse o comunicarse entre sí. 

   O bien, usar la característica de "Conectarse al clúster" en el Administrador de clústeres de conmutación por error para conectarse al otro clúster o comprobar que responde otro clúster de uno de los nodos del clúster actual.  
   
   ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
   ```
   ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
   ```   

15. Cree a los testigos de la nube para ambos clústeres. Cree dos [cuentas de almacenamiento](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**, **az2azcw2**) en azure es para cada clúster en el mismo grupo de recursos (**SR-AZ2AZ**).

    - Copiar el nombre de la cuenta de almacenamiento y la clave de "claves de acceso"
    - Cree al testigo en la nube de "Administrador de clústeres de conmutación por error" y use el nombre de cuenta y la clave anterior para crearlo.

16. Ejecute [las pruebas de validación de clúster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) antes de continuar con el paso siguiente.

17. Inicie Windows PowerShell y use el cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) para determinar si satisface todos los requisitos de la Réplica de almacenamiento. Puede usar el cmdlet en un modo de solo requisitos para una prueba rápida, así como un modo de evaluación del rendimiento de larga ejecución.

18. Configurar réplica de almacenamiento de clúster a clúster.
   
   Conceder acceso de un clúster a otro clúster en ambas direcciones:

   En nuestro ejemplo:

   ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
   ```
Si usa Windows Server 2016, a continuación, también se ejecute este comando:

   ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
   ```   
   
19. Crear SRPartnership para los clústeres:</ol>

 - Para clúster **SRAZC1**.
   - Ubicación del volumen:-c:\ClusterStorage\DataDisk1
   - Ubicación del registro:-g:
 - Para clúster **SRAZC2**
    - Ubicación del volumen:-c:\ClusterStorage\DataDisk2
    - Ubicación del registro:-g:

Ejecute el siguiente comando:

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```