---
title: Configuración de NIC convergente NIC de equipo
description: Este tema proporciona instrucciones sobre cómo configurar el NIC convergido en una configuración de centro de datos en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ac6c2915301b1cf64486f24c197ebbf22bb5e2e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-teamed-nic-configuration"></a>Configuración de NIC convergente NIC de equipo

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Las siguientes secciones proporcionan instrucciones para implementar NIC convergido en una configuración de NIC asociado con el modificador incrustado agrupación \(SET\). Dos hosts de Hyper-V, Hyper-V Host 1 y 2 de Host de Hyper-V, muestra la configuración de ejemplo en esta guía.

## <a name="test-connectivity-between-source-and-destination"></a>Probar la conectividad entre el origen y de destino

Esta sección proporciona los pasos necesarios para probar la conectividad entre hosts de Hyper-V de origen y destino.

La ilustración siguiente muestra dos hosts de Hyper-V, **1 de Host de Hyper-V** y **2 de Host de Hyper-V**. 

Ambos hosts de Hyper-V tienen dos adaptadores de red. En cada host, un adaptador está conectado a la subred 192.168.1.x/24 y un adaptador está conectado a la subred 192.168.2.x/24.

![Hosts de Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>Probar la conectividad NIC al conmutador Virtual Hyper-V

Mediante el uso de este paso, puedes asegurarte de que la tarjeta física, para que crearás más adelante un conmutador Virtual Hyper-V, se puede conectar con el host de destino. 

Esta prueba muestra conectividad mediante \(L3\) Layer 3 - o la capa IP -, así como las redes de área local virtual de nivel 2 \(L2\) \(VLAN\).

Puedes usar el siguiente comando de Windows PowerShell para obtener las propiedades del adaptador de red.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
     
A continuación se muestran los resultados de ejemplo de este comando.

|Nombre|InterfaceDescription|ifIndex|Estado|Dirección MAC|Velocidad de vínculo|
|-----|--------------------|-------|-----|----------|---------|
|
|Prueba-40G-1|Adaptador Ethernet de Mellanox ConnectX 3 Pro|11|Arriba|E4-1D-2D-07-43-D0|40 GB/s|

Puedes usar uno de los siguientes comandos para obtener las propiedades del adaptador adicional, incluida la dirección IP.

    Get-NetIPAddress -InterfaceAlias "Test-40G-1"

    Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
    
A continuación se muestran ejemplos de resultados de estos comandos.

|Parámetro|Valor|
|---------|-----|
|
|Dirección IP| 192.168.1.3|
|ÍndiceDeInterfaz|11|
|InterfaceAlias|Prueba-40G-1|
|AddressFamily|IPv4|
|Tipo| Unidifusión|
|PrefixLength|24|

### <a name="verify-that-nic-team-member-has-a-valid-ip-address"></a>Comprueba que los miembros del equipo NIC tiene una dirección IP válida

Puedes usar este paso para comprobar que otro equipo NIC o cambiar de equipo Embedded \(SET\) miembro física NIC \(pNICs\) también tienen una dirección IP válida.

En este paso, puedes usar una subred independiente, \ (xxx.xxx.** 2**xxx vs xxx.xxx. **1**. xxx\), para facilitar el envío de este adaptador al destino. 

De lo contrario, si ambos pNICs que encuentres en la misma subred, la carga de la pila TCP/IP de Windows mantiene un excelente equilibrio entre las interfaces y validación simple se vuelve más complicado.

Puedes usar el siguiente comando de Windows PowerShell para obtener las propiedades del segundo adaptador de red.

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |InterfaceDescription |ifIndex |Estado |Dirección MAC |Velocidad de vínculo|
|----|--------------------|-------|------|----------|---------|
|
|PRUEBA-40G-2 |Mellanox ConnectX 3 Ethernet Pro r. … \ #2 |13 |Arriba |E4-1D-2D-07-40-70 |40 GB/s|

Puedes usar uno de los siguientes comandos para obtener las propiedades del adaptador adicional, incluida la dirección IP.

    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$\_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

A continuación se muestran ejemplos de resultados de estos comandos.

|Parámetro|Valor|
|---------|-----|
|
|Dirección IP|192.168.2.3|
|ÍndiceDeInterfaz|13|
|InterfaceAlias|PRUEBA-40G-2|
|AddressFamily|IPv4|
|Tipo|Unidifusión|
|PrefixLength|24|

### <a name="verify-that-additional-nics-have-valid-ip-addresses"></a>Compruebe que NIC adicionales tienen direcciones IP

Puedes usar este paso para garantizar que la otra NIC tiene una dirección IP válida.

En este paso, puedes usar una subred independiente, \ (xxx.xxx.** 2**xxx vs xxx.xxx. **1**. xxx\), para facilitar el envío de este adaptador al destino. 

De lo contrario, si ambos pNICs que encuentres en la misma subred, la carga de la pila TCP/IP de Windows mantiene un excelente equilibrio entre las interfaces y validación simple se vuelve más complicado.

Puedes usar el siguiente comando de Windows PowerShell para obtener las propiedades del segundo adaptador de red.

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |InterfaceDescription |ifIndex |Estado |Dirección MAC |Velocidad de vínculo|
|----|--------------------|-------|------|----------|---------|
|
|PRUEBA-40G-2 |Mellanox ConnectX 3 Ethernet Pro r. … \ #2 |13 |Arriba |E4-1D-2D-07-40-70 |40 GB/s|

Puedes usar uno de los siguientes comandos para obtener las propiedades del adaptador adicional, incluida la dirección IP.
    
    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

A continuación se muestran ejemplos de resultados de estos comandos.

|Parámetro|Valor|
|---------|-----|
|
|Dirección IP|192.168.2.3|
|ÍndiceDeInterfaz|13|
|InterfaceAlias|PRUEBA-40G-2|
|AddressFamily|IPv4|
|Tipo|Unidifusión|
|PrefixLength|24|

### <a name="ensure-that-source-and-destination-can-communicate"></a>Asegúrate de que puedan comunicarse origen y destino

Puedes usar este paso para comprobar la comunicación bidireccional \ (ping desde el origen al destino y viceversa en ambos sistemas con errores\).  En el siguiente ejemplo, el **prueba NetConnection** se usa el comando de Windows PowerShell, pero si lo prefieres, puedes usar la **ping** comando. 

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

### <a name="verify-connectivity-for-additional-nics"></a>Comprobar la conectividad adicionales NIC

Con este paso, puedes repetir los pasos anteriores para todos los pNICs posteriores que se incluyen en el equipo NIC o un conjunto.

Puedes usar el siguiente comando para probar la conexión.
    
    Test-NetConnection 192.168.2.5

A continuación se muestran los resultados de ejemplo de este comando.

|Parámetro|Valor|
|---------|-----|
|
|ComputerName|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|Prueba-40G-2|
|Dirección de origen|192.168.2.3|
|PingSucceeded|"False"|
|PingReplyDetails \(RTT\)|0 ms|


## <a name="configure-vlans"></a>Configurar VLAN

En este paso, el NIC están en **acceso** modo. Sin embargo cuando creas un \(vSwitch\) conmutador Virtual de Hyper-V más adelante en esta guía, se aplican a las propiedades VLAN en el nivel de puerto vSwitch. 

Dado que un modificador puede hospedar varias VLAN, es necesario para la parte superior de bastidor \(ToR\) interruptor tener su puerto configurado en el modo de enlace.

Consulte la documentación del conmutador de términos de referencia para obtener instrucciones sobre cómo configurar el modo de tronco en el conmutador.

La ilustración siguiente muestra dos hosts de Hyper-V con dos adaptadores de red cada tienen VLAN 101 y 102 VLAN configurado en Propiedades del adaptador de red.

![Configurar las redes de área local virtuales](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)

### <a name="configure-the-vlan-ids-for-both-nics"></a>Configurar los identificadores VLAN para ambos NIC

Puedes usar este paso para configurar los identificadores de VLAN para NIC instaladas en los hosts de Hyper-V.

Según el Instituto de electricidad y \(IEEE\) electrónica ingenieros estándares de red, las propiedades de \(QoS\) de calidad de servicio en la física NIC actuar en el encabezado de p 802.1X está incrustado en la 802.1Q \(VLAN\) encabezado al configurar el ID. VLAN

#### <a name="configure-nic-test-40g-1"></a>Configurar el NIC prueba-40G-1

Con los comandos siguientes y configurar el ID de VLAN para la primera NIC, prueba-40G-1, a continuación, ver la configuración resultante.

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    
A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |DisplayName| Valor de visualización| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|PRUEBA-40G-1|ID. DE VLAN|101|VlanID|{101}|


Asegúrate de que el identificador de VLAN surte efecto independiente de la implementación de adaptador de red mediante el siguiente comando para reiniciar el adaptador de red.

    Restart-NetAdapter -Name "Test-40G-1"

Puedes usar el siguiente comando para asegurarse de que el estado del adaptador de red es **arriba** antes de continuar.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre|InterfaceDescription|ifIndex| Estado|Dirección MAC|Velocidad de vínculo|
|----|--------------------|-------|------|----------| ---------|
|
|Prueba-40G-1|Ada Mellanox ConnectX 3 Pro Ethernet...|11|Arriba|E4-1D-2D-07-43-D0|40 GB/s|

#### <a name="configure-nic-test-40g-2"></a>Configurar el NIC prueba-40G-2

Con los comandos siguientes y configurar el ID de VLAN para la segunda NIC, prueba-40G-2, a continuación, ver la configuración resultante.

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |DisplayName| Valor de visualización| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|PRUEBA-40G-2|ID. DE VLAN|102|VlanID|{102}|

Asegúrate de que el identificador de VLAN surte efecto independiente de la implementación de adaptador de red mediante el siguiente comando para reiniciar el adaptador de red.

`Restart-NetAdapter -Name "Test-40G-2"  `              

Puedes usar el siguiente comando para asegurarse de que el estado del adaptador de red es **arriba** antes de continuar.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre|InterfaceDescription|ifIndex| Estado|Dirección MAC|Velocidad de vínculo|
|----|--------------------|-------|------|----------| ---------|
|
|Prueba-40G-2 |Ada Mellanox ConnectX 3 Pro Ethernet... |11 |Arriba |E4-1D-2D-07-43-D1 |40 GB/s|


### <a name="verify-connectivity"></a>Comprobar la conectividad

Puedes usar esta sección para comprobar la conectividad después de reinician los adaptadores de red. Puedes confirmar conectividad después de aplicar la etiqueta VLAN a ambos adaptadores. Si se produce un error de conectividad, puede inspeccionar el conmutador de participación de destino o la configuración de VLAN en la misma VLAN. 

>[!IMPORTANT]
>Después de realizar los pasos en la sección anterior, puede tardar varios segundos para el dispositivo para reiniciar y estén disponibles en la red.

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>Comprobar la conectividad de NIC prueba-40G-1

Para comprobar la conectividad para la primera NIC, puede ejecutar el comando siguiente.

    Test-NetConnection 192.168.1.5
    
A continuación se muestran los resultados de ejemplo de este comando.

|Parámetro|Valor|
|---------|-----|
|
|ComputerName|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|Prueba-40G-1|
|Dirección de origen|192.168.1.5|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0 ms|

#### <a name="verify-connectivity-for-nic-test-40g-2"></a>Comprobar la conectividad de NIC prueba-40G-2

En esta sección puedes usar para probar la conectividad de NIC prueba-40G-2.

Puedes usar el siguiente comando para probar la conectividad de la NIC de segundo.
    
    Test-NetConnection 192.168.2.5
    
A continuación se muestran los resultados de ejemplo de este comando.

|Parámetro|Valor|
|---------|-----|
|
|ComputerName|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|Prueba-40G-2|
|Dirección de origen|192.168.2.3|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0 ms|

Un **prueba NetConnection** fallo o comando "ping" que se produce inmediatamente después de realizar **reinicio AdaptadorRed** no es infrecuente que, por lo tanto, espere a que el adaptador de red inicializar completamente y, a continuación, vuelve a intentarlo.

Si las conexiones de 101 VLAN trabajar, pero las conexiones VLAN 102 no funcionan, el problema puede ser que el modificador debe configurarse para permitir el tráfico de puerto en la VLAN deseada. Puedes comprobar esto temporalmente estableciendo los adaptadores de producir un error a VLAN 101 y repetir la prueba de conectividad.

## <a name="configure-quality-of-service-qos"></a>Configurar la calidad de servicio \(QoS\)

El siguiente paso es configurar \(QoS\) calidad de servicio, lo que requiere que instalar primero la característica de Windows Server 2016 \(DCB\) puente de centro de datos.

La ilustración siguiente muestra los hosts de Hyper-V después de configurar correctamente VLAN en el paso anterior.

![Configurar la calidad de servicio](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)

### <a name="install-data-center-bridging-dcb"></a>Instalar el puente \(DCB\) el centro de datos

Puedes usar este paso para instalar y habilitar DCB. En la mayoría de los casos, este paso es opcional para las implementaciones de iWarp, pero es necesario en la escala fabric, por ejemplo, para escenarios de bastidor de entre.

Puedes usar el siguiente comando para instalar DCB en cada uno de los hosts de Hyper-V. 

    Install-WindowsFeature Data-Center-Bridging

A continuación se muestran los resultados de ejemplo de este comando.

|Éxito |Es necesitado el reinicio |Código de salida|Resultado de la característica|
|------- |-------------- |--------- |-------------- |
|
|True |No |Éxito| {Centro de datos puente}|

### <a name="set-the-qos-policies-for-smb-direct"></a>Establecer las directivas de QoS SMB Direct 

Puedes usar el siguiente comando para configurar las directivas de QoS para SMB directa.

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

### <a name="set-policies-for-other-traffic-on-the-interface"></a>Establecer directivas para el resto del tráfico en la interfaz 

Puedes usar el siguiente comando para establecer las directivas de QoS adicionales.

    New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
 
A continuación se muestran los resultados de ejemplo de este comando.

|Parámetro|Valor|
|---------|-----|
|
|Nombre | PREDETERMINADO|
|Propietario|\(Machine\) Directiva de grupo|
|NetworkProfile|Todos los|
|Prioridad|127|
|Plantilla| Predeterminado|
|JobObject| &nbsp;|
|PriorityValue|0|

### <a name="turn-on-flow-control-for-smb"></a>Activar el Control de flujo de SMB 

Puedes usar los siguientes comandos para habilitar el control de flujo SMB y ver los resultados.

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

A continuación se muestran los resultados de ejemplo de este comando.

|Prioridad|Habilitado|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |"False" |Global|&nbsp;|&nbsp;|
|1 |"False" |Global|&nbsp;|&nbsp;|
|2 |"False" |Global|&nbsp;|&nbsp;|
|3 |True |Global|&nbsp;|&nbsp;|
|4 |"False" |Global|&nbsp;|&nbsp;|
|5 |"False" |Global|&nbsp;|&nbsp;|
|6 |"False" |Global|&nbsp;|&nbsp;|
|7 |"False" |Global|&nbsp;|&nbsp;|

Si los resultados no coinciden con estos resultados porque los elementos que no sean 3 tienen un habilitado el valor True, debes realizar el paso siguiente. Si los resultados de coincidir con estos resultados de ejemplo y único elemento 3 tiene un valor True habilitado, no es necesario realizar el siguiente paso y puede omitir hacia abajo hasta **QoS habilitar para los adaptadores de red de destino y de destino**.

### <a name="ensure-flow-control-is-disabled-for-other-traffic-classes-optional"></a>Asegúrate de que el Control de flujo está deshabilitado para otras clases de tráfico \(Optional\)

No es necesario realizar este paso si **flujo** no está habilitado para clases de tráfico 0,1,2,4,5,6 y 7.

Si **flujo** ya está habilitado para todo el tráfico clases distinto 3 \ (0,1,2,4,5,6 y 7\), debes deshabilitar **flujo** para estas clases. 

>[!NOTE]
>En las configuraciones más complejas, otras clases de tráfico pueden requerir el control de flujo, sin embargo, estos escenarios están fuera del ámbito de esta guía.

Puedes usar los siguientes comandos para deshabilitar el control de flujo SMB clases de tráfico 0,1,2,4,5,6 y 7 y para ver los resultados.

    Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
    Get-NetQosFlowControl

A continuación se muestran los resultados de ejemplo de este comando.

|Prioridad|Habilitado|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |"False" |Global|&nbsp;|&nbsp;|
|1 |"False" |Global|&nbsp;|&nbsp;|
|2 |"False" |Global|&nbsp;|&nbsp;|
|3 |True |Global|&nbsp;|&nbsp;|
|4 |"False" |Global|&nbsp;|&nbsp;|
|5 |"False" |Global|&nbsp;|&nbsp;|
|6 |"False" |Global|&nbsp;|&nbsp;|
|7 |"False" |Global|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>Habilitar QoS para los adaptadores de red local y de destino 

Con este paso puede habilitar QoS para adaptadores de red local y de destino. Asegúrate de ejecutar estos comandos en ambos de los hosts de Hyper-V.

#### <a name="enable-qos-for-nic-test-40g-1"></a>Habilitar QoS para NIC prueba-40G-1

Puedes usar los siguientes comandos para habilitar QoS y ver los resultados de la configuración.

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
    Get-NetAdapterQos -Name "Test-40G-1"

A continuación se muestran los resultados de ejemplo de este comando.

**Nombre**: prueba-40G-1 **habilitado**: True **funcionalidades**:   

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
|0| Estricta|&nbsp;|0-7|

**OperationalFlowControl**: prioridad 3 habilitado **OperationalClassifications**:

|Protocolo|Tipo de puerto /|Prioridad|
|--------|---------|--------|
|
|Predeterminado|&nbsp;|0|
|NetDirect| 445|3|

#### <a name="enable-qos-for-nic-test-40g-2"></a>Habilitar QoS para NIC prueba-40G-2

Puedes usar los siguientes comandos para habilitar QoS y ver los resultados de la configuración.

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
    Get-NetAdapterQos -Name "Test-40G-2"

 
A continuación se muestran los resultados de ejemplo de este comando.

**Nombre**: prueba-40G-2 **habilitado**: True **funcionalidades**:   

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
|0| Estricta|&nbsp;|0-7|

**OperationalFlowControl**: prioridad 3 habilitado **OperationalClassifications**:

|Protocolo|Tipo de puerto /|Prioridad|
|--------|---------|--------|
|
|Predeterminado|&nbsp;|0|
|NetDirect| 445|3|


### <a name="allocate-50-of-the-bandwidth-reservation-to-smb-direct-rdma"></a>Asignar el 50% de la reserva de ancho de banda a SMB directa \(RDMA\)

Puedes usar los siguientes comandos para asignar la mitad de la reserva de ancho de banda a SMB directa.

    New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ESTABLECE     | 50 |3 |Global |&nbsp;|&nbsp;|                                      
 
Puedes usar el siguiente comando para ver información de reserva de ancho de banda.

    Get-NetQosTrafficClass | ft -AutoSize

A continuación se muestran los resultados de ejemplo de este comando.
 
|Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Default]| ESTABLECE|50 |0-2,4-7|  Global|&nbsp;|&nbsp;| 
SMB |ESTABLECE|50 |3 |Global|&nbsp;|&nbsp;| 

### <a name="create-traffic-classes-for-tenant-ip-traffic-optional"></a>Crear las clases de tráfico para el tráfico IP de inquilino \(optional\)

Puedes usar este paso para crear dos clases de tráfico adicional para el tráfico IP de inquilino. 

Cuando se ejecución los siguientes comandos de ejemplo, puedes omitir los valores "IP1" y "IP2" Si lo prefieres.

    New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP1 |ESTABLECE |10 |1 |Global|&nbsp;|&nbsp;|


    New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP2 |ESTABLECE |10 |2 |Global|&nbsp;|&nbsp;|

Puedes usar el siguiente comando para ver las clases de tráfico de QoS.

    Get-NetQosTrafficClass | ft -AutoSize

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre|Algoritmo |Bandwidth(%)| Prioridad |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Default] |ESTABLECE |30 |0,4-7 |Global|&nbsp;|&nbsp;|
|SMB |ESTABLECE |50 |3 |Global|&nbsp;|&nbsp;|
|IP1 |ESTABLECE |10 |1 |Global|&nbsp;|&nbsp;|
|IP2 |ESTABLECE |10 |2 |Global|&nbsp;|&nbsp;|

## <a name="configure-debugger-optional"></a>Configurar el depurador \(Optional\)

Puedes usar este paso para configurar al depurador.

De manera predeterminada, el depurador asociado bloquea NetQos. Puedes usar los siguientes comandos para invalidar al depurador y ver los resultados.

    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger

A continuación se muestran los resultados de ejemplo de este comando.

    AllowFlowControlUnderDebugger
    -----------------------------
    1

## <a name="test-rdma-mode-1"></a>Prueba RDMA \(Mode 1\)

Puedes usar este paso para garantizar que el tejido está configurado correctamente antes de crear un vSwitch y la transición al RDMA \(Mode 2\).

La ilustración siguiente muestra el estado actual de los hosts de Hyper-V.

![Prueba RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)

Para comprobar la configuración de RDMA, puede ejecutar el comando siguiente.

    Get-NetAdapterRdma | ft -AutoSize

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |InterfaceDescription |Habilitado|
|----|--------------------|-------|
|
|PRUEBA-40G-1| Adaptador Mellanox ConnectX 4 VPI #2 |True|
|PRUEBA-40G-2| Adaptador VPI Mellanox ConnectX-4 |True|


### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Determinar el valor de ifIndex de su adaptador de destino

Puedes usar el siguiente comando para detectar el valor ifIndex del adaptador de destino.

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

A continuación se muestran los resultados de ejemplo de este comando.

|InterfaceAlias |ÍndiceDeInterfaz |IPv4Address|
|-------------- |-------------- |-----------|
|PRUEBA-40G-1 |14 |{192.168.1.3}|
|PRUEBA-40G-2 | 13 |{192.168.2.3}|

### <a name="download-diskspdexe-and-a-powershell-script"></a>Descargar un Script de PowerShell y DiskSpd.exe

Para continuar, primero debes descargar los siguientes elementos.

- Descarga la utilidad DiskSpd.exe y extrae la utilidad en C:\TEST\[http://tinyurl.com/z68h3rc](http://tinyurl.com/z68h3rc)

- Descargar el script de powershell Test-RDMA en C:\TEST\[https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="run-the-powershell-script"></a>Ejecutar el script de PowerShell

Cuando ejecutas la. ps1 de Test-Rdma script de Windows PowerShell, puede pasar el valor de ifIndex al script, junto con la dirección IP del adaptador remoto en la misma VLAN.

Puedes usar el siguiente comando de ejemplo para ejecutar el script con una ifIndex de 14 del adaptador de red 192.168.1.5.
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter TEST-40G-1 is a physical adapter
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

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Determinar el valor de ifIndex de su adaptador de destino

Puedes usar el siguiente comando para detectar el valor ifIndex del adaptador de destino.

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

A continuación se muestran los resultados de ejemplo de este comando.

|InterfaceAlias |ÍndiceDeInterfaz |IPv4Address|
|-------------- |-------------- |-----------|
|PRUEBA-40G-1 |14 |{192.168.1.3}|
|PRUEBA-40G-2 | 13 |{192.168.2.3}|

Puedes usar el siguiente comando de ejemplo para ejecutar el script con una ifIndex de 13 años en el adaptador de red 192.168.2.5.

    
    C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    

## <a name="create-a-hyper-v-virtual-switch"></a>Crear un conmutador Virtual de Hyper-V

En esta sección puedes usar para crear un \(vSwitch\) conmutador Virtual de Hyper-V en los hosts de Hyper-V.

La ilustración siguiente muestra el 1 de Host de Hyper-V con un vSwitch.

![Crear un conmutador Virtual de Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

### <a name="create-a-vswitch-in-switch-embedded-teaming-set-mode"></a>Crear un vSwitch en modo \(SET\) conmutador incrustado agrupación

Puedes usar el siguiente comando de ejemplo para crear un equipo independiente del conmutador conjunto denominado **VMSTEST** en el Host de Hyper-V 1. Tanto los adaptadores de red en el equipo, prueba-40G-1 y prueba-40G-2, se agregan al equipo en conjunto con este comando.
    
    New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
    
A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |Externo |Interfaz asociado|

Puedes usar este comando para ver el equipo del adaptador físico en conjunto.

    Get-VMSwitchTeam -Name "VMSTEST" | fl

A continuación se muestran los resultados de ejemplo de este comando.

**Nombre**: VMSTEST **Id**: ad9bb542-dda2-4450-a00e-f96d44bdfbec **NetAdapterInterfaceDescription**: {adaptador Ethernet de Mellanox ConnectX 3 Pro, adaptador Ethernet de Mellanox ConnectX 3 Pro #2} **TeamingMode**: SwitchIndependent **LoadBalancingAlgorithm**: dinámico

#### <a name="display-two-views-of-the-host-vnic"></a>Mostrar dos vistas de la vNIC Host

Puedes usar los siguientes dos comandos para las dos vistas distintas de la vNIC Host.

Puedes usar este comando para mostrar las propiedades de la vNIC Host.

    Get-NetAdapter

A continuación se muestran los resultados de ejemplo de este comando.

| Nombre | InterfaceDescription | ifIndex | Estado | Dirección MAC | Velocidad de vínculo | |------|---|---|---|---| | | vEthernet (VMSTEST) | Hyper-V Adaptador Ethernet virtual #2 | 28 | Arriba | E4-1D-2D-07-40-71|80 GB/s |

Puedes usar este comando para mostrar las propiedades adicionales de la vNIC Host.

    Get-VMNetworkAdapter -ManagementOS

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |IsManagementOs |VMName |SwitchName |Dirección MAC |Estado |DireccionesIP|
|----|--------------|------|----------|----------|------|-----------|
|
|VMSTEST|True |VMSTEST |E41D2D074071| {Aceptar}|&nbsp;|

#### <a name="test-the-network-connection-to-the-remote-vlan-101-adapter"></a>Probar la conexión de red para el adaptador de 101 VLAN remoto

Puedes usar este comando para probar la conexión con el adaptador de 101 VLAN remoto.

`Test-NetConnection 192.168.1.5` 

A continuación se muestran los resultados de ejemplo de este comando.

    WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 10.199.48.170
    PingSucceeded  : False
    PingReplyDetails (RTT) : 0 ms
    
### <a name="reconfigure-vlans"></a>Reconfigurar VLAN

Puedes usar este paso para quitar la configuración de acceso VLAN desde la NIC física y para establecer el VLANID usando el vSwitch.

Debes quitar la configuración de acceso VLAN para evitar que ambas etiquetado automático el tráfico de salida con el identificador de VLAN incorrecta y de entrada de filtrado de tráfico que no coincide con el identificador de VLAN de acceso.

Puedes usar los siguientes comandos para quitar la configuración de acceso VLAN.
    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
    

Puedes usar los siguientes comandos para establecer el VLANID mediante los comandos de Windows PowerShell vSwitch específicos y para ver los resultados de esta acción.

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

A continuación se muestran los resultados de ejemplo de este comando.

VMName VMNetworkAdapterName modo VlanList
------ -------------------- ----   --------
       VMSTEST              Access 101     

Puedes usar el siguiente comando para probar la conexión de red y ver los resultados.

    Test-NetConnection 192.168.1.5
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

Si los resultados no son similares a los resultados de ejemplo y se produce un error en el comando "ping" con el mensaje "Advertencia: no se pudo realizar Ping a 192.168.1.5: estado: DestinationHostUnreachable," para confirmar que el "vEthernet (VMSTEST)" contiene la dirección IP correcta ejecutando el siguiente comando.

    
    Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
    

Si no se establece la dirección IP, puedes usar el siguiente comando para corregir el problema y ver los resultados de las acciones.

    
    New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
    
    IPAddress : 192.168.1.3
    InterfaceIndex: 37
    InterfaceAlias: vEthernet (VMSTEST)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Tentative
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : ActiveStore
    
#### <a name="rename-the-management-nic"></a>Cambiar el nombre de la administración NIC

Puedes usar esta sección para cambiar el nombre de la NIC de administración y luego usar diferentes Host vNIC instancias para RDMA.

Puedes ejecutar los siguientes comandos para cambiar el nombre de la NIC de administración y ver algunas propiedades NIC.

    Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
    Get-VMNetworkAdapter -ManagementOS

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |IsManagementOs |VMName |SwitchName |Dirección MAC |Estado |DireccionesIP
|----|--------------|------|----------|----------|------|-----------|
|
|Conmutador de CORP externo |True |&nbsp;|Conmutador de CORP externo |001B785768AA |{Aceptar}|&nbsp;|
|AVANZADA |True |&nbsp;|VMSTEST |E41D2D074071 |{Aceptar}|&nbsp;|

Puede ejecutar el comando siguiente para ver algunas propiedades NIC adicionales.

    Get-NetAdapter

A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |InterfaceDescription |ifIndex |Estado |Dirección MAC |Velocidad de vínculo|
|----|--------------------|------|----------|---------|------|
|
|vEthernet (avanzada) |Adaptador Ethernet virtuales de Hyper-V #2 |28 |Arriba | E4-1D-2D-07-40-71 |80 GB/s|

## <a name="test-hyper-v-virtual-switch-rdma"></a>Prueba el conmutador Virtual de Hyper-V RDMA

La ilustración siguiente muestra el estado actual de los hosts de Hyper-V, incluidos el vSwitch en el Host de Hyper-V 1.

![Prueba el conmutador Virtual Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

### <a name="set-priority-tagging-on-the-host-vnic-to-complement-the-previous-vlan-settings"></a>Establecer la prioridad de etiquetado de la vNIC Host para complementar la configuración de VLAN anterior

Puedes usar los siguientes comandos de ejemplo para establecer la prioridad de etiquetado de la vNIC Host y ver los resultados de las acciones.
    
    Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
    

    Name: MGT
    IeeePriorityTag : On
    
### <a name="create-two-host-vnics-for-rdma"></a>Crear dos vNICs Host para RDMA

Puedes usar los siguientes comandos de ejemplo para crear dos vNICs host para RDMA y conectarlos al vSwitch VMSTEST.
    
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
    
Puedes usar el siguiente comando de ejemplo para ver las propiedades de la NIC de administración.
    
    Get-VMNetworkAdapter -ManagementOS
    
A continuación se muestran los resultados de ejemplo de este comando.

|Nombre |IsManagementOs |VMName |SwitchName |Dirección MAC |Estado |DireccionesIP|
|----|--------------|------|----------|----------|------|-----------|
|
|Conmutador de CORP externo |True |Conmutador de CORP externo |001B785768AA|{Aceptar} |&nbsp;| 
|Avanzada |True |VMSTEST |E41D2D074071 |{Aceptar} |&nbsp;|
|PROTOCOLOS SMB1 |True |VMSTEST |00155D30AA00 |{Aceptar} |&nbsp;|
|SMB2 |True |VMSTEST |00155D30AA01 |{Aceptar} |&nbsp;|

### <a name="assign-an-ip-address-to-the-smb-host-vnics"></a>Asignar una dirección IP para el vNICs Host SMB

Puedes usar esta sección para asignar IP addressrd a la vEthernet vNICs Host SMB \(SMB1\) y vEthernet \(SMB2\).

La prueba-40G-1 y adaptadores de prueba-40G-2 físicos aún tienen una VLAN de acceso de 101 y 102 configurado. Por este motivo, los adaptadores de etiquetar el tráfico - y comando "ping" se realiza correctamente.

Anteriormente, había configurado ambos identificadores de VLAN pNIC a cero y establece el vSwitch VMSTEST en VLAN 101. Después, sigue podía hacer ping el adaptador VLAN 101 remoto mediante la vNIC avanzada, pero actualmente no hay VLAN 102 participantes.

Ahora puedes usar el siguiente comando de ejemplo para quitar la configuración de acceso VLAN desde la NIC física para impedir que ambas etiquetado automático el tráfico de salida con el identificador de VLAN incorrecta y para evitar que el filtrado de tráfico de entrada no coincide con el identificador de VLAN de acceso.

Puedes usar el siguiente comando de ejemplo para alcanzar estos objetivos mediante la adición de una nueva dirección IP para una interfaz vEthernet (protocolos SMB1) y ver los resultados.

    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
    
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
    

Puedes usar el siguiente comando de ejemplo para probar el adaptador VLAN 102 remoto y ver los resultados.
    
    Test-NetConnection 192.168.2.5 
    
    ComputerName   : 192.168.2.5
    RemoteAddress  : 192.168.2.5
    InterfaceAlias : vEthernet (SMB1)
    SourceAddress  : 192.168.2.111
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
Puedes usar el siguiente comando de ejemplo para agregar una nueva dirección IP para la interfaz vEthernet \(SMB2\) y ver los resultados.
    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
    
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
    
No es necesario probar la conexión porque se establece la comunicación.

### <a name="place-the-rdma-host-vnics-on-vlan-102"></a>Coloca el vNICs RDMA Host en VLAN 102

Puedes usar esta sección para asignar el vNICs RDMA Host a VLAN 102.

Como la vNIC "Avanzada" Host se encuentra en VLAN 101, puedes usar los siguientes comandos de ejemplo para colocar el vNICs RDMA Host en el 102 VLAN ya existentes y ver los resultados de las acciones.

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS
    
    Get-VMNetworkAdapterVlan -ManagementOS
    
    VMName VMNetworkAdapterName Mode VlanList
    ------ -------------------- ---- --------
       SMB1 Access   102 
       Mgt  Access   101 
       SMB2 Access   102 
       CORP-External-Switch Untagged
    
### <a name="inspect-the-mapping-of-smb1-and-smb2-to-the-underlying-physical-nics"></a>Inspeccionar la asignación de protocolos SMB1 y SMB2a la NIC físicas subyacentes

Puedes usar esta sección para inspeccionar la asignación de los protocolos SMB1 y SMB2a la NIC físicas subyacentes en el equipo establecer vSwitch.  La asociación de Host vNIC a NIC físicas es aleatorio y sujeto a volver a equilibrar durante la creación y destrucción.

En este caso, puedes usar un mecanismo indirecto para comprobar la asociación actual.

Las direcciones MAC de los protocolos SMB1 y SMB2 se asocian con los miembros del equipo de NIC prueba-40G-2. Esto no es lo ideal porque no tiene un asociado vNIC Host SMB prueba-40G-1 y no permitirá que el uso de tráfico RDMA sobre el vínculo hasta que se le asigna una vNIC Host SMB.

Puedes usar los siguientes comandos de ejemplo para ver esta información.

    
    Get-NetAdapterVPort (Preferred)
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
    
    Get-VMNetworkAdapter -ManagementOS
    
    Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
    ---- -------------- ------ ----------   ----------   ------ -----------
    CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
    Mgt  True  VMSTEST  E41D2D074071 {Ok}  
    SMB1 True  VMSTEST  00155D30AA00 {Ok}  
    SMB2 True  VMSTEST  00155D30AA01 {Ok}  
    

Los siguientes comandos no deben devolver ninguna información porque no se ha realizado asignación.
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
    
Puedes usar los siguientes comandos de ejemplo para asignar los protocolos SMB1 y SMB2 para separar a los miembros del equipo NIC físicos y ver los resultados de las acciones.

>[!IMPORTANT]
>Asegúrate de que haya completado este paso antes de continuar, o se producirá un error en la implementación.
    
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
    
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
    
### <a name="confirm-the-mac-associations"></a>Confirma las asociaciones de MAC

Puedes usar los siguientes comandos de ejemplo para confirmar las asociaciones de MAC que hayas creado anteriormente.
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    

Dado que ambos vNICs Host residen en la misma subred y tienen el mismo \(102\) VLAN ID, puede validar la comunicación del sistema remoto y ver los resultados de las acciones.

>[!IMPORTANT]
>Ejecuta los siguientes comandos en el equipo remoto.

    
    Test-NetConnection 192.168.2.111
    
    
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
    Test-NetConnection 192.168.2.222
    
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    
        
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    
    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    

    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    
    
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
      

### <a name="validate-rdma-functionality-from-the-remote-system"></a>Validar la funcionalidad RDMA desde el sistema remoto

Puedes usar esta sección para validar la funcionalidad RDMA desde el sistema remoto en el sistema local, que tiene un vSwitch, con lo que se prueba ambos adaptadores que son miembros del equipo de conjunto vSwitch.

Dado que ambos vNICs Host \ (protocolos SMB1 y SMB2\) se asignan a VLAN 102, puedes seleccionar el adaptador VLAN 102 en el sistema remoto.  

En este proceso, el NIC 40G-Test-2 no RDMA a los protocolos SMB1 (192.168.2.111) y SMB2 (192.168.2.222).

Opcional: Es posible que debas deshabilitar el Firewall en el sistema.  Para obtener más información, consulte la directiva de fabric.

    
    Set-NetFirewallProfile -All -Enabled False
    
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
    
    Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
    ----  -----------------------   --------------- -------------  
    .
    .
    Test-40G-2VLAN ID102VlanID  {102} 
    
    Get-NetAdapter
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
    
    
    Get-NetAdapterRdma
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
### <a name="test-for-rdma-traffic-from-the-local-to-the-remote-computer"></a>Prueba de tráfico RDMA desde el equipo local al equipo remoto

En esta sección puedes comprobar que el tráfico RDMA se envía desde el equipo local al equipo remoto.

Puedes usar los siguientes comandos de ejemplo para probar y ver el flujo de tráfico.

    
    Get-NetAdapter | ft –AutoSize
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
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
    
La última línea en esta salida, "prueba de tráfico RDMA correcto: tráfico RDMA se envió a 192.168.2.5," se muestra correctamente configuradas NIC convergido en el adaptador.

## <a name="all-topics-in-this-guide"></a>Todos los temas de esta guía

Esta guía contiene los siguientes temas.

- [Configuración de NIC convergente con un adaptador de red](cnic-single.md)
- [Configuración de NIC convergente NIC de equipo](cnic-datacenter.md)
- [Configuración del conmutador físico para convergente NIC](cnic-app-switch-config.md)
- [Solución de problemas convergido configuraciones NIC](cnic-app-troubleshoot.md)
