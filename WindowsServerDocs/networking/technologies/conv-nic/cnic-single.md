---
title: Configuración de NIC convergente con un adaptador de red
description: Este tema proporciona instrucciones sobre cómo implementar NIC convergido con un adaptador de red en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d6663a966026afb301a4bad90a9573d16fc82875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configuración de NIC convergente con un adaptador de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Las siguientes secciones proporcionan instrucciones para configurar el NIC convergido con una sola NIC en el host de Hyper-V.

La configuración de ejemplo en esta guía muestra dos hosts de Hyper-V, **un Host de Hyper-V**, y **B de Host de Hyper-V**.

## <a name="test-connectivity-between-source-and-destination"></a>Probar la conectividad entre el origen y de destino

Esta sección proporciona los pasos necesarios para probar la conectividad entre el origen y destino Hyper-V Hosts.

La ilustración siguiente muestra dos Hosts de Hyper-V, **un Host de Hyper-V** y **B de Host de Hyper-V**. 

Ambos servidores tienen una sola física NIC (pNIC) instalada y la NIC están conectadas a una parte superior del conmutador \(ToR\) físico. Además, los servidores se encuentran en la misma subred, que es 192.168.1.x/24.

![Hosts de Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>Probar la conectividad NIC al conmutador Virtual Hyper\-V

Mediante el uso de este paso, puedes asegurarte de que la tarjeta física, para que crearás más adelante un conmutador Virtual Hyper\-V, se puede conectar con el host de destino. 

Esta prueba muestra conectividad mediante \(L3\) Layer 3 - o la capa IP - así como \(L2\) nivel 2.

Puedes usar el siguiente comando de Windows PowerShell para obtener las propiedades del adaptador de red.

    Get-NetAdapter
     
A continuación se muestran los resultados de ejemplo de este comando.

|Nombre|InterfaceDescription|ifIndex|Estado|Dirección MAC|Velocidad de vínculo|
|-----|--------------------|-------|-----|----------|---------|
|
|M1|Mellanox ConnectX 3 Pro...| 4| Arriba|7C-FE-90-93-8F-A1|40 GB/s|

Puedes usar uno de los siguientes comandos para obtener las propiedades del adaptador adicional, incluida la dirección IP.

    Get-NetAdapter M1 | fl *

A continuación se muestran los resultados de ejemplo editada de este comando.
    
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
    

### <a name="ensure-that-source-and-destination-can-communicate"></a>Asegúrate de que puedan comunicarse origen y destino

Puedes usar este paso para comprobar la comunicación bidireccional \ (ping desde el origen al destino y viceversa en ambos sistemas con errores\).  En el siguiente ejemplo, el **prueba NetConnection** se usa el comando de Windows PowerShell, pero si lo prefieres, puedes usar la **ping** comando. 

>[!NOTE]
>Si estás seguro de que tu hosts pueden comunicarse entre sí, puedes omitir este paso.

    Test-NetConnection 192.168.1.5

A continuación se muestran los resultados de ejemplo de este comando.

|Parámetro|Valor|
|---------|-----|
|
|ComputerName|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|M1|
|Dirección de origen|192.168.1.3|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0 ms|

En algunos casos, es posible que debas deshabilitar el Firewall de Windows con seguridad avanzada para realizar esta prueba con éxito. Si se deshabilita el firewall, seguridad recuerde y asegurarse de que la configuración cumple los requisitos de seguridad de la organización.

El siguiente comando de ejemplo te permite deshabilitar todos los perfiles de firewall.

    Set-NetFirewallProfile -All -Enabled False
    
Después de deshabilitar el firewall, puedes usar el siguiente comando para probar la conexión.

    Test-NetConnection 192.168.1.5

A continuación se muestran los resultados de ejemplo de este comando.

|Parámetro|Valor|
|---------|-----|
|
|ComputerName|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|Prueba-40G-1|
|Dirección de origen|192.168.1.3|
|PingSucceeded|"False"|
|PingReplyDetails \(RTT\)|0 ms|

## <a name="configure-vlans-optional"></a>Configurar VLAN \(Optional\)

Muchas de las configuraciones de red hacen uso de VLAN. Si vas a usar VLAN en la red, debe repetir la prueba anterior con VLAN configuradas. \ (Si vas a usar RoCE para los servicios RDMA debe habilitar VLAN. \)

En este paso, el NIC están en **acceso** modo. Sin embargo cuando creas un \(vSwitch\) conmutador Virtual de Hyper-V más adelante en esta guía, se aplican a las propiedades VLAN en el nivel de puerto vSwitch. 

Dado que un modificador puede hospedar varias VLAN, es necesario para la parte superior de bastidor \(ToR\) interruptor tener el puerto que el host está conectado a configurado en el modo de enlace.

>[!NOTE]
>Consulte la documentación del conmutador de términos de referencia para obtener instrucciones sobre cómo configurar el modo de tronco en el conmutador.

La ilustración siguiente muestra dos hosts de Hyper-V, cada uno con un adaptador de red física, y cada uno configurado para comunicar en VLAN 101.

![Configurar las redes de área local virtuales](../../media/Converged-NIC/2-single-configure-vlans.jpg)

### <a name="configure-the-vlan-id"></a>Configurar el identificador de VLAN

Puedes usar este paso para configurar los identificadores de VLAN para NIC instaladas en los hosts de Hyper-V.

#### <a name="configure-nic-m1"></a>Configurar el NIC M1

Con los comandos siguientes y configurar el identificador de VLAN para la primera NIC, M1, a continuación, ver la configuración resultante.

>[!IMPORTANT]
>No ejecutes este comando si está conectado al host de forma remota a través de esta interfaz, ya que esto conlleva pérdida de acceso al host.
    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
    
A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |DisplayName| Valor de visualización| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|M1|ID. DE VLAN|101|VlanID|{101}|


Asegúrate de que el identificador de VLAN surte efecto independiente de la implementación de adaptador de red mediante el siguiente comando para reiniciar el adaptador de red.

    Restart-NetAdapter -Name "M1"

Puedes usar el siguiente comando para asegurarse de que el estado del adaptador de red es **arriba** antes de continuar.

    Get-NetAdapter -Name "M1"

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre|InterfaceDescription|ifIndex| Estado|Dirección MAC|Velocidad de vínculo|
|----|--------------------|-------|------|----------| ---------|
|
|M1|Ada Mellanox ConnectX 3 Pro Ethernet...|4|Arriba|7C-FE-90-93-8F-A1|40 GB/s|

Asegúrate de que realice este paso en servidores locales y de destino. Si el servidor de destino no está configurado con el mismo identificador de VLAN como el servidor local, no se pueden comunicar los dos.

### <a name="verify-connectivity"></a>Comprobar la conectividad

Puedes usar esta sección para comprobar la conectividad después de reinician los adaptadores de red. Puedes confirmar conectividad después de aplicar la etiqueta VLAN a ambos adaptadores. Si se produce un error de conectividad, puede inspeccionar el conmutador de participación de destino o la configuración de VLAN en la misma VLAN. 

>[!IMPORTANT]
>Después de realizar los pasos en la sección anterior, puede tardar varios segundos para el dispositivo para reiniciar y estén disponibles en la red.

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>Comprobar la conectividad de NIC prueba-40G-1

Para comprobar la conectividad para la primera NIC, puede ejecutar el comando siguiente.

    Test-NetConnection 192.168.1.5
    
## <a name="configure-data-center-bridging-dcb"></a>Configurar el centro de datos puente \(DCB\)

El siguiente paso es configurar \(QoS\) DCB y calidad de servicio, lo que requiere que instalar primero la característica de Windows Server 2016 DCB.

>[!NOTE]
>Debes realizar todos los pasos de configuración siguientes DCB y QoS en todos los servidores que están diseñados para comunicarse entre sí.

### <a name="install-data-center-bridging-dcb"></a>Instalar el puente \(DCB\) el centro de datos

Puedes usar este paso para instalar y habilitar DCB. 

>[!IMPORTANT]
>- Instalar y configurar DCB están **opcional** para configuraciones de red que usan iWarp para los servicios RDMA.
>- Instalar y configurar DCB están **requiere** para configuraciones de red que usan RoCE \(any version\) para los servicios RDMA.


Puedes usar el siguiente comando para instalar DCB en cada uno de los hosts de Hyper-V. 

    Install-WindowsFeature Data-Center-Bridging

### <a name="set-the-qos-policies-for-smb-direct"></a>Establecer las directivas de QoS SMB Direct 

Puedes usar el siguiente comando para configurar las directivas de QoS para SMB directa.

>[!IMPORTANT]
>- Este paso es opcional para las configuraciones de red que usan iWarp.
>- Este paso es necesario para las configuraciones de red que usan RoCE.
>- En el siguiente comando de ejemplo, el valor "3" es arbitrario. Puedes usar cualquier valor entre 1 y 7 siempre que use el mismo valor en toda la configuración de directivas de QoS.

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

A continuación se muestran los resultados de ejemplo de este comando.

|Parámetro|Valor|
|---------|-----|
|
|Nombre |SMB|
|Propietario|\(Machine\) Directiva de grupo|
|NetworkProfile|Todos los|
|Prioridad|127|
|JobObject|&nbsp;| 
|NetDirectPort|445
|PriorityValue|3

### <a name="for-roce-deployments-turn-on-priority-flow-control-for-smb-traffic"></a>Para RoCE implementaciones activar el Control de flujo de prioridad para el tráfico SMB 

Si estás usando RoCE para los servicios RDMA, puedes usar los siguientes comandos para habilitar el control de flujo SMB y ver los resultados. Control de flujo de la prioridad se requiere para RoCE, pero es innecesario cuando usas iWarp.

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

A continuación se muestran ejemplos de resultados de la **Get NetQosFlowControl** comando.

|Prioridad|Habilitado|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |"False" |Global|&nbsp;|&nbsp;|
|1 |"False" |Global|&nbsp;|&nbsp;|
|2 |"False" |Global|&nbsp;|&nbsp;|
|3 |True  |Global|&nbsp;|&nbsp;|
|4 |"False" |Global|&nbsp;|&nbsp;|
|5 |"False" |Global|&nbsp;|&nbsp;|
|6 |"False" |Global|&nbsp;|&nbsp;|
|7 |"False" |Global|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>Habilitar QoS para los adaptadores de red local y de destino
Con este paso puede habilitar DCB en adaptadores de red específicos.

>[!IMPORTANT]
>-  Este paso no es necesario para las configuraciones de red que usan iWarp.
>-  Este paso es necesario para las configuraciones de red que usan RoCE.




#### <a name="enable-qos-for-nic-m1"></a>Habilitar QoS para M1 NIC

Puedes usar los siguientes comandos para habilitar QoS y ver los resultados de la configuración.

    Enable-NetAdapterQos -InterfaceAlias "M1"
    Get-NetAdapterQos -Name "M1"

A continuación se muestran los resultados de ejemplo de este comando.

**Nombre**: M1 **habilitado**: True **funcionalidades**:   

|Parámetro|Hardware|Actual|
|---------|--------|-------|
|
|MacSecBypass|NotSupported|NotSupported|
|DcbxSupport|Ninguno|Ninguno|
|NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
 
**OperationalTrafficClasses**: 

|TC|TSA|Ancho de banda|Prioridades|
|----|-----|--------|-------|
|
|0| ESTABLECE|70%|0-2,4-7|
|1|ESTABLECE|30%|3

**OperationalFlowControl**: prioridad 3 habilitado **OperationalClassifications**:

|Protocolo|Tipo de puerto /|Prioridad|
|--------|---------|--------|
|
|Predeterminado|&nbsp;|0|
|NetDirect| 445|3|

### <a name="reserve-a-percentage-of-the-bandwidth-for-smb-direct-rdma"></a>Reservar un porcentaje del ancho de banda para \(RDMA\) SMB directa

Puedes usar el siguiente comando para reservar un porcentaje del ancho de banda para SMB directa.  

En este ejemplo, se usa una reserva de ancho de banda de 30%. Debes seleccionar un valor que representa a lo que esperas que requerirá el tráfico de almacenamiento. El valor de la **- bandwidthpercentage** parámetro debe ser un múltiplo del 10%.

    New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ESTABLECE     | 30 |3 |Global |&nbsp;|&nbsp;|                                      
 
Puedes usar el siguiente comando para ver información de reserva de ancho de banda.

    Get-NetQosTrafficClass

A continuación se muestran los resultados de ejemplo de este comando.
 
|Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Default]|ESTABLECE|70 |0-2,4-7| Global|&nbsp;|&nbsp;| 
|SMB      |ESTABLECE|30 |3 |Global|&nbsp;|&nbsp;| 

## <a name="remove-debugger-conflict-mellanox-adapter-only"></a>Quitar el depurador conflicto \(Mellanox adapter only\)

Si estás usando un adaptador de Mellanox que necesita realizar este paso para configurar al depurador. De manera predeterminada, cuando se usa un adaptador Mellanox, el depurador asociado bloquea NetQos. Puedes usar el siguiente comando para invalidar al depurador.

    
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    

## <a name="test-rdma-native-host"></a>Prueba RDMA (host nativo)

Puedes usar este paso para garantizar que el tejido está configurado correctamente antes de crear un vSwitch y la transición al RDMA \(Converged NIC\).

La ilustración siguiente muestra el estado actual de los hosts de Hyper-V.

![Prueba RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

Para comprobar la configuración de RDMA, puede ejecutar el comando siguiente.

    Get-NetAdapterRdma

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |InterfaceDescription |Habilitado|
|----|--------------------|-------|
|
|M1| Adaptador Ethernet de Mellanox ConnectX 3 Pro |True|

### <a name="download-diskspdexe-and-a-powershell-script"></a>Descargar un Script de PowerShell y DiskSpd.exe

Para continuar, primero debes descargar los siguientes elementos.

- Descarga la utilidad DiskSpd.exe y extrae la utilidad en C:\TEST\ [utilidad Diskspd: un eficaz almacenamiento pruebas herramienta (reemplazo SQLIO)](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223)

- Descargar el script de powershell Test-RDMA en C:\TEST\[https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Determinar el valor de ifIndex de su adaptador de destino

Puedes usar el siguiente comando para detectar el valor ifIndex del adaptador de destino. Puedes usar este valor en los siguientes pasos cuando se ejecuta el script que hayas descargado.

    Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

A continuación se muestran los resultados de ejemplo de este comando.

|InterfaceAlias |ÍndiceDeInterfaz |IPv4Address|
|-------------- |-------------- |-----------|
|
|M2 |14 |{192.168.1.5}|

### <a name="run-the-powershell-script"></a>Ejecutar el script de PowerShell

Cuando ejecutas la. ps1 de Test-Rdma script de Windows PowerShell, puede pasar el valor de ifIndex al script, junto con la dirección IP del adaptador remoto en la misma VLAN.

Puedes usar el siguiente comando de ejemplo para ejecutar el script con una ifIndex de 14 del adaptador de red 192.168.1.5.
    
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
    

>[!NOTE]
>Si el tráfico RDMA produce un error, para el caso de RoCE específicamente, consulta la configuración de ToR Switch para la configuración correcta de PFC/NET que debe coincidir con la configuración de Host. Consulta la sección de QoS en este documento para valores de referencia.

## <a name="remove-the-access-vlan-setting"></a>Quitar la configuración de acceso VLAN

Debes quitar la configuración de VLAN que instalaste anteriormente en preparación para crear el conmutador de Hyper-V.  Puedes usar el siguiente comando para quitar la configuración de VLAN de acceso de la NIC física. Esta acción impide que la NIC etiquetado automático el tráfico de salida con el identificador de VLAN incorrecto y también evita que filtra el tráfico de entrada que no coincide con el identificador de VLAN de acceso.

    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
    

Puedes usar el siguiente comando de ejemplo para confirmar la opción VlanID y ver los resultados, que muestran que el valor de ID es cero.

    
    Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
    


## <a name="create-a-hyper-v-virtual-switch"></a>Crear un conmutador Virtual de Hyper-V

En esta sección puedes usar para crear un \(vSwitch\) conmutador Virtual de Hyper-V en los hosts de Hyper-V.

La ilustración siguiente muestra el 1 de Host de Hyper-V con un vSwitch.

![Crear un conmutador Virtual de Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


### <a name="create-an-external-hyper-v-virtual-switch"></a>Crear un conmutador Virtual externo de Hyper-V

Puedes usar esta sección para crear un vSwitch externo en Hyper-V en el Host de Hyper-V A.

Puedes usar el siguiente comando de ejemplo para crear un modificador denominado VMSTEST.

>[!NOTE]
>El parámetro **AllowManagementOS** en el siguiente comando crea un vNIC Host que hereda la dirección MAC y la dirección IP del NIC física.

    New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |Externo |Adaptador Ethernet de Mellanox ConnectX 3 Pro|

Puedes usar el siguiente comando para ver las propiedades del adaptador de red.

    Get-NetAdapter | ft -AutoSize

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |InterfaceDescription | ifIndex |Estado |Dirección MAC |Velocidad de vínculo|
|---- |-------------------- |-------| ------|----------|---------|
|
|vEthernet \(VMSTEST\) |Adaptador Ethernet virtuales de Hyper-V #2|27 |Arriba |E4-1D-2D-07-40-71 |40 GB/s|


Puedes administrar un vNIC de Host de dos maneras. Un método es el **AdaptadorRed** vista, que funciona según la "vEthernet \(VMSTEST\)" nombre.

El otro método es el **VMNetworkAdapter** vista, que quita el prefijo "vEthernet" y simplemente se usa el nombre de vmswitch.

La **VMNetworkAdapter** vista muestra algunas propiedades de adaptador de red que no se muestran con el **AdaptadorRed** comando.

Puedes usar el siguiente comando para ver los resultados de la **VMNetworkAdapter** método.

    Get-VMNetworkAdapter –ManagementOS | ft -AutoSize

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |IsManagementOs |VMName |SwitchName |Dirección MAC |Estado |DireccionesIP|
|----|-------------- |------ |----------|----------|------ |-----------|
|
|Conmutador de CORP externo |True |Conmutador de CORP externo| 001B785768AA |{Aceptar} |&nbsp;|
|VMSTEST |True |VMSTEST | E41D2D074071| {Aceptar} | &nbsp;| 

### <a name="test-the-connection"></a>Probar la conexión

Puedes usar el siguiente comando de ejemplo para probar la conexión y ver los resultados.
    
    Test-NetConnection 192.168.1.5

    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
Puedes usar los siguientes comandos de ejemplo para asignar y ver la configuración de VLAN del adaptador de red.
    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

|VMName |VMNetworkAdapterName |Modo |VlanList|
|------ |-------------------- |---- |--------|
|
|&nbsp;|VMSTEST |Acceso |101     
 
### <a name="test-the-connection"></a>Probar la conexión

Recuerda que el cambio puede tardar unos segundos en completarse antes de que puede hacer ping correctamente el otro adaptador.

Puedes usar el siguiente comando de ejemplo para probar la conexión y ver los resultados.
    
    Test-NetConnection 192.168.1.5
     
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

## <a name="test-hyper-v-virtual-switch-rdma-mode-2"></a>Prueba el conmutador Virtual de Hyper-V RDMA (modo 2)

La ilustración siguiente muestra el estado actual de los hosts de Hyper-V, incluidos el vSwitch en el Host de Hyper-V 1.

![Prueba el conmutador Virtual Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


### <a name="set-priority-tagging-on-the-host-vnic"></a>Establecer la prioridad de etiquetado de la vNIC Host

Puedes usar los siguientes comandos de ejemplo para establecer la prioridad de etiquetado de la vNIC Host y ver los resultados de las acciones.
    
    Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
    
    Name: VMSTEST
    IeeePriorityTag : On
    
Puedes usar el siguiente comando de ejemplo para ver información de RDMA del adaptador de red. En los resultados, cuando el parámetro **habilitado** tiene el valor **False**, significa que no está habilitado RDMA.
    
    Get-NetAdapterRdma
    
|Nombre |InterfaceDescription |Habilitado |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| Adaptador Ethernet virtuales de Hyper-V #2|"False"|

### <a name="enable-rdma-on-the-host-vnic"></a>Habilitar RDMA en la vNIC Host

Puedes usar los siguientes comandos de ejemplo para ver las propiedades del adaptador de red, habilitar RDMA para el adaptador y, a continuación, ver la información de RDMA del adaptador de red.
    
    Get-NetAdapter
    
|Nombre |InterfaceDescription |ifIndex |Estado |Dirección MAC |Velocidad de vínculo|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|Adaptador Ethernet virtuales de Hyper-V #2|27|Arriba|E4-1D-2D-07-40-71|40 GB/s|

    Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
    Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"

En los resultados siguientes, cuando el parámetro **habilitado** tiene el valor **True**, significa que RDMA está habilitado.

|Nombre |InterfaceDescription |Habilitado |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| Adaptador Ethernet virtuales de Hyper-V #2|True|


    
    Get-NetAdapter 
    

|Nombre |InterfaceDescription |ifIndex |Estado |Dirección MAC |Velocidad de vínculo|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|Adaptador Ethernet virtuales de Hyper-V #2|27|Arriba|E4-1D-2D-07-40-71|40 GB/s|

### <a name="perform-rdma-traffic-test-by-using-the-script"></a>Realizar la prueba de tráfico RDMA mediante el script

Puedes usar el siguiente comando para ejecutar el script que has descargado y ver los resultados.
    
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
    
La última línea en esta salida, "prueba de tráfico RDMA correcto: tráfico RDMA se envió a 192.168.1.5," se muestra correctamente configuradas NIC convergido en el adaptador.

## <a name="all-topics-in-this-guide"></a>Todos los temas de esta guía

Esta guía contiene los siguientes temas.

- [Configuración de NIC convergente con un adaptador de red](cnic-single.md)
- [Configuración de NIC convergente NIC de equipo](cnic-datacenter.md)
- [Configuración del conmutador físico para convergente NIC](cnic-app-switch-config.md)
- [Solución de problemas convergido configuraciones NIC](cnic-app-troubleshoot.md)
