---
title: Configuración de NIC convergente con un solo adaptador de red
description: En este tema, se proporcionan las instrucciones para configurar la NIC convergente con una sola NIC en el host de Hyper-V.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: 2ad7592fd9faf1e92893e6271daabdad907d3aaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405792"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configuración de NIC convergente con un solo adaptador de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, se proporcionan las instrucciones para configurar la NIC convergente con una sola NIC en el host de Hyper-V.

La configuración de ejemplo de este tema describe dos hosts de Hyper-v, el **host de Hyper-v a**y el **host de Hyper-v B**. Ambos hosts tienen instalada una única NIC física (pNIC) y las NIC están conectadas a una parte superior del bastidor \(ToR @ no__t-1. Además, los hosts se encuentran en la misma subred, que es 192.168.1. x/24.

![Hosts de Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Paso 1. Probar la conectividad entre el origen y el destino

Asegúrese de que la NIC física pueda conectarse al host de destino. Esta prueba muestra la conectividad mediante el uso de la capa 3 \(L3 @ no__t-1-o el nivel de IP, así como el nivel 2 \(L2 @ no__t-3.

1. Ver las propiedades del adaptador de red.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Resultados**_  


   | Nombre |    InterfaceDescription     | ifIndex | Estado |    Mac     | LinkSpeed |
   |------|-----------------------------|---------|--------|-------------------|-----------|
   |  CONECTOR  | Mellanox ConnectX-3 Pro... |    4    |   Arriba   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

2. Vea las propiedades adicionales del adaptador, incluida la dirección IP.

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**Resultados**_

   ```PowerShell   
    MacAddress   : 7C-FE-90-93-8F-A1
    Status   : Up
    LinkSpeed: 40 Gbps
    MediaType: 802.3
    PhysicalMediaType: 802.3
    AdminStatus  : Up
    MediaConnectionState : Connected
    DriverInformation: Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60
    DriverFileName   : mlx4eth63.sys
    NdisVersion  : 6.60
    ifOperStatus : Up
    ifAlias  : M1
    InterfaceAlias   : M1
    ifIndex  : 4
    ifDesc   : Mellanox ConnectX-3 Pro Ethernet Adapter
    ifName   : ethernet_32773
    DriverVersion: 5.25.12665.0
    LinkLayerAddress : 7C-FE-90-93-8F-A1
    Caption  :
    Description  :
    ElementName  :
    InstanceID   : {39B58B4C-8833-4ED2-A2FD-E105E7146D43}
    CommunicationStatus  :
    DetailedStatus   :
    HealthState  :
    InstallDate  :
    Name : M1
    OperatingStatus  :
    OperationalStatus:
    PrimaryStatus:
    StatusDescriptions   :
    AvailableRequestedStates :
    EnabledDefault   : 2
    EnabledState : 5
    OtherEnabledState:
    RequestedState   : 12
    TimeOfLastStateChange:
    TransitioningToState : 12
    AdditionalAvailability   :
    Availability :
    CreationClassName: MSFT_NetAdapter
   ``` 

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Paso 2. Asegurarse de que el origen y el destino pueden comunicarse

En este paso, usamos el comando **Test-NetConnection de** Windows PowerShell, pero si lo prefiere, puede usar el comando **ping** . 

>[!TIP]
>Si está seguro de que los hosts pueden comunicarse entre sí, puede omitir este paso.

1. Compruebe la comunicación bidireccional.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados**_


   |        Parámetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      |     CONECTOR      |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    True     |
   | RTT \(PingReplyDetails\) |    0 milisegundos     |

   ---

   En algunos casos, es posible que tenga que deshabilitar Firewall de Windows con seguridad avanzada para realizar correctamente esta prueba. Si deshabilita el firewall, tenga en cuenta la seguridad y asegúrese de que la configuración cumple los requisitos de seguridad de su organización.

2. Deshabilitar todos los perfiles de Firewall.

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. Después de deshabilitar los perfiles de firewall, vuelva a probar la conexión. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados**_


   |        Parámetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | RTT \(PingReplyDetails\) |    0 milisegundos     |

   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Paso 3. Opta Configurar los identificadores de VLAN para las NIC instaladas en los hosts de Hyper-V

Muchas configuraciones de red hacen uso de redes VLAN y, si tiene previsto usar VLAN en la red, debe repetir la prueba anterior con VLAN configuradas. Además, si tiene previsto usar RoCE para los servicios RDMA, debe habilitar las VLAN.

En este paso, las NIC están en modo de **acceso** . Sin embargo, cuando se crea un vSwitch \(\) de conmutador virtual de Hyper-V más adelante en esta guía, las propiedades de la VLAN se aplican en el nivel de Puerto vSwitch. 

Dado que un conmutador puede hospedar varias VLAN, es necesario para la parte superior del \(conmutador\) físico del bastidor Tor para que el puerto al que está conectado el host esté configurado en el modo de tronco.

>[!NOTE]
>Consulte la documentación del conmutador ToR para obtener instrucciones sobre cómo configurar el modo de tronco en el conmutador.

En la imagen siguiente se muestran dos hosts de Hyper-V, cada uno con un adaptador de red físico y cada uno configurado para comunicarse en la VLAN 101.

![Configuración de redes de área local virtuales](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>Realice esto en los servidores locales y de destino. Si el servidor de destino no está configurado con el mismo identificador de VLAN que el servidor local, los dos no se pueden comunicar.


1. Configure el identificador de VLAN para las NIC instaladas en los hosts de Hyper-V.

   >[!IMPORTANT]
   >No ejecute este comando si está conectado al host de forma remota a través de esta interfaz, ya que esto provoca la pérdida de acceso al host.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**Resultados**_


   | Nombre | DisplayName | DisplayValue | RegistryKeyword | Deleteinstance |
   |------|-------------|--------------|-----------------|---------------|
   |  CONECTOR  |   ID. DE VLAN   |     101      |     VlanID      |     {101}     |

   ---

2. Reinicie el adaptador de red para aplicar el ID. de VLAN.

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. Asegúrese de que el estado es **activo**.

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```

   _**Resultados**_


   | Nombre |          InterfaceDescription           | ifIndex | Estado |    Mac     | LinkSpeed |
   |------|-----------------------------------------|---------|--------|-------------------|-----------|
   |  CONECTOR  | Adaptador Mellanox ConnectX-3 Pro Ethernet ADA... |    4    |   Arriba   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

   >[!IMPORTANT]
   >El dispositivo puede tardar varios segundos en reiniciarse y estar disponible en la red. 

4. Compruebe la conectividad.<p>Si se produce un error de conectividad, revise la configuración de VLAN del conmutador o la participación de destino en la misma VLAN. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

## <a name="step-4-configure-quality-of-service-qos"></a>Paso 4. Configurar QoS de calidad \(de servicio\)

>[!NOTE]
>Debe realizar todos los siguientes pasos de configuración de DCB y QoS en todos los hosts que estén diseñados para comunicarse entre sí.

1. Instale el DCB\) de \(puente del centro de datos en cada uno de los hosts de Hyper-V.

   - **Opcional** para las configuraciones de red que usan iWarp para los servicios RDMA.
   - Se **requiere** para las configuraciones de red \(que usan\) RoCE cualquier versión para los servicios RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. Establezca las directivas de QoS para SMB-Direct:

   - **Opcional** para las configuraciones de red que usan iWarp.
   - Se **requiere** para las configuraciones de red que usan RoCE.

   En el siguiente comando de ejemplo, el valor "3" es arbitrario. Puede usar cualquier valor entre 1 y 7 siempre que use el mismo valor de forma coherente en toda la configuración de las directivas de QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**Resultados**_


   |   Parámetro    |          Valor           |
   |----------------|--------------------------|
   |      Nombre      |           SMB            |
   |     Propietario      | Máquina \(Directiva de grupo\) |
   | NetworkProfile |           Todos            |
   |   Precedencia   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. En el caso de las implementaciones de RoCE, active el **control de flujo de prioridad** para el tráfico SMB, que no es necesario para iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**Resultados**_


   | Prioridad | Enabled | PolicySet | ifIndex | IfAlias |
   |----------|---------|-----------|---------|---------|
   |    0     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    1     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    2     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    3     |  True   |  Global   | &nbsp;  | &nbsp;  |
   |    4     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    5     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    6     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    7     |  False  |  Global   | &nbsp;  | &nbsp;  |

   ---

4. Habilitar QoS para los adaptadores de red locales y de destino.

   - **No es necesario** para las configuraciones de red que usan iWarp.
   - Se **requiere** para las configuraciones de red que usan RoCE.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**Resultados**_

   **Nombre**: CONECTOR  
   **Habilitado**: True  

   _**Capabilities**_   


   |      Parámetro      |   Hardware   |   Corrientes    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Ninguno     |     Ninguno     |
   | NumTCs (Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses:**_ 


   | API | TSA | consumo | Fija |
   |----|-----|-----------|------------|
   | 0  | ETS |    70%    |  0:2, 4-7   |
   | 1  | ETS |    30 %    |     3      |

   ---

   _**OperationalFlowControl:**_  

   Prioridad 3 habilitada  

   _**OperationalClassifications:**_  


   | Protocol  | Puerto/tipo | Prioridad |
   |-----------|-----------|----------|
   |  Default  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

5. Reserve un porcentaje del ancho de banda para SMB directo \(RDMA @ no__t-1.

    En este ejemplo, se usa una reserva de ancho de banda del 30%. Debe seleccionar un valor que represente lo que espera que requiera el tráfico de almacenamiento. 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**Resultados**_


   | Nombre | Algoritmo | Ancho de banda (%) | Prioridad | PolicySet | ifIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      30      |    3     |  Global   | &nbsp;  | &nbsp;  |

   ---                                      

6. Vea la configuración de reserva de ancho de banda.  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**Resultados**_


   |   Nombre    | Algoritmo | Ancho de banda (%) | Prioridad | PolicySet | ifIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | Predeterminada |    ETS    |      70      | 0:2, 4-7  |  Global   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      30      |    3     |  Global   | &nbsp;  | &nbsp;  |

   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>Paso 5. Opta Resolver el conflicto del depurador del adaptador de Mellanox 

De forma predeterminada, cuando se usa un adaptador de Mellanox, el depurador adjunto bloquea NetQos, que es un problema conocido. Por lo tanto, si usa un adaptador de Mellanox y desea adjuntar un depurador, use el siguiente comando para resolver este problema. Este paso no es necesario si no desea adjuntar un depurador o si no está usando un adaptador Mellanox.

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>Paso 6. Comprobar la configuración de RDMA (host nativo)

Desea asegurarse de que el tejido esté configurado correctamente antes de crear un vSwitch y de pasar a RDMA (NIC convergente). 

En la imagen siguiente se muestra el estado actual de los hosts de Hyper-V.

![Probar RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. Compruebe la configuración de RDMA.

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**Resultados**_


   | Nombre |           InterfaceDescription           | Enabled |
   |------|------------------------------------------|---------|
   |  CONECTOR  | Adaptador Ethernet ConnectX-3 Pro de Mellanox |  True   |

   ---

2. Determine **el valor** de los valores de la de su adaptador de destino.<p>Este valor se usa en los pasos siguientes cuando se ejecuta el script que se descarga.

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**Resultados**_ 


   | InterfaceAlias | InterfaceIndex |  Dirección IPv4  |
   |----------------|----------------|---------------|
   |       ²       |       14       | 192.168.1.5 |

   ---

3. Descargue la [utilidad DiskSpd. exe](https://aka.ms/diskspd) y extráigalo en C:\test\.

4. Descargue el [script de PowerShell test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) en una carpeta de prueba de la unidad local, por ejemplo, C:\test @ no__t-1.

5. Ejecute el script de PowerShell **Test-RDMA. PS1** para pasar el valor de índice a la secuencia de comandos, junto con la dirección IP del adaptador remoto en la misma VLAN.<p>En este ejemplo **, el script pasa el valor de** índice de 14 en la dirección IP del adaptador de red remoto 192.168.1.5.

   ```PowerShell
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\

    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter M2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 662979201 RDMA bytes written per second
    VERBOSE: 37561021 RDMA bytes sent per second
    VERBOSE: 1023098948 RDMA bytes written per second
    VERBOSE: 8901349 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
   ```

   >[!NOTE]
   >Si se produce un error en el tráfico RDMA, en el caso de RoCE, consulte la configuración del conmutador ToR para obtener una configuración correcta de PFC/ETS que debe coincidir con la configuración del host. Consulte la sección QoS de este documento para obtener los valores de referencia.

## <a name="step-7-remove-the-access-vlan-setting"></a>Paso 7. Quitar la configuración de VLAN de acceso

Como preparación para crear el conmutador de Hyper-V, debe quitar la configuración de VLAN que instaló anteriormente.  

1. Quite la configuración de acceso de VLAN de la NIC física para impedir que la NIC etiquete automáticamente el tráfico de salida con el identificador de VLAN incorrecto.<p>Al quitar esta opción, también se impide que filtre el tráfico de entrada que no coincide con el identificador de VLAN de acceso.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. Confirme que la **opción VlanID** muestra el valor de identificador de VLAN como cero.

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Paso 8. Creación de un vSwitch de Hyper-V en los hosts de Hyper-V

En la imagen siguiente se muestra el host 1 de Hyper-V con un vSwitch.

![Crear un conmutador virtual de Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. Cree un vSwitch externo de Hyper-V en Hyper-v en el host de Hyper-V A. <p>En este ejemplo, el modificador se denomina VMSTEST. Además, el parámetro **AllowManagementOS** crea un host VNIC que hereda las direcciones MAC y IP de la NIC física.

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**Resultados**_


   |  Nombre   | Tipodeconmutador |      Si      |
   |---------|------------|------------------------------------------|
   | VMSTEST |  External  | Adaptador Ethernet ConnectX-3 Pro de Mellanox |

   ---

2. Ver las propiedades del adaptador de red.

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**Resultados**_


   |         Nombre          |        InterfaceDescription         | ifIndex | Estado |    Mac     | LinkSpeed |
   |-----------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet \(VMSTEST @ no__t-1 | Adaptador Ethernet Virtual de Hyper-V #2 |   27    |   Arriba   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---

3. Administre el host vNIC de una de estas dos maneras. 

   - La vista **NetAdapter** funciona según el nombre "vEthernet \(VMSTEST @ no__t-2". No todas las propiedades del adaptador de red se muestran en esta vista.
   - La vista **VMNetworkAdapter** quita el prefijo "vEthernet" y simplemente usa el nombre de vmswitch. (recomendado) 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**Resultados**_


   |         Nombre         | IsManagementOs |        VMName        |  SwitchName  | Mac | Estado | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-external-switch |      True      | CORP-external-switch | 001B785768AA |    Aceptar    | &nbsp; |             |
   |       VMSTEST        |      True      |       VMSTEST        | E41D2D074071 |    Aceptar    | &nbsp; |             |

   ---

4. Pruebe la conexión.

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados**_ 

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

5. Asignar y ver la configuración de VLAN del adaptador de red.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```    

   _**Resultados**_


   | VMName | VMNetworkAdapterName |  Modo  | VlanList |
   |--------|----------------------|--------|----------|
   | &nbsp; |       VMSTEST        | Acceso |   101    |

   ---  

6. Pruebe la conexión.<p>Puede tardar unos segundos en completarse antes de que pueda hacer ping correctamente en el otro adaptador.  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados**_

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>Paso 9. Probar el conmutador virtual de Hyper-V RDMA (modo 2)

En la imagen siguiente se muestra el estado actual de los hosts de Hyper-V, incluido el vSwitch en el host 1 de Hyper-V.

![Probar el conmutador virtual de Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. Establezca el etiquetado de prioridades en el host vNIC.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  

   _**Resultados**_

    Nombre: VMSTEST IeeePriorityTag : Activado


2. Ver la información de RDMA del adaptador de red. 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**Resultados**_


   |         Nombre          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST @ no__t-1 | Adaptador Ethernet Virtual de Hyper-V #2 |  False  |

   ---

   >[!NOTE]
   >Si el parámetro **habilitado** tiene el valor **false**, significa que RDMA no está habilitado.


3. Ver la información del adaptador de red.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Resultados**_   


   |        Nombre         |        InterfaceDescription         | ifIndex | Estado |    Mac     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | Adaptador Ethernet Virtual de Hyper-V #2 |   27    |   Arriba   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---


4. Habilite RDMA en el host vNIC.

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**Resultados**_


   |         Nombre          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST @ no__t-1 | Adaptador Ethernet Virtual de Hyper-V #2 |  True   |

   ---

   >[!NOTE]
   >Si el parámetro **habilitado** tiene el valor **true**, significa que RDMA está habilitado.

5. Realice una prueba de tráfico RDMA.

   ```PowerShell    
    C:\TEST\Test-RDMA.PS1 -IfIndex 27 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (VMSTEST) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 9162492 RDMA bytes sent per second
    VERBOSE: 938797258 RDMA bytes written per second
    VERBOSE: 34621865 RDMA bytes sent per second
    VERBOSE: 933572610 RDMA bytes written per second
    VERBOSE: 35035861 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
   ```

La última línea de esta salida, "prueba de tráfico RDMA correcta: El tráfico RDMA se envió a 192.168.1.5, "muestra que ha configurado correctamente la NIC convergente en el adaptador.

## <a name="related-topics"></a>Temas relacionados
- [Configuración de NIC en equipo NIC convergente](cnic-datacenter.md)
- [Configuración del conmutador físico para la NIC convergente](cnic-app-switch-config.md)
- [Solución de problemas de configuraciones de NIC convergentes](cnic-app-troubleshoot.md)
