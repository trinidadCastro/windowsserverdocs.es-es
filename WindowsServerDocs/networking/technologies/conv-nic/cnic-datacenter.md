---
title: Convergente NIC en una configuración de NIC agrupados (datacenter)
description: En este tema, se proporcionan instrucciones para implementar estos convergen de NIC en una configuración de NIC agrupados con Switch Embedded Teaming (SET).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/17/2018
ms.openlocfilehash: 5f99600e24c62da9bdf674897dbadde9246b7bb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821036"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>Convergente NIC en una configuración de NIC agrupados (datacenter)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, se proporcionan instrucciones para implementar estos convergen de NIC en una configuración de NIC agrupados con Switch Embedded Teaming \(establecer\). 

La configuración de ejemplo en este tema describe dos hosts de Hyper-V, **1 Host de Hyper-V** y **2 de Host de Hyper-V**. Ambos hosts tengan dos adaptadores de red. En cada host, un adaptador está conectado a la subred 192.168.1.x/24 y un adaptador está conectado a la subred 192.168.2.x/24.

![Hosts de Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Paso 1. Probar la conectividad entre el origen y destino

Asegúrese de que la física NIC puede conectarse al host de destino.  Esta prueba muestra la conectividad mediante el uso de nivel 3 \(L3\) - o el nivel de IP - así como el nivel 2 \(L2\) redes de área local virtuales \(VLAN\).

1. Ver las propiedades de adaptador de red primeras.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
  
   _**Resultados:**_

   |Nombre|InterfaceDescription|ifIndex|Estado|MacAddress|LinkSpeed|
   |-----|--------------------|-------|-----|----------|---------|
   |Prueba 1 de 40G|Mellanox ConnectX-3 Pro Ethernet Adapter|11|Arriba|E4-1D-2D-07-43-D0|40 Gbps|
   ---
   
2. Ver propiedades adicionales para el primer adaptador, incluida la dirección IP. 

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```
   
   _**Resultados:**_

   |Parámetro|Valor|
   |---------|-----|
   |IPAddress| 192.168.1.3|
   |InterfaceIndex|11|
   |InterfaceAlias|Prueba 1 de 40G|
   |AddressFamily|IPv4|
   |Tipo| Unidifusión|
   |PrefixLength|24|
   ---

3. Ver las propiedades del segundo adaptador de red.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```
   
   _**Resultados:**_

   |Name |InterfaceDescription |ifIndex |Estado |MacAddress |LinkSpeed|
   |----|--------------------|-------|------|----------|---------|
   |TEST-40G-2 |Mellanox ConnectX-3 Pro Ethernet A...#2 |13 |Arriba |E4-1D-2D-07-40-70 |40 Gbps|
   ---
   
4. Ver propiedades adicionales del segundo adaptador, incluida la dirección IP.

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```
   
   _**Resultados:**_

   |Parámetro|Valor|
   |---------|-----|
   |IPAddress|192.168.2.3|
   |InterfaceIndex|13|
   |InterfaceAlias|TEST-40G-2|
   |AddressFamily|IPv4|
   |Tipo|Unidifusión|
   |PrefixLength|24|
   ---

5. Compruebe que otros pNICs de miembro equipo NIC o un conjunto tiene una dirección IP válida.<p>Usar una subred independiente, \(xxx.xxx. **2**.xxx vs xxx.xxx. **1**.xxx\), para facilitar el envío de este adaptador al destino. En caso contrario, si encuentra ambos pNICs en la misma subred, equilibra la carga de la pila TCP/IP de Windows entre las interfaces y una validación simple se vuelve más complicada.


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Paso 2. Asegúrese de que pueden comunicarse origen y destino

En este paso, usamos el **Test-NetConnection** comando de Windows PowerShell, pero si usas el **ping** comando si lo prefiere. 

1. Compruebe la comunicación bidireccional.

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
   
   
4. Compruebe la conectividad de NIC adicionales. Repita los pasos anteriores para todos los pNICs posteriores incluidos en el equipo NIC o un conjunto.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```
   
   _**Resultados:**_

   |Parámetro|Valor|
   |---------|-----|
   |ComputerName|192.168.2.5|
   |RemoteAddress|192.168.2.5|
   |InterfaceAlias|Prueba-40G-2|
   |SourceAddress|192.168.2.3|
   |PingSucceeded|False|
   |PingReplyDetails \(RTT\)|0 milisegundos|
   ---

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Paso 3. Configurar los identificadores de VLAN para las NIC que se instala en los hosts de Hyper-V

Muchas configuraciones de red hacen uso de las VLAN y, si va a usar las VLAN en la red, debe repetir la prueba anterior con VLAN configuradas.

Este paso, las NIC están en **acceso** modo. Sin embargo, cuando creas un conmutador Virtual de Hyper-V \(vSwitch\) más adelante en esta guía, se aplican las propiedades VLAN en el nivel de puerto vSwitch. 

Dado que un conmutador puede hospedar varias VLAN, es necesario para la parte superior del Rack \(ToR\) conmutador físico que el puerto que está conectado el host configurado en modo de tronco.

>[!NOTE]
>Para obtener instrucciones sobre cómo configurar el modo de tronco en el conmutador, consulte la documentación del conmutador ToR.

La siguiente imagen muestra dos hosts de Hyper-V con dos adaptadores de red cada tienen VLAN 101 y 102 de VLAN configurados en las propiedades del adaptador de red.

![Configurar redes de área local virtuales](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>Según el Institute of Electrical and Electronics Engineers \(IEEE\) estándares, la calidad de servicio de red \(QoS\) propiedades en la NIC física actúan en el encabezado de 802.1p incrustado dentro de la 802.1Q \(VLAN\) encabezado al configurar el ID. VLAN

1. Configure el identificador de VLAN en la primera NIC, Test-40G-1.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**Resultados:**_   

   |Nombre |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |TEST-40G-1|ID. DE VLAN|101|VlanID|{101}|
   ---

2. Reiniciar para aplicar el ID. VLAN el adaptador de red

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-1"
   ```
   
3. Asegúrese de que el estado es **seguridad**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
   
   _**Resultados:**_

   |Name|InterfaceDescription|ifIndex| Estado|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |Prueba 1 de 40G|Mellanox ConnectX-3 Pro Ethernet Ada...|11|Arriba|E4-1D-2D-07-43-D0|40 Gbps|
   ---
    
4. Configure el identificador de VLAN en la segunda NIC, Test-40G-2.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ``` 

   _**Resultados:**_

   |Nombre |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |TEST-40G-2|ID. DE VLAN|102|VlanID|{102}|
   ---
   
5. Reiniciar para aplicar el ID. VLAN el adaptador de red

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-2" 
   ```
   
6. Asegúrese de que el estado es **seguridad**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
   
   _**Resultados:**_

   |Nombre|InterfaceDescription|ifIndex| Estado|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |Prueba-40G-2 |Mellanox ConnectX-3 Pro Ethernet Ada... |11 |Arriba |E4-1D-2D-07-43-D1 |40 Gbps|
   ---

   >[!IMPORTANT]
   >Podría tardar varios segundos para el dispositivo para reiniciar y están disponibles en la red. 
   
7. Compruebe la conectividad de la primera NIC, Test-40G-1.<p>Si se produce un error de conectividad, inspeccione el conmutador de participación de configuración o el destino de VLAN en la misma VLAN. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados:**_   

   |Parámetro|Valor|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|Prueba 1 de 40G|
   |SourceAddress|192.168.1.5|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 milisegundos|
   ---
   
8. Compruebe la conectividad de la primera NIC, Test-40G-2.<p>Si se produce un error de conectividad, inspeccione el conmutador de participación de configuración o el destino de VLAN en la misma VLAN.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**Resultados:**_    

   |Parámetro|Valor|
   |---------|-----|
   |ComputerName|192.168.2.5|
   |RemoteAddress|192.168.2.5|
   |InterfaceAlias|Prueba-40G-2|
   |SourceAddress|192.168.2.3|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 milisegundos|
   ---
   
   >[!IMPORTANT]
   >No es raro que un **Test-NetConnection** o error de ping que se produzca inmediatamente después de realizar **Restart-NetAdapter**.  Por lo que espere a que el adaptador de red en inicializarse por completo y, a continuación, vuelva a intentarlo.
   >
   >Si las conexiones de VLAN 101 funcionan, pero no las conexiones de 102 de VLAN, el problema podría ser que el modificador debe configurarse para permitir el tráfico del puerto de la VLAN deseada. Puede comprobar para este temporalmente estableciendo los adaptadores por error a VLAN 101 y repetir la prueba de conectividad.

   
   La siguiente imagen muestra los hosts de Hyper-V después de configurar correctamente las VLAN.

   ![Configuración de calidad de servicio](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)
   
   
## <a name="step-4-configure-quality-of-service-qos"></a>Paso 4. Configuración de calidad de servicio \(QoS\)

>[!NOTE]
>Debe realizar todos estos pasos de configuración DCB y QoS en todos los hosts que están diseñados para comunicarse entre sí.

1. Instalar el protocolo de puente del centro de datos \(DCB\) en cada uno de los hosts de Hyper-V.

   - **Opcional** para configuraciones de red que usan iWarp.
   - **Requiere** configuraciones de red que usan RoCE \(cualquier versión\) para servicios RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**Resultados:**_

   |Correcto |Se requiere un reinicio |Código de salida|Resultado de la característica|
   |------- |-------------- |--------- |-------------- |
   |True |No |Correcto| {Data Center Bridging}|
   ---
   
2. Establecer las directivas de QoS para SMB-Direct:

   - **Opcional** para configuraciones de red que usan iWarp.
   - **Requiere** configuraciones de red que usan RoCE \(cualquier versión\) para servicios RDMA.
   
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
   |NetDirectPort|445
   |PriorityValue|3
   ---

3. Establecer directivas de QoS adicionales para el tráfico restante en la interfaz.   

   ```PowerShell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**Resultados:**_   

   |Parámetro|Valor|
   |---------|-----|
   |Nombre | DEFAULT|
   |Propietario|Directiva de grupo \(máquina\)|
   |NetworkProfile|Todos|
   |Precedencia|127|
   |Plantilla| Default|
   |JobObject| &nbsp;|
   |PriorityValue|0|
   ---
   
4. Activar **Control de flujo de prioridad** para el tráfico SMB, que no es necesario para iWarp.

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
   |3 |True |Global|&nbsp;|&nbsp;|
   |4 |False |Global|&nbsp;|&nbsp;|
   |5 |False |Global|&nbsp;|&nbsp;|
   |6 |False |Global|&nbsp;|&nbsp;|
   |7 |False |Global|&nbsp;|&nbsp;|
   ---
   
   >**IMPORTANTE** si los resultados no coincide con estos resultados porque los elementos que no sea de 3 tienen habilitado el valor True, debe deshabilitar **flujo** para estas clases.
   >
   >```PowerShell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >En configuraciones más complejas, las otras clases de tráfico podrían requerir control de flujo, sin embargo, estos escenarios están fuera del ámbito de esta guía.


5. Habilitar la calidad de servicio para la primera NIC, Test-40G-1.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1 
   Enabled: True
   ```
   _**Capacidades**:_   

   |Parámetro|Hardware|Actual|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|Ninguno|Ninguno|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---
 
   _**OperationalTrafficClasses**:_    

   |TC|TSA|Ancho de banda|Prioridades|
   |----|-----|--------|-------|
   |0| Estricta|&nbsp;|0-7|
   ---

   _**OperationalFlowControl**:_  

   Prioridad 3 habilitado  

   _**OperationalClassifications**:_  

   |Protocolo|Tipo de puerto /|Prioridad|
   |--------|---------|--------|
   |Default|&nbsp;|0|
   |NetDirect| 445|3|
   ---
   
6. Habilitar la calidad de servicio para la segunda NIC, Test-40G-2.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2 
   Enabled: True 
   ```

   _**Capacidades**:_ 

   |Parámetro|Hardware|Actual|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|Ninguno|Ninguno|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---

   _**OperationalTrafficClasses**:_  

   |TC|TSA|Ancho de banda|Prioridades|
   |----|-----|--------|-------|
   |0| Estricta|&nbsp;|0-7|
   ---
   
    _**OperationalFlowControl**:_  

    Prioridad 3 habilitado  
   
   _**OperationalClassifications**:_  

   |Protocolo|Tipo de puerto /|Prioridad|
   |--------|---------|--------|
   |Default|&nbsp;|0|
   |NetDirect| 445|3|
   ---

   
7. Reservar la mitad del ancho de banda SMB directo \(RDMA\)

   ```PowerShell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```
   
   _**Resultados:**_  
   
   |Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |SMB | ETS     | 50 |3 |Global |&nbsp;|&nbsp;|   
   ---

8. Ver la configuración de reserva de ancho de banda:   

   ```PowerShell
   Get-NetQosTrafficClass | ft -AutoSize
   ```
   
   _**Resultados:**_  

   |Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |[Predeterminado]| ETS|50 |0-2,4-7|  Global|&nbsp;|&nbsp;| 
   |SMB |ETS|50 |3 |Global|&nbsp;|&nbsp;| 
   ---
   
9. (Opcional) Cree dos clases de tráfico adicional para el tráfico IP de inquilino. 

   >[!TIP]
   >Puede omitir los valores "IP1" y "IP2".

   ```PowerShell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```
   
   _**Resultados:**_
   
   |Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |IP1 |ETS |10 |1 |Global|&nbsp;|&nbsp;|
   ---
   
   ```PowerShell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```
   
   _**Resultados:**_

   |Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |IP2 |ETS |10 |2 |Global|&nbsp;|&nbsp;|
   ---
   
10. Ver las clases de tráfico de QoS.

    ```PowerShell
    Get-NetQosTrafficClass | ft -AutoSize
    ```
    
    _**Resultados:**_

    |Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
    |----|---------| ------------ |--------| ---------|------- |------- |
    |[Predeterminado] |ETS |30 |0,4-7 |Global|&nbsp;|&nbsp;|
    |SMB |ETS |50 |3 |Global|&nbsp;|&nbsp;|
    |IP1 |ETS |10 |1 |Global|&nbsp;|&nbsp;|
    |IP2 |ETS |10 |2 |Global|&nbsp;|&nbsp;|
    ---
   
11. (Opcional) Invalidar al depurador.<p>De forma predeterminada, el depurador asociado bloquea NetQos. 

    ```PowerShell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**Resultados:**_  

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>Paso 5. Compruebe la configuración de RDMA \(modo 1\) 

Para asegurarse de que el tejido está configurado correctamente antes de crear un vSwitch y realizar la transición a RDMA \(modo 2\).

La siguiente imagen muestra el estado actual de los hosts de Hyper-V.

![Prueba RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. Compruebe la configuración de RDMA.

   ```PowerShell
   Get-NetAdapterRdma | ft -AutoSize
   ```
   
   _**Resultados:**_

   |Nombre |InterfaceDescription |Enabled|
   |----|--------------------|-------|
   |TEST-40G-1| Adaptador Mellanox ConnectX-4 VPI #2 |True|
   |TEST-40G-2| Adaptador Mellanox ConnectX-4 VPI |True|
   ---

2. Determinar la **ifIndex** valor de los adaptadores de destino.<p>Use este valor en pasos posteriores al ejecutar el script que descargó.   

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```
   
   _**Resultados:**_

   |InterfaceAlias |InterfaceIndex |Dirección IPv4|
   |-------------- |-------------- |-----------|
   |TEST-40G-1 |14 |{192.168.1.3}|
   |TEST-40G-2 | 13 |{192.168.2.3}|
   ---
   
3. Descargue el [DiskSpd.exe utilidad](https://aka.ms/diskspd) y extráigalo en C:\TEST\.

4. Descargue el [script de PowerShell de prueba RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) en una carpeta de prueba en la unidad local, por ejemplo, C:\TEST\.

5. Ejecute el **prueba Rdma.ps1** script de PowerShell para pasar el valor de ifIndex a la secuencia de comandos, junto con la dirección IP del primer adaptador remoto en la misma VLAN.<p>En este ejemplo, el script pasa el **ifIndex** valor 14 en la dirección IP de adaptador de red remota 192.168.1.5.
   
   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Resultados:**_ 
   
   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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

6. Ejecute el **prueba Rdma.ps1** script de PowerShell para pasar el valor de ifIndex a la secuencia de comandos, junto con la dirección IP del segundo adaptador remoto en la misma VLAN.<p>En este ejemplo, el script pasa el **ifIndex** valor 13 en la dirección IP de adaptador de red remota 192.168.2.5.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Resultados:**_ 
   
   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter TEST-40G-2 is a physical adapter
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
   VERBOSE: Remote IP 192.168.2.5 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 541185606 RDMA bytes written per second
   VERBOSE: 34821478 RDMA bytes sent per second
   VERBOSE: 954717307 RDMA bytes written per second
   VERBOSE: 35040816 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
   ``` 

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Paso 6. Crear un vSwitch de Hyper-V en los hosts de Hyper-V


La siguiente imagen muestra 1 Host de Hyper-V con un vSwitch.

![Crear un conmutador Virtual de Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. Crear un vSwitch en establecer el modo en el host de Hyper-V 1.

   ```PowerShell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```
   
   _**Resultado:**_

   |Nombre |SwitchType |NetAdapterInterfaceDescription|
   |---- |---------- |------------------------------|
   |VMSTEST |External |Teamed-Interface|
   ---
   
2. Ver el equipo de adaptadores físicos en conjunto.

   ```PowerShell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```
   
   _**Resultados:**_  
   
   ```
   Name: VMSTEST  
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec  
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}  
   TeamingMode: SwitchIndependent  
   LoadBalancingAlgorithm: Dynamic   
   ```
   
3. Mostrar dos vistas de la vNIC de host

   ```PowerShell
    Get-NetAdapter
   ```
   
   _**Resultados:**_

   |Name |InterfaceDescription |ifIndex |Estado |MacAddress |LinkSpeed|
   |---- |--------------------|-------|------|----------|---------|
   |vEthernet (VMSTEST)|Adaptador Ethernet virtuales Hyper-V #2 |28 |Arriba|E4-1D-2D-07-40-71|80 Gbps|
   ---
   
4. Ver propiedades adicionales de la vNIC de host. 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Resultados:**_

   |Name |IsManagementOs |VMName |SwitchName |MacAddress |Estado |IPAddresses|
   |----|--------------|------|----------|----------|------|-----------|
   |VMSTEST|True |VMSTEST |E41D2D074071| {Ok}|&nbsp;|
   ---
   

5. Probar la conexión de red con el adaptador de VLAN 101 remoto.

   ```PowerShell
   Test-NetConnection 192.168.1.5 
   ```
   
   _**Resultados:**_  
   
   ```
   WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable
    
   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (CORP-External-Switch)
   SourceAddress  : 10.199.48.170
   PingSucceeded  : False
   PingReplyDetails (RTT) : 0 ms
   ```
   
## <a name="step-7-remove-the-access-vlan-setting"></a>Paso 7. Quite la configuración de acceso de VLAN

En este paso, quite la configuración de acceso de VLAN de la NIC física y para establecer el VLANID utilizando el vSwitch.

Debe quitar la configuración de acceso de VLAN para evitar que tanto el etiquetado automático el tráfico de salida con el identificador de VLAN incorrecta y del filtrado de entrada de tráfico que no coincide con el identificador de VLAN de acceso.

1. Quite la configuración.
    
   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
   ```

2. Establece el identificador de VLAN.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```
   
   _**Resultados:**_  
   
   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101     
   ```
   
   
3. Probar la conexión de red.

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

   >**IMPORTANTE** si los resultados no son similares a los resultados de ejemplo y se produce un error de ping con el mensaje "Advertencia: Error de ping para 192.168.1.5--estado: DestinationHostUnreachable,"confirmar que el"vEthernet (VMSTEST)"tiene la dirección IP correcta.
   >
   >```PowerShell
   >Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
   >```
   >
   >Si no se establece la dirección IP, corrija el problema.
   >
   >```PowerShell
   >New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
   >
   >IPAddress : 192.168.1.3
   >InterfaceIndex: 37
   >InterfaceAlias: vEthernet (VMSTEST)
   >AddressFamily : IPv4
   >Type  : Unicast
   >PrefixLength  : 24
   >PrefixOrigin  : Manual
   >SuffixOrigin  : Manual
   >AddressState  : Tentative
   >ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   >PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   >SkipAsSource  : False
   >PolicyStore   : ActiveStore
   >```  
   

4. Cambiar el nombre de la NIC de administración.

   ```PowerShell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Resultados:**_ 
   
   |Name |IsManagementOs |VMName |SwitchName |MacAddress |Estado |IPAddresses
   |----|--------------|------|----------|----------|------|-----------|
   |CORP-External-Switch |True |&nbsp;|CORP-External-Switch |001B785768AA |{Ok}|&nbsp;|
   |MGT |True |&nbsp;|VMSTEST |E41D2D074071 |{Ok}|&nbsp;|
   ---
   
5. Ver propiedades NIC adicionales.

   ```PowerShell
   Get-NetAdapter
   ```
   
   _**Resultados:**_

   |Name |InterfaceDescription |ifIndex |Estado |MacAddress |LinkSpeed|
   |----|--------------------|------|----------|---------|------|
   |vEthernet (MGT) |Adaptador Ethernet virtuales Hyper-V #2 |28 |Arriba | E4-1D-2D-07-40-71 |80 Gbps|
   ---
   
## <a name="step-8-test-hyper-v-vswitch-rdma"></a>Paso 8. Probar Hyper-V vSwitch RDMA

La siguiente imagen muestra el estado actual de los hosts de Hyper-V, incluido el vSwitch en el Host de Hyper-V 1.

![Probar el conmutador Virtual de Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. Establezca la prioridad de etiquetado en la vNIC de Host para complementar la configuración de VLAN anterior.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```
   
   _**Resultados:**_  
      
   nombre: MGT  
   IeeePriorityTag:  Activado  
    
2. Cree dos VNIC de host para RDMA y conectarlos al vSwitch VMSTEST.

   ```PowerShell    
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```
   
3. Ver las propiedades de NIC de administración.

   ```PowerShell    
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Resultados:**_ 

   |Nombre |IsManagementOs |VMName |SwitchName |MacAddress |Estado |IPAddresses|
   |----|--------------|------|----------|----------|------|-----------|
   |CORP-External-Switch |True |CORP-External-Switch |001B785768AA|{Ok} |&nbsp;| 
   |Administración del |True |VMSTEST |E41D2D074071 |{Ok} |&nbsp;|
   |SMB1 |True |VMSTEST |00155D30AA00 |{Ok} |&nbsp;|
   |SMB2 |True |VMSTEST |00155D30AA01 |{Ok} |&nbsp;|
   ---
   
## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>Paso 9. Asignar una dirección IP a la vEthernet VNIC de Host de SMB \(bloque de mensajes 1\) y vEthernet \(SMB2\)

La prueba 1 de 40G y adaptadores físicos de prueba-40G-2 todavía tienen un acceso de VLAN de 101 y 102 configurado. Por este motivo, los adaptadores de etiquetar el tráfico - y ping se realiza correctamente. Anteriormente, se configura ambos identificadores de VLAN pNIC cero, Establece el vSwitch VMSTEST en VLAN 101. Después de eso, todavía puede hacer ping en el adaptador de VLAN 101 remoto mediante el uso de la vNIC MGT, pero actualmente no hay ningún miembro de 102 de VLAN.



1. Quitar la configuración de acceso de VLAN de la NIC física para impedir que tanto el etiquetado automático el tráfico de salida con el identificador de VLAN incorrecta y para evitar que se filtran el tráfico de entrada que no coincide con el identificador de VLAN de acceso.

   ```PowerShell    
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**Resultados:**_  

   ```   
   IPAddress : 192.168.2.111
   InterfaceIndex: 40
   InterfaceAlias: vEthernet (SMB1)
   AddressFamily : IPv4
   Type  : Unicast
   PrefixLength  : 24
   PrefixOrigin  : Manual
   SuffixOrigin  : Manual
   AddressState  : Invalid
   ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   SkipAsSource  : False
   PolicyStore   : PersistentStore
   ```

2. Probar el adaptador de VLAN 102 remoto.
    
   ```PowerShell
   Test-NetConnection 192.168.2.5 
   ```
   
   _**Resultados:**_  
   
   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```
    
3. Agregar una nueva dirección IP de interfaz vEthernet \(SMB2\).

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
   ```
   
   _**Resultados:**_ 
   
   ```
   IPAddress : 192.168.2.222
   InterfaceIndex: 44
   InterfaceAlias: vEthernet (SMB2)
   AddressFamily : IPv4
   Type  : Unicast
   PrefixLength  : 24
   PrefixOrigin  : Manual
   SuffixOrigin  : Manual
   AddressState  : Invalid
   ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   SkipAsSource  : False
   PolicyStore   : PersistentStore
   ```
   
4. Probar la conexión de nuevo.    


5. Coloque las VNIC de Host de RDMA en el 102 VLAN ya existentes.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS
    
   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**Resultados:**_ 

   ```   
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102 
      Mgt  Access   101 
      SMB2 Access   102 
      CORP-External-Switch Untagged
   ```
   
6. Inspeccione la asignación de bloque de mensajes 1 y SMB2 a las NIC físicas subyacentes en el equipo establece vSwitch.<p>La asociación de vNIC de Host a una NIC física es aleatorio y están sujetos a reequilibrio durante la creación y destrucción. En este caso, puede usar un mecanismo indirecto para comprobar la asociación actual. Las direcciones MAC de bloque de mensajes 1 y SMB2 asociadas con el miembro del equipo de NIC prueba-40G-2. Esto no es ideal porque no tiene una vNIC de Host de SMB asociada Test-40G-1 y no permitirá para el uso de tráfico RDMA a través del vínculo hasta que se le asigna una vNIC de Host de SMB.

   ```PowerShell    
   Get-NetAdapterVPort (Preferred)
    
   Get-NetAdapterVmqQueue
   ```
   
   _**Resultados:**_ 
   
   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. Ver las propiedades del adaptador de red de máquina virtual.

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Resultados:**_ 
   
   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
   Mgt  True  VMSTEST  E41D2D074071 {Ok}  
   SMB1 True  VMSTEST  00155D30AA00 {Ok}  
   SMB2 True  VMSTEST  00155D30AA01 {Ok}  
   ```

8. Ver la asignación de equipo del adaptador de red.<p>Los resultados no deben devolver información porque no se ha realizado aún asignación.
    
   ```PowerShell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```
   
   
9. Asignar el bloque de mensajes 1 y SMB2 para separar a los miembros del equipo NIC físicos y para ver los resultados de sus acciones.

   >[!IMPORTANT]
   >Asegúrese de completar este paso antes de continuar, o se produce un error en la implementación.
    
   ```PowerShell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"
    
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**Resultados:**_ 
   
   ```   
   NetAdapterName : Test-40G-1
   NetAdapterDeviceId : {BAA9A00F-A844-4740-AA93-6BD838F8CFBA}
   ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB1'
   IsTemplate : False
   CimSession : CimSession: .
   ComputerName   : 27-3145G0803
   IsDeleted  : False
    
   NetAdapterName : Test-40G-2
   NetAdapterDeviceId : {B7AB5BB3-8ACB-444B-8B7E-BC882935EBC8}
   ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB2'
   IsTemplate : False
   CimSession : CimSession: .
   ComputerName   : 27-3145G0803
   IsDeleted  : False
   ```
   
10. Confirme las asociaciones de MAC que creó anteriormente.

    ```PowerShell    
    Get-NetAdapterVmqQueue
    ```

    _**Resultados:**_ 
   
    ```   
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. Probar la conexión desde el sistema remoto porque ambos VNIC de Host reside en la misma subred y tiene el mismo identificador de VLAN \(102\).

    ```PowerShell 
    Test-NetConnection 192.168.2.111
    ```

    _**Resultados:**_   

    ```
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    ```
    
    ```PowerShell   
    Test-NetConnection 192.168.2.222
    ```

    _**Resultados:**_   

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    ```
12. Establezca el nombre, nombre del conmutador y etiquetas de prioridad.   

    ```PowerShell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**Resultados:**_   
    
    ```
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On 
    Status  : {Ok}
   
    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    ```

13. Ver las propiedades del adaptador de red vEthernet.
    
    ```PowerShell
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    ```

    _**Resultados:**_   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    ```

14. Habilitar a los adaptadores de red vEthernet.  
    
    ```PowerShell
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    ```

    _**Resultados:**_   
    
    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>Paso 10. Validar la funcionalidad RDMA.

Desea validar la funcionalidad RDMA desde el sistema remoto al sistema local, que tiene un vSwitch en ambos miembros del equipo del conjunto de vSwitch.<p>Dado que ambos VNIC de Host \(bloque de mensajes 1 y SMB2\) se asignan a 102 de VLAN, puede seleccionar el adaptador de 102 de VLAN en el sistema remoto. <p>En este ejemplo, la prueba-40G-2 de NIC hace RDMA al bloque de mensajes 1 (192.168.2.111) y SMB2 (192.168.2.222).

>[!TIP]
>Es posible que deba deshabilitar el Firewall en este sistema.  Para obtener más información, consulte la directiva de fabric.
>
>```PowerShell
>Set-NetFirewallProfile -All -Enabled False
>    
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**Resultados:**_ 
>   
>```
>Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
>----  -----------------------   --------------- -------------  
> .
> .
>Test-40G-2VLAN ID102VlanID  {102} 
>```
    
1. Ver las propiedades de adaptador de red.

   ```PowerShell
   Get-NetAdapter
   ```
    
   _**Resultados:**_ 
    
   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```
   
2. Ver la información de RDMA de adaptador de red.

   ```PowerShell
   Get-NetAdapterRdma
   ```
    
   _**Resultados:**_  
    
   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
   ```

3. Realizar la prueba de tráfico RDMA en el primer adaptador físico.

   ```PowerShell 
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Resultados:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter Test-40G-2 is a physical adapter
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.111, is reachable.
   VERBOSE: Remote IP 192.168.2.111 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 34251744 RDMA bytes sent per second
   VERBOSE: 967346308 RDMA bytes written per second
   VERBOSE: 35698177 RDMA bytes sent per second
   VERBOSE: 976601842 RDMA bytes written per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.111
   ```

4. Realizar la prueba de tráfico RDMA en el segundo adaptador físico.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Resultados:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter Test-40G-2 is a physical adapter
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.222, is reachable.
   VERBOSE: Remote IP 192.168.2.222 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 485137693 RDMA bytes written per second
   VERBOSE: 35200268 RDMA bytes sent per second
   VERBOSE: 939044611 RDMA bytes written per second
   VERBOSE: 34880901 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.222
   ```
    
5. Prueba para el tráfico RDMA desde el equipo local al equipo remoto.

    ```PowerShell
    Get-NetAdapter | ft –AutoSize
    ```
    
    _**Resultados:**_ 
    
    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. Realizar la prueba de tráfico RDMA en el primer adaptador virtual.    
    
   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Resultados:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter vEthernet (SMB1) is a virtual adapter
   VERBOSE: Retrieving vSwitch bound to the virtual adapter
   VERBOSE: Found vSwitch: VMSTEST
   VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
   VERBOSE: Remote IP 192.168.2.5 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 15250197 RDMA bytes sent per second
   VERBOSE: 896320913 RDMA bytes written per second
   VERBOSE: 33947559 RDMA bytes sent per second
   VERBOSE: 912160540 RDMA bytes written per second
   VERBOSE: 34091930 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
   ```

7. Realizar la prueba de tráfico RDMA en el segundo adaptador virtual.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Resultados:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
   VERBOSE: The adapter vEthernet (SMB2) is a virtual adapter
   VERBOSE: Retrieving vSwitch bound to the virtual adapter
   VERBOSE: Found vSwitch: VMSTEST
   VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
   VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
   VERBOSE: QoS/DCB/PFC configuration is correct.
   VERBOSE: RDMA configuration is correct.
   VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
   VERBOSE: Remote IP 192.168.2.5 is reachable.
   VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
   VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
   VERBOSE: 0 RDMA bytes written per second
   VERBOSE: 0 RDMA bytes sent per second
   VERBOSE: 385169487 RDMA bytes written per second
   VERBOSE: 33902277 RDMA bytes sent per second
   VERBOSE: 907354685 RDMA bytes written per second
   VERBOSE: 33923662 RDMA bytes sent per second
   VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
   VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
   ```
    
La última línea en este resultado, "prueba de tráfico RDMA correcto: Tráfico RDMA se envió a 192.168.2.5,"muestra que ha configurado correctamente convergente NIC en el adaptador.

## <a name="related-topics"></a>Temas relacionados 

- [Configuración de NIC convergentes con un único adaptador de red](cnic-single.md)
- [Configuración del conmutador físico de NIC convergente](cnic-app-switch-config.md)
- [Solución de problemas convergente configuraciones de NIC](cnic-app-troubleshoot.md)
