---
title: Configuración de NIC convergente con un único adaptador de red
description: En este tema, se proporcionan las instrucciones para configurar estos convergen de NIC con una sola NIC en el host de Hyper-V.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: 7777278f374984f242e44fd8a8fa94388df88a30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836476"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configuración de NIC convergente con un único adaptador de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, se proporcionan las instrucciones para configurar estos convergen de NIC con una sola NIC en el host de Hyper-V.

La configuración de ejemplo en este tema describe dos hosts de Hyper-V, **un Host de Hyper-V**, y **B de Host de Hyper-V**. Ambos hosts tienen un NIC físico único (pNIC) instalado y está conectada la NIC a una parte superior del rack \(ToR\) conmutador físico. Además, los hosts se encuentran en la misma subred, que es 192.168.1.x/24.

![Hosts de Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Paso 1. Probar la conectividad entre el origen y destino

Asegúrese de que la física NIC puede conectarse al host de destino. Esta prueba muestra la conectividad mediante el uso de nivel 3 \(L3\) - o el nivel de IP -, así como de nivel 2 \(L2\).

1. Ver las propiedades de adaptador de red.

   ```PowerShell
   Get-NetAdapter
   ```
   
   _**Resultados:**_  

   |Nombre|InterfaceDescription|ifIndex|Estado|MacAddress|LinkSpeed|
   |-----|--------------------|-------|-----|----------|---------|
   |M1|Mellanox ConnectX-3 Pro ...| 4| Arriba|7C-FE-90-93-8F-A1|40 Gbps|
   ---

2. Ver las propiedades de adaptador adicional, incluida la dirección IP.

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**Resultados:**_

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

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Paso 2. Asegúrese de que pueden comunicarse origen y destino

En este paso, usamos el **Test-NetConnection** comando de Windows PowerShell, pero si usas el **ping** comando si lo prefiere. 

>[!TIP]
>Si está seguro de que los hosts puedan comunicarse entre sí, puede omitir este paso.

1. Compruebe la comunicación bidireccional.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```
   
   _**Resultados:**_

   |Parámetro|Valor|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|M1|
   |SourceAddress|192.168.1.3|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 milisegundos|
   ---
   
   En algunos casos, es posible que deba deshabilitar Firewall de Windows con seguridad avanzada para realizar correctamente esta prueba. Si deshabilita el firewall, mantienen la seguridad en mente y asegúrese de que la configuración cumple los requisitos de seguridad de su organización.

2. Deshabilitar todos los perfiles de firewall.

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```
    
3. Después de deshabilitar los perfiles de firewall, pruebe de nuevo la conexión. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados:**_

   |Parámetro|Valor|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|Prueba 1 de 40G|
   |SourceAddress|192.168.1.3|
   |PingSucceeded|False|
   |PingReplyDetails \(RTT\)|0 milisegundos|
   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Paso 3. (Opcional) Configurar los identificadores de VLAN para las NIC que se instala en los hosts de Hyper-V

Muchas configuraciones de red hacen uso de las VLAN y, si va a usar las VLAN en la red, debe repetir la prueba anterior con VLAN configuradas. Además, si planea usar RoCE para servicios RDMA debe habilitar las VLAN.

Este paso, las NIC están en **acceso** modo. Sin embargo, cuando creas un conmutador Virtual de Hyper-V \(vSwitch\) más adelante en esta guía, se aplican las propiedades VLAN en el nivel de puerto vSwitch. 

Dado que un conmutador puede hospedar varias VLAN, es necesario para la parte superior del Rack \(ToR\) conmutador físico que el puerto que está conectado el host configurado en modo de tronco.

>[!NOTE]
>Para obtener instrucciones sobre cómo configurar el modo de tronco en el conmutador, consulte la documentación del conmutador ToR.

La siguiente imagen muestra dos hosts de Hyper-V, cada uno con un adaptador de red físico, y cada uno configurado para comunicarse en VLAN 101.

![Configurar redes de área local virtuales](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>Realizar esto en servidores locales y de destino. Si el servidor de destino no está configurado con el mismo identificador de VLAN que el servidor local, no se pueden comunicar los dos.


1. Para las NIC que se instala en los hosts de Hyper-V, configure el identificador de VLAN.

   >[!IMPORTANT]
   >No ejecute este comando si está conectado al host de forma remota a través de esta interfaz, porque esto da como resultado la pérdida de acceso al host.
    
   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**Resultados:**_

   |Name |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |M1|ID. DE VLAN|101|VlanID|{101}|
   ---

2. Reiniciar para aplicar el ID. VLAN el adaptador de red

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. Asegúrese de que el estado es **seguridad**.

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```
   
   _**Resultados:**_

   |Nombre|InterfaceDescription|ifIndex| Estado|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |M1|Mellanox ConnectX-3 Pro Ethernet Ada...|4|Arriba|7C-FE-90-93-8F-A1|40 Gbps|
   ---

   >[!IMPORTANT]
   >Podría tardar varios segundos para el dispositivo para reiniciar y están disponibles en la red. 

4. Compruebe la conectividad.<p>Si se produce un error de conectividad, inspeccione el conmutador de participación de configuración o el destino de VLAN en la misma VLAN. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```
    
## <a name="step-4-configure-quality-of-service-qos"></a>Paso 4. Configuración de calidad de servicio \(QoS\)

>[!NOTE]
>Debe realizar todos estos pasos de configuración DCB y QoS en todos los hosts que están diseñados para comunicarse entre sí.

1. Instalar el protocolo de puente del centro de datos \(DCB\) en cada uno de los hosts de Hyper-V.

   - **Opcional** para configuraciones de red que usan iWarp para servicios RDMA.
   - **Requiere** configuraciones de red que usan RoCE \(cualquier versión\) para servicios RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. Establecer las directivas de QoS para SMB-Direct:

   - **Opcional** para configuraciones de red que usan iWarp.
   - **Requiere** para configuraciones de red que usan RoCE.
   
   En el siguiente comando de ejemplo, el valor "3" es arbitrario. Puede usar cualquier valor entre 1 y 7 siempre y cuando se usa siempre el mismo valor en toda la configuración de directivas de QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**Resultados:**_

   |Parámetro|Valor|
   |---------|-----|
   |Nombre |SMB|
   |Propietario|Directiva de grupo \(máquina\)|
   |NetworkProfile|Todos|
   |Precedencia|127|
   |JobObject|&nbsp;| 
   |NetDirectPort|445 |
   |PriorityValue|3 |
 ---

3. Para las implementaciones de RoCE, activar **Control de flujo de prioridad** para el tráfico SMB, que no es necesario para iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**Resultados:**_

   |Prioridad|Enabled|PolicySet|IfIndex|IfAlias|
   |---------|-----|--------- |-------| -------|
   |0 |False |Global|&nbsp;|&nbsp;|
   |1 |False |Global|&nbsp;|&nbsp;|
   |2 |False |Global|&nbsp;|&nbsp;|
   |3 |True  |Global|&nbsp;|&nbsp;|
   |4 |False |Global|&nbsp;|&nbsp;|
   |5 |False |Global|&nbsp;|&nbsp;|
   |6 |False |Global|&nbsp;|&nbsp;|
   |7 |False |Global|&nbsp;|&nbsp;|
   ---

4. Habilitar la calidad de servicio para los adaptadores de red local y de destino.

   - **No es necesario** para configuraciones de red que usan iWarp.
   - **Requiere** para configuraciones de red que usan RoCE.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**Resultados:**_

   **Nombre**: M1  
   **Habilitado**: True  

   _**Funcionalidades:**_   

   |Parámetro|Hardware|Actual|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|Ninguno|Ninguno|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---
 
   _**OperationalTrafficClasses:**_ 

   |TC|TSA|Ancho de banda|Prioridades|
   |----|-----|--------|-------|
   |0| ETS|70%|0-2,4-7|
   |1|ETS|30 %|3 |
   ---

   _**OperationalFlowControl:**_  
   
   Prioridad 3 habilitado  

   _**OperationalClassifications:**_  

   |Protocolo|Tipo de puerto /|Prioridad|
   |--------|---------|--------|
   |Default|&nbsp;|0|
   |NetDirect| 445|3|
   ---

5. Reservar un porcentaje del ancho de banda de SMB directo \(RDMA\).

    En este ejemplo, se utiliza una reserva de ancho de banda de 30%. Debe seleccionar un valor que representa lo que espera que requiere que el tráfico de almacenamiento. 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**Resultados:**_

   |Name|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |SMB | ETS     | 30 |3 |Global |&nbsp;|&nbsp;|
   ---                                      

6. Ver la configuración de reserva de ancho de banda.  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**Resultados:**_
 
   |Name|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |[Predeterminado]|ETS|70 |0-2,4-7| Global|&nbsp;|&nbsp;| 
   |SMB      |ETS|30 |3 |Global|&nbsp;|&nbsp;| 
   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>Paso 5. (Opcional) Resolver el conflicto de depurador adaptador Mellanox 

De forma predeterminada, cuando se usa un adaptador Mellanox, el depurador asociado bloquea NetQos, que es un problema conocido. Por lo tanto, si está usando un adaptador Mellanox y va a adjuntar a un depurador, use siguiente comando ha resuelto este problema. Este paso no es necesario si no desea asociar a un depurador o si no usa un adaptador Mellanox.

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>Paso 6. Compruebe la configuración de RDMA (host nativo)

Desea asegurarse de que el tejido está configurado correctamente antes de crear un vSwitch y realizar la transición a RDMA (convergente NIC). 

La siguiente imagen muestra el estado actual de los hosts de Hyper-V.

![Prueba RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. Compruebe la configuración de RDMA.

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**Resultados:**_

   |Nombre |InterfaceDescription |Enabled|
   |----|--------------------|-------|
   |M1| Mellanox ConnectX-3 Pro Ethernet Adapter |True|
   ---

2. Determinar la **ifIndex** valor de su adaptador de destino.<p>Use este valor en pasos posteriores al ejecutar el script que descargó.

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**Resultados:**_ 

   |InterfaceAlias |InterfaceIndex |Dirección IPv4|
   |-------------- |-------------- |-----------|
   |M2 |14 |{192.168.1.5}|
   ---

3. Descargue el [DiskSpd.exe utilidad](https://aka.ms/diskspd) y extráigalo en C:\TEST\.

4. Descargue el [script de powershell de prueba RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) en una carpeta de prueba en la unidad local, por ejemplo, C:\TEST\.

5. Ejecute el **prueba Rdma.ps1** script de PowerShell para pasar el valor de ifIndex a la secuencia de comandos, junto con la dirección IP del adaptador remoto en la misma VLAN.<p>En este ejemplo, el script pasa el **ifIndex** valor 14 en la dirección IP de adaptador de red remota 192.168.1.5.
   
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
   >Si falla el tráfico RDMA, para el caso de RoCE en concreto, consulte la configuración de ToR Switch para opciones PFC/ETS adecuadas que deben coincidir con la configuración de Host. Consulte la sección de QoS en este documento, los valores de referencia.

## <a name="step-7-remove-the-access-vlan-setting"></a>Paso 7. Quite la configuración de acceso de VLAN

En preparación para la creación de la función Hyper-V cambie, debe quitar la configuración de VLAN que se ha instalado anteriormente.  

1. Quitar la configuración de acceso de VLAN de la NIC física para evitar que la NIC de etiquetado automático el tráfico de salida con el identificador de VLAN incorrecta.<p>Quitar esta configuración también impide que filtran el tráfico de entrada que no coincide con el identificador de VLAN de acceso.
    
   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. Confirme que la **configuración VlanID** muestra el valor de Id. de VLAN como cero.

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Paso 8. Crear un vSwitch de Hyper-V en los hosts de Hyper-V

La siguiente imagen muestra 1 Host de Hyper-V con un vSwitch.

![Crear un conmutador Virtual de Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. Crear un vSwitch externo de Hyper-V en Hyper-V en el Host de Hyper-V A. <p>En este ejemplo, el conmutador se denomina VMSTEST. Además, el parámetro **AllowManagementOS** crea una vNIC de host que se hereda de las direcciones MAC e IP de la NIC física.

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**Resultados:**_

   |Nombre |SwitchType |NetAdapterInterfaceDescription|
   |---- |---------- |------------------------------|
   |VMSTEST |External |Mellanox ConnectX-3 Pro Ethernet Adapter|
   ---

2. Ver las propiedades de adaptador de red.

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**Resultados:**_

   |Name |InterfaceDescription | ifIndex |Estado |MacAddress |LinkSpeed|
   |---- |-------------------- |-------| ------|----------|---------|
   |vEthernet \(VMSTEST\) |Adaptador Ethernet virtuales Hyper-V #2|27 |Arriba |E4-1D-2D-07-40-71 |40 Gbps|
   ---

3. Administrar la vNIC de host en uno de dos maneras. 

   - **NetAdapter** vista funciona según el "vEthernet \(VMSTEST\)" nombre. No todas las propiedades del adaptador de red se muestran en esta vista.
   - **VMNetworkAdapter** vista quita el prefijo "vEthernet" y simplemente usa el nombre de vmswitch. (recomendado) 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**Resultados:**_

   |Nombre |IsManagementOs |VMName |SwitchName |MacAddress |Estado |IPAddresses|
   |----|-------------- |------ |----------|----------|------ |-----------|
   |CORP-External-Switch |True |CORP-External-Switch| 001B785768AA |{Ok} |&nbsp;|
   |VMSTEST |True |VMSTEST | E41D2D074071| {Ok} | &nbsp;| 
   ---

4. Probar la conexión.

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados:**_ 

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

   _**Resultados:**_

   |VMName |VMNetworkAdapterName |Modo |VlanList|
   |------ |-------------------- |---- |--------|
   |&nbsp;|VMSTEST |Acceso |101   |
   ---  
 
6. Probar la conexión.<p>Puede tardar unos segundos en completarse antes de que puede hacer ping correctamente en el otro adaptador.  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados:**_
     
   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>Paso 9. Probar el conmutador Virtual de Hyper-V RDMA (modo 2)

La siguiente imagen muestra el estado actual de los hosts de Hyper-V, incluido el vSwitch en el Host de Hyper-V 1.

![Probar el conmutador Virtual de Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. Establezca la prioridad de etiquetado en la vNIC de host.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  
   
   _**Resultados:**_

    Nombre: VMSTEST IeeePriorityTag: Activado


2. Ver la información de RDMA de adaptador de red. 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**Resultados:**_

   |Name |InterfaceDescription |Enabled |
   |---- |-------------------- |-------|
   |vEthernet \(VMSTEST\)| Adaptador Ethernet virtuales Hyper-V #2|False|
   ---

   >[!NOTE]
   >Si el parámetro **habilitado** tiene el valor **False**, significa que RDMA no está habilitado.
    

3. Ver la información del adaptador de red.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Resultados:**_   
 
   |Nombre |InterfaceDescription |ifIndex |Estado |MacAddress |LinkSpeed|
   |----|--------------------|-------|------|----------|---------|
   |vEthernet (VMSTEST)|Adaptador Ethernet virtuales Hyper-V #2|27|Arriba|E4-1D-2D-07-40-71|40 Gbps|
   ---


4. Habilite RDMA en la vNIC de host.

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**Resultados:**_

   |Name |InterfaceDescription |Enabled |
   |---- |-------------------- |-------|
   |vEthernet \(VMSTEST\)| Adaptador Ethernet virtuales Hyper-V #2|True|
   ---

   >[!NOTE]
   >Si el parámetro **habilitado** tiene el valor **True**, significa que RDMA está habilitada.

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

La última línea en este resultado, "prueba de tráfico RDMA correcto: Tráfico RDMA se envió a 192.168.1.5,"muestra que ha configurado correctamente convergente NIC en el adaptador.

## <a name="related-topics"></a>Temas relacionados
- [Configuración de NIC convergente NIC asociadas](cnic-datacenter.md)
- [Configuración del conmutador físico de NIC convergente](cnic-app-switch-config.md)
- [Solución de problemas convergente configuraciones de NIC](cnic-app-troubleshoot.md)
