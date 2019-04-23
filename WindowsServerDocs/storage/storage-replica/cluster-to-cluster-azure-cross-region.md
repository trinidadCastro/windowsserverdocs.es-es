---
title: Intercambio entre regiones de la Réplica de almacenamiento de clúster a clúster en Azure
description: Replicación de almacenamiento de clúster a clúster entre la región de Azure
keywords: Réplica de almacenamiento, el administrador del servidor, Windows Server, Azure, clúster entre regiones, una región diferente
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 41f435c3d537cbfd204dfa869d750b22200deb33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891136"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Intercambio entre regiones de la Réplica de almacenamiento de clúster a clúster en Azure
Puede configurar las réplicas de almacenamiento de clúster a clúster para las aplicaciones entre regiones de Azure. En los ejemplos siguientes, usamos un clúster de dos nodos, pero la réplica de almacenamiento de clúster a clúster no está restringido a un clúster de dos nodos. La ilustración siguiente es un clúster de espacio de almacenamiento directo de dos nodos que puede comunicarse entre sí, se encuentran en el mismo dominio y entre regiones.

Vea el siguiente vídeo para obtener un tutorial completo del proceso.
> [!video https://www.microsoft.com/en-us/videoplayer/embed/RE26xeW]

![Que muestra SR C2C en Azure dentro de diagrama de arquitectura de la misma región.](media\Cluster-to-cluster-azure-cross-region\architecture.png)
> [!IMPORTANT]
> Todos los ejemplos que se hace referencia son específicos de la ilustración anterior.


1. En el portal de Azure, cree [grupos de recursos](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) en dos regiones diferentes.

    Por ejemplo, **SR-AZ2AZ** en **oeste de EE.UU. 2** y **SR-AZCROSS** en **centro occidental de Ee.uu.**, como se indicó anteriormente.

2. Cree dos [conjuntos de disponibilidad](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM), uno en cada grupo de recursos para cada clúster.
    - Conjunto de disponibilidad (**az2azAS1**) en (**SR-AZ2AZ**)
    - Conjunto de disponibilidad (**azcross-AS**) en (**SR-AZCROSS**)

3. Crear dos redes virtuales
   - Crear el [red virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az Vnet**) en el primer grupo de recursos (**SR-AZ2AZ**), que tiene una subred y una subred de puerta de enlace.
   - Crear el [red virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**azcross VNET**) en el segundo grupo de recursos (**SR-AZCROSS**), que tiene una subred y una subred de puerta de enlace.

4. Cree dos grupos de seguridad de red
   - Crear el [grupo de seguridad de red](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az NSG**) en el primer grupo de recursos (**SR-AZ2AZ**).
   - Crear el [grupo de seguridad de red](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**azcross NSG**) en el segundo grupo de recursos (**SR-AZCROSS**). 

   Agregar una regla de seguridad de entrada para RDP:3389 a ambos grupos de seguridad de red. Puede quitar esta regla después de finalizar el programa de instalación.

5. Creación de Windows Server [máquinas virtuales](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) en los grupos de recursos creado anteriormente.

   Controlador de dominio (**az2azDC**). Puede crear un conjunto de disponibilidad 3rd para el controlador de dominio o agregar el controlador de dominio en uno de los conjuntos de disponibilidad de dos. Si va a agregar al conjunto de disponibilidad creado para los dos clústeres, asignar, una dirección IP pública estándar durante la creación de máquinas virtuales.
      - Instalar servicio de dominio de Active Directory.
      - Crear un dominio (contoso.com)
      - Crear un usuario con privilegios de administrador (contosoadmin)

   Cree dos máquinas virtuales (**az2az1**, **az2az2**) en el grupo de recursos (**SR-AZ2AZ**) mediante la red virtual (**az2az Vnet**) y grupo de seguridad de red (**az2az NSG**) en conjunto de disponibilidad (**az2azAS1**). Asignar una dirección IP pública estándar para cada máquina virtual durante la creación propia.
      - Agregar al menos dos discos administrados a cada máquina
      - Instalar la característica de réplica de almacenamiento y clústeres de conmutación por error

   Cree dos máquinas virtuales (**azcross1**, **azcross2**) en el grupo de recursos (**SR-AZCROSS**) mediante la red virtual (**VNET azcross**) y el grupo de seguridad de red (**azcross NSG**) en conjunto de disponibilidad (**azcross-AS**). Asignar dirección IP pública estándar para cada máquina virtual durante la creación propia
      - Agregar al menos dos discos administrados a cada máquina
      - Instalar la característica de réplica de almacenamiento y clústeres de conmutación por error

   Conectar todos los nodos al dominio y proporcionar los privilegios de administrador para el usuario creado anteriormente.

   Cambie el servidor DNS de la red virtual a la dirección IP privada de controlador de dominio.
   - En el ejemplo, el controlador de dominio **az2azDC** tiene la dirección IP privada (10.3.0.8). En la red Virtual (**az2az Vnet** y **azcross VNET**) cambiar el servidor DNS 10.3.0.8. 

    En el ejemplo, conecte todos los nodos para "contoso.com" y proporcionar los privilegios de administrador para "contosoadmin".
   - Inicie sesión como contosoadmin de todos los nodos. 
 
6. Crear los clústeres (**SRAZC1**, **SRAZCross**).

   A continuación encontrará los comandos de PowerShell para el ejemplo
   ```powershell
      New-Cluster -Name SRAZC1 -Node az2az1,az2az2 – StaticAddress 10.3.0.100
   ```
   ```powershell
      New-Cluster -Name SRAZCross -Node azcross1,azcross2 – StaticAddress 10.0.0.10
   ```

7. Habilitar espacios de almacenamiento directo.

   ```powershell
      Enable-clusterS2D
   ```

   > [!NOTE]
   > Para cada clúster crear volúmenes y discos virtuales. Uno de los datos y otro para el registro.

8. Creación de una SKU estándar interno [equilibrador de carga](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) para cada clúster (**azlbr1**, **azlbazcross**).

   Proporcionar la dirección IP del clúster como dirección IP privada estática para el equilibrador de carga.
      - azlbr1 => Frontend IP: 10.3.0.100 (seleccionar una dirección IP no utilizada de la red Virtual (**az2az Vnet**) subred)
      - Crear grupo de back-end para cada equilibrador de carga. Agregue los nodos de clúster asociado.
      - Cree el sondeo de estado: puerto 59999
      - Crear regla de equilibrio de carga: Permitir que los puertos de alta disponibilidad, con IP flotante habilitada.

   Proporcionar la dirección IP del clúster como dirección IP privada estática para el equilibrador de carga. 
      - azlbazcross = > Frontend IP: 10.0.0.10 (seleccionar una dirección IP no utilizada de la red Virtual (**azcross VNET**) subred)
      - Crear grupo de back-end para cada equilibrador de carga. Agregue los nodos de clúster asociado.
      - Cree el sondeo de estado: puerto 59999
      - Crear regla de equilibrio de carga: Permitir que los puertos de alta disponibilidad, con IP flotante habilitada. 

9. Crear [puerta de enlace de red Virtual](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM) para la conectividad de red virtual a red virtual.

 - Crear la primera puerta de enlace de red virtual (**az2az VNetGateway**) en el primer grupo de recursos (**SR-AZ2AZ**)
 - Tipo de puerta de enlace = VPN y el tipo de VPN = basada en rutas

 - Cree la segunda puerta de enlace de red Virtual (**azcross VNetGateway**) en el segundo grupo de recursos (**SR-AZCROSS**)
 - Tipo de puerta de enlace = VPN y el tipo de VPN = basada en rutas

 - Crear una conexión de red virtual a red virtual desde la primera puerta de enlace de red Virtual a la segunda puerta de enlace de red Virtual. Proporcione una clave compartida

 - Crear una conexión de red virtual a red virtual de segunda puerta de enlace de red Virtual a la primera puerta de enlace de red Virtual. Proporcione la misma clave compartida como se indica en el paso anterior. 

10. En cada nodo del clúster, abra el puerto 59999 (sondeo de estado).

   Ejecute el siguiente comando en cada nodo:

   ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
   ```

11. Indicar al clúster para escuchar mensajes de sondeo de estado en el puerto 59999 y responder desde el nodo que posee actualmente este recurso.

   Ejecútelo una vez desde cualquier nodo de clúster, para cada clúster. 
    
   En nuestro ejemplo, asegúrese de cambiar el "ILBIP" según los valores de configuración. Ejecute el siguiente comando desde cualquier uno nodo **az2az1**/**az2az2**

   ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
   ```

12. Ejecute el siguiente comando desde cualquier uno nodo **azcross1**/**azcross2**
   ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
   ```

   Asegúrese de que ambos clústeres pueden conectarse o comunicarse entre sí.

   O bien, usar la característica de "Conectarse al clúster" en el Administrador de clústeres de conmutación por error para conectarse al otro clúster o comprobar que responde otro clúster de uno de los nodos del clúster actual.

   En el ejemplo que hemos estado utilizando:
   ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
   ```
   ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
   ```

13. Cree el testigo en la nube para ambos clústeres. Cree dos [cuentas de almacenamiento](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**,**azcrosssa**) en Azure, uno para cada clúster en cada grupo de recursos (**SR-AZ2AZ**,  **SR-AZCROSS**).
   
   - Copiar el nombre de la cuenta de almacenamiento y la clave de "claves de acceso"
   - Cree al testigo en la nube de "Administrador de clústeres de conmutación por error" y use el nombre de cuenta y la clave anterior para crearlo. 

14. Ejecute [las pruebas de validación de clúster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) antes de continuar con el paso siguiente

15. Inicie Windows PowerShell y use el cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) para determinar si satisface todos los requisitos de la Réplica de almacenamiento. Puede usar el cmdlet en modo de solo requisitos para una prueba rápida, o en modo de evaluación de rendimiento de ejecución más larga.
 
16. Configurar réplica de almacenamiento de clúster a clúster.
Conceder acceso de un clúster a otro clúster en ambas direcciones:

   Ejemplo:
   ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
   ```
Si usa Windows Server 2016, a continuación, también se ejecute este comando:

   ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
   ```

17. Crear asociación de SR para los dos clústeres:</ol>

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