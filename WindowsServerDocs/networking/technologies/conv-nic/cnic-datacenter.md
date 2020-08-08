---
title: NIC convergente en una configuración de NIC en equipo (centro de trabajo)
description: En este tema, se proporcionan instrucciones para implementar la NIC convergente en una configuración de NIC en equipo con switch Embedded Teaming (SET).
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/17/2018
ms.openlocfilehash: 918b3d10c39c6f06330f9c0986bc08b5bc04a229
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949379"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>NIC convergente en una configuración de NIC en equipo (centro de trabajo)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, se proporcionan instrucciones para implementar la NIC convergente en una configuración de NIC en equipo con switch Embedded Teaming \( establecido \) .

La configuración de ejemplo de este tema describe dos hosts de Hyper-v, el **host 1 de Hyper-v** y el **host 2 de Hyper-v**. Ambos hosts tienen dos adaptadores de red. En cada host, un adaptador se conecta a la subred 192.168.1. x/24 y un adaptador se conecta a la subred 192.168.2. x/24.

![Hosts de Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Paso 1. Probar la conectividad entre el origen y el destino

Asegúrese de que la NIC física pueda conectarse al host de destino.  Esta prueba muestra la conectividad mediante el uso de la capa 3 \( L3 \) o el nivel de IP, así como \( las \) redes VLAN de red de área local virtual de nivel 2 \( \) .

1. Vea las propiedades del primer adaptador de red.

   ```powershell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Resultados**_


   |    Nombre    |           InterfaceDescription           | ifIndex | Estado |    MacAddress     | LinkSpeed |
   |------------|------------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-1 | Adaptador Ethernet ConnectX-3 Pro de Mellanox |   11    |   Arriba   | E4-1D-2D-07-43-D0 |  40 Gbps  |

2. Vea las propiedades adicionales del primer adaptador, incluida la dirección IP.

   ```powershell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**Resultados**_


   |   Parámetro    |    Valor    |
   |----------------|-------------|
   |   IPAddress    | 192.168.1.3 |
   | InterfaceIndex |     11      |
   | InterfaceAlias | Test-40G-1  |
   | AddressFamily  |    IPv4     |
   |      Tipo      |   Unidifusión   |
   |  PrefixLength  |     24      |

3. Vea las propiedades del segundo adaptador de red.

   ```powershell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```

   _**Resultados**_


   |    Nombre    |          InterfaceDescription           | ifIndex | Estado |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | TEST-40G-2 | Ethernet Mellanox ConnectX-3 Pro A... #2 |   13    |   Arriba   | E4-1D-2D-07-40-70 |  40 Gbps  |

4. Vea las propiedades adicionales del segundo adaptador, incluida la dirección IP.

   ```powershell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**Resultados**_


   |   Parámetro    |    Valor    |
   |----------------|-------------|
   |   IPAddress    | 192.168.2.3 |
   | InterfaceIndex |     13      |
   | InterfaceAlias | TEST-40G-2  |
   | AddressFamily  |    IPv4     |
   |      Tipo      |   Unidifusión   |
   |  PrefixLength  |     24      |

5. Compruebe que el equipo de la NIC o el miembro del conjunto pNICs tiene una dirección IP válida.<p>Use una subred independiente, \( xxx.xxx.** 2**. xxx frente a xxx.xxx. **1**. xxx \) , para facilitar el envío desde este adaptador al destino. De lo contrario, si encuentra pNICs en la misma subred, los equilibrios de carga de la pila TCP/IP de Windows entre las interfaces y la validación simple se vuelven más complicados.


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Paso 2. Asegurarse de que el origen y el destino pueden comunicarse

En este paso, usamos el comando **Test-NetConnection de** Windows PowerShell, pero si lo prefiere, puede usar el comando **ping** .

1. Compruebe la comunicación bidireccional.

   ```powershell
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
   | \(RTT PingReplyDetails\) |    0 ms     |

   En algunos casos, es posible que tenga que deshabilitar Firewall de Windows con seguridad avanzada para realizar correctamente esta prueba. Si deshabilita el firewall, tenga en cuenta la seguridad y asegúrese de que la configuración cumple los requisitos de seguridad de su organización.

2. Deshabilitar todos los perfiles de Firewall.

   ```powershell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. Después de deshabilitar los perfiles de firewall, vuelva a probar la conexión.

   ```powershell
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
   | \(RTT PingReplyDetails\) |    0 ms     |


4. Compruebe la conectividad de NIC adicionales. Repita los pasos anteriores para todas las pNICs subsiguientes incluidas en la NIC o establezca el equipo.

   ```powershell
   Test-NetConnection 192.168.2.5
   ```

   _**Resultados**_


   |        Parámetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | Test-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    False    |
   | \(RTT PingReplyDetails\) |    0 ms     |

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Paso 3. Configurar los identificadores de VLAN para las NIC instaladas en los hosts de Hyper-V

Muchas configuraciones de red hacen uso de redes VLAN y, si tiene previsto usar VLAN en la red, debe repetir la prueba anterior con VLAN configuradas.

En este paso, las NIC están en modo de **acceso** . Sin embargo, cuando se crea un vSwitch de conmutador virtual de Hyper-V \( \) más adelante en esta guía, las propiedades de la VLAN se aplican en el nivel de Puerto vSwitch.

Dado que un conmutador puede hospedar varias VLAN, es necesario para la parte superior del \( conmutador físico del bastidor Tor \) para que el puerto al que está conectado el host esté configurado en el modo de tronco.

>[!NOTE]
>Consulte la documentación del conmutador ToR para obtener instrucciones sobre cómo configurar el modo de tronco en el conmutador.

En la imagen siguiente se muestran dos hosts de Hyper-V con dos adaptadores de red que tienen configurada la VLAN 101 y VLAN 102 en las propiedades del adaptador de red.

![Configuración de redes de área local virtuales](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>Según el Instituto de ingenieros eléctricos y electrónicos \( estándar de \) redes IEEE, las propiedades QoS de calidad de servicio \( \) de la NIC física actúan en el encabezado p de 802.1 que se inserta en el encabezado 802.1 q de la \( VLAN \) al configurar el identificador de VLAN.

1. Configure el identificador de VLAN en la primera NIC, test-40G-1.

   ```powershell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**Resultados**_


   |    Nombre    | DisplayName | DisplayValue | RegistryKeyword | Deleteinstance |
   |------------|-------------|--------------|-----------------|---------------|
   | TEST-40G-1 |   ID. DE VLAN   |     101      |     VlanID      |     {101}     |

2. Reinicie el adaptador de red para aplicar el ID. de VLAN.

   ```powershell
   Restart-NetAdapter -Name "Test-40G-1"
   ```

3. Asegúrese de que el estado es **activo**.

   ```powershell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Resultados**_


   |    Nombre    |          InterfaceDescription           | ifIndex | Estado |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-1 | Adaptador Mellanox ConnectX-3 Pro Ethernet ADA... |   11    |   Arriba   | E4-1D-2D-07-43-D0 |  40 Gbps  |

4. Configure el identificador de VLAN en la segunda NIC, test-40G-2.

   ```powershell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**Resultados**_


   |    Nombre    | DisplayName | DisplayValue | RegistryKeyword | Deleteinstance |
   |------------|-------------|--------------|-----------------|---------------|
   | TEST-40G-2 |   ID. DE VLAN   |     102      |     VlanID      |     {102}     |

5. Reinicie el adaptador de red para aplicar el ID. de VLAN.

   ```powershell
   Restart-NetAdapter -Name "Test-40G-2"
   ```

6. Asegúrese de que el estado es **activo**.

   ```powershell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Resultados**_


   |    Nombre    |          InterfaceDescription           | ifIndex | Estado |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-2 | Adaptador Mellanox ConnectX-3 Pro Ethernet ADA... |   11    |   Arriba   | E4-1D-2D-07-43-D1 |  40 Gbps  |

   >[!IMPORTANT]
   >El dispositivo puede tardar varios segundos en reiniciarse y estar disponible en la red.

7. Compruebe la conectividad de la primera NIC, test-40G-1.<p>Si se produce un error de conectividad, revise la configuración de VLAN del conmutador o la participación de destino en la misma VLAN.

   ```powershell
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados**_


   |        Parámetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.5 |
   |      PingSucceeded       |    True     |
   | \(RTT PingReplyDetails\) |    0 ms     |

8. Compruebe la conectividad de la primera NIC, test-40G-2.<p>Si se produce un error de conectividad, revise la configuración de VLAN del conmutador o la participación de destino en la misma VLAN.

   ```powershell
   Test-NetConnection 192.168.2.5
   ```

   _**Resultados**_


   |        Parámetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      InterfaceAlias      | Test-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    True     |
   | \(RTT PingReplyDetails\) |    0 ms     |

   >[!IMPORTANT]
   >No es raro que se produzca un error de **prueba-NetConnection** o ping inmediatamente después de que se realice **el reinicio NetAdapter**.  Espere a que el adaptador de red se inicialice por completo e inténtelo de nuevo.
   >
   >Si las conexiones VLAN 101 funcionan, pero las conexiones VLAN 102 no, el problema podría ser que el conmutador se debe configurar para permitir el tráfico de puerto en la VLAN deseada. Puede comprobarlo si establece temporalmente los adaptadores con errores en VLAN 101 y repite la prueba de conectividad.


   En la imagen siguiente se muestran los hosts de Hyper-V después de configurar correctamente las VLAN.

   ![Configurar la calidad de servicio](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)


## <a name="step-4-configure-quality-of-service-qos"></a>Paso 4. Configurar QoS de calidad de servicio \(\)

>[!NOTE]
>Debe realizar todos los siguientes pasos de configuración de DCB y QoS en todos los hosts que estén diseñados para comunicarse entre sí.

1. Instale el DCB de puente del centro de datos \( \) en cada uno de los hosts de Hyper-V.

   - **Opcional** para las configuraciones de red que usan iWarp.
   - Se **requiere** para las configuraciones de red que usan RoCE \( cualquier versión \) para los servicios RDMA.

   ```powershell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**Resultados**_


   | Correcto | Reinicio necesario | Código de salida |     Resultado de la característica     |
   |---------|----------------|-----------|------------------------|
   |  True   |       No       |  Correcto  | {Protocolo de puente del centro de datos} |

2. Establezca las directivas de QoS para SMB-Direct:

   - **Opcional** para las configuraciones de red que usan iWarp.
   - Se **requiere** para las configuraciones de red que usan RoCE \( cualquier versión \) para los servicios RDMA.

   En el siguiente comando de ejemplo, el valor "3" es arbitrario. Puede usar cualquier valor entre 1 y 7 siempre que use el mismo valor de forma coherente en toda la configuración de las directivas de QoS.

   ```powershell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**Resultados**_


   |   Parámetro    |          Valor           |
   |----------------|--------------------------|
   |      Nombre      |           SMB            |
   |     Propietario      | \(Máquina Directiva de grupo\) |
   | NetworkProfile |           Todo            |
   |   Prioridad   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

3. Establezca directivas de QoS adicionales para otro tráfico en la interfaz.

   ```powershell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**Resultados**_


   |   Parámetro    |          Valor           |
   |----------------|--------------------------|
   |      Nombre      |         DEFAULT          |
   |     Propietario      | \(Máquina Directiva de grupo\) |
   | NetworkProfile |           Todo            |
   |   Prioridad   |           127            |
   |    Plantilla    |         Valor predeterminado          |
   |   JobObject    |          &nbsp;          |
   | PriorityValue  |            0             |

4. Active el **control de flujo de prioridad** para el tráfico SMB, que no es necesario para iWarp.

   ```powershell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**Resultados**_


   | Prioridad | Habilitado | PolicySet | IfIndex | IfAlias |
   |----------|---------|-----------|---------|---------|
   |    0     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    1     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    2     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    3     |  True   |  Global   | &nbsp;  | &nbsp;  |
   |    4     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    5     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    6     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    7     |  False  |  Global   | &nbsp;  | &nbsp;  |

   >**Importante** Si los resultados no coinciden con estos resultados porque los elementos que no son 3 tienen un valor habilitado de true, debe deshabilitar **FlowControl** para estas clases.
   >
   >```powershell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >En configuraciones más complejas, las otras clases de tráfico podrían requerir el control de flujo; sin embargo, estos escenarios están fuera del ámbito de esta guía.


5. Habilite QoS para la primera NIC, test-40G-1.

   ```powershell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1
   Enabled: True
   ```

   _**Capacidades**:_


   |      Parámetro      |   Hardware   |   Current    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     None     |     None     |
   | NumTCs (Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   _**OperationalTrafficClasses**:_


   | TC |  TSA   | Ancho de banda | Prioridades |
   |----|--------|-----------|------------|
   | 0  | Strict |  &nbsp;   |    0-7     |

   _**OperationalFlowControl**:_

   Prioridad 3 habilitada

   _**OperationalClassifications**:_


   | Protocolo  | Puerto/tipo | Prioridad |
   |-----------|-----------|----------|
   |  Valor predeterminado  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

6. Habilite QoS para la segunda NIC, test-40G-2.

   ```powershell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2
   Enabled: True
   ```

   _**Capacidades**:_


   |      Parámetro      |   Hardware   |   Current    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     None     |     None     |
   | NumTCs (Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   _**OperationalTrafficClasses**:_


   | TC |  TSA   | Ancho de banda | Prioridades |
   |----|--------|-----------|------------|
   | 0  | Strict |  &nbsp;   |    0-7     |

    _**OperationalFlowControl**:_

    Prioridad 3 habilitada

   _**OperationalClassifications**:_


   | Protocolo  | Puerto/tipo | Prioridad |
   |-----------|-----------|----------|
   |  Valor predeterminado  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |


7. Reserve la mitad del ancho de banda en RDMA directo de SMB \(\)

   ```powershell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```

   _**Resultados**_


   | Nombre | Algoritmo | Ancho de banda (%) | Prioridad | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      50      |    3     |  Global   | &nbsp;  | &nbsp;  |

8. Vea la configuración de reserva de ancho de banda:

   ```powershell
   Get-NetQosTrafficClass | ft -AutoSize
   ```

   _**Resultados**_

   |   Nombre    | Algoritmo | Ancho de banda (%) | Prioridad | PolicySet | IfIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | [Predeterminado] |    ETS    |      50      | 0:2, 4-7  |  Global   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      50      |    3     |  Global   | &nbsp;  | &nbsp;  |

9. Opta Cree dos clases de tráfico adicionales para el tráfico de IP de inquilino.

   >[!TIP]
   >Puede omitir los valores "IP1" y "IP2".

   ```powershell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**Resultados**_

   | Nombre | Algoritmo | Ancho de banda (%) | Prioridad | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP1  |    ETS    |      10      |    1     |  Global   | &nbsp;  | &nbsp;  |

   ```powershell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**Resultados**_


   | Nombre | Algoritmo | Ancho de banda (%) | Prioridad | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP2  |    ETS    |      10      |    2     |  Global   | &nbsp;  | &nbsp;  |

1.  Vea las clases de tráfico de QoS.

    ```powershell
    Get-NetQosTrafficClass | ft -AutoSize
    ```

    _**Resultados**_


    |   Nombre    | Algoritmo | Ancho de banda (%) | Prioridad | PolicySet | IfIndex | IfAlias |
    |-----------|-----------|--------------|----------|-----------|---------|---------|
    | [Predeterminado] |    ETS    |      30      |  0, 4-7   |  Global   | &nbsp;  | &nbsp;  |
    |    SMB    |    ETS    |      50      |    3     |  Global   | &nbsp;  | &nbsp;  |
    |    IP1    |    ETS    |      10      |    1     |  Global   | &nbsp;  | &nbsp;  |
    |    IP2    |    ETS    |      10      |    2     |  Global   | &nbsp;  | &nbsp;  |

    ---

2.  Opta Invalide el depurador.<p>De forma predeterminada, el depurador adjunto bloquea NetQos.

    ```powershell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**Resultados**_

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>Paso 5. Comprobar el modo de configuración de RDMA \( 1\)

Desea asegurarse de que el tejido esté configurado correctamente antes de crear un vSwitch y pasar al \( modo RDMA 2 \) .

En la imagen siguiente se muestra el estado actual de los hosts de Hyper-V.

![Probar RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. Compruebe la configuración de RDMA.

   ```powershell
   Get-NetAdapterRdma | ft -AutoSize
   ```

   _**Resultados**_


   |    Nombre    |        InterfaceDescription        | Habilitado |
   |------------|------------------------------------|---------|
   | TEST-40G-1 | Adaptador VPI ConnectX-4 de Mellanox #2 |  True   |
   | TEST-40G-2 |  Adaptador VPI ConnectX-4 de Mellanox   |  True   |

2. Determine **el valor** de la de los adaptadores de destino.<p>Este valor se usa en los pasos siguientes cuando se ejecuta el script que se descarga.

   ```powershell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**Resultados**_


   | InterfaceAlias | InterfaceIndex |  Dirección IPv4  |
   |----------------|----------------|---------------|
   |   TEST-40G-1   |       14       | {192.168.1.3} |
   |   TEST-40G-2   |       13       | {192.168.2.3} |

3. Descargue la [utilidadDiskSpd.exe](https://aka.ms/diskspd) y extráigalo en C:\test\.

4. Descargue el [script de PowerShell test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) en una carpeta de prueba de la unidad local, por ejemplo, C:\test\.

5. Ejecute el script de **Test-Rdma.ps1** PowerShell para pasar el valor de índice a la secuencia de comandos, junto con la dirección IP del primer adaptador remoto en la misma VLAN.<p>En este ejemplo **, el script pasa el valor de** índice de 14 en la dirección IP del adaptador de red remoto 192.168.1.5.

   ```powershell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Resultados**_

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
   >Si se produce un error en el tráfico RDMA, en el caso de RoCE, consulte la configuración del conmutador ToR para obtener una configuración correcta de PFC/ETS que debe coincidir con la configuración del host. Consulte la sección QoS de este documento para obtener los valores de referencia.

6. Ejecute el script de **Test-Rdma.ps1** PowerShell para pasar el valor de índice a la secuencia de comandos, junto con la dirección IP del segundo adaptador remoto en la misma VLAN.<p>En este ejemplo **, el script pasa el valor de** índice de 13 en la dirección IP del adaptador de red remoto 192.168.2.5.

   ```powershell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Resultados**_

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

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Paso 6. Creación de un vSwitch de Hyper-V en los hosts de Hyper-V


En la imagen siguiente se muestra el host 1 de Hyper-V con un vSwitch.

![Crear un conmutador virtual de Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. Cree un vSwitch en modo SET en el host 1 de Hyper-V.

   ```powershell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```

   _**Da**_


   |  Nombre   | Tipodeconmutador | Si |
   |---------|------------|--------------------------------|
   | VMSTEST |  Externo  |        Interfaz en equipo        |

2. Vea el equipo del adaptador físico en el conjunto.

   ```powershell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```

   _**Resultados**_

   ```
   Name: VMSTEST
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}
   TeamingMode: SwitchIndependent
   LoadBalancingAlgorithm: Dynamic
   ```

3. Mostrar dos vistas del host vNIC

   ```powershell
    Get-NetAdapter
   ```

   _**Resultados**_


   |        Nombre         |        InterfaceDescription         | ifIndex | Estado |    MacAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | Adaptador Ethernet Virtual de Hyper-V #2 |   28    |   Arriba   | E4-1D-2D-07-40-71 |  80 Gbps  |

4. Vea las propiedades adicionales del host vNIC.

   ```powershell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Resultados**_


   |  Nombre   | IsManagementOs | VMName  |  SwitchName  | MacAddress | Estado | IPAddresses |
   |---------|----------------|---------|--------------|------------|--------|-------------|
   | VMSTEST |      True      | VMSTEST | E41D2D074071 |    Aceptar    | &nbsp; |             |


5. Pruebe la conexión de red con el adaptador de VLAN 101 remoto.

   ```powershell
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados**_

   ```
   WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable

   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (CORP-External-Switch)
   SourceAddress  : 10.199.48.170
   PingSucceeded  : False
   PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-7-remove-the-access-vlan-setting"></a>Paso 7. Quitar la configuración de VLAN de acceso

En este paso, se quita la configuración de acceso de VLAN de la NIC física y se establece el valor de VLANID mediante el vSwitch.

Debe quitar la configuración de VLAN de acceso para evitar que el etiquetado automático del tráfico de salida con el ID. de VLAN incorrecto y del filtrado de tráfico de entrada no coincida con el identificador de VLAN de acceso.

1. Quite la configuración.

   ```powershell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
   ```

2. Establezca el identificador de VLAN.

   ```powershell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```

   _**Resultados**_

   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101
   ```


3. Pruebe la conexión de red.

   ```powershell
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

   >**Importante** Si los resultados no son similares a los resultados del ejemplo y se produce un error en el comando ping con el mensaje "WARNING: ping to 192.168.1.5 failed--status: DestinationHostUnreachable", confirme que "vEthernet (VMSTEST)" tiene la dirección IP correcta.
   >
   >```powershell
   >Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
   >```
   >
   >Si la dirección IP no está establecida, corrija el problema.
   >
   >```powershell
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


4. Cambie el nombre de la NIC de administración.

   ```powershell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Resultados**_


   |         Nombre         | IsManagementOs | VMName |      SwitchName      |  MacAddress  | Estado | IPAddresses |
   |----------------------|----------------|--------|----------------------|--------------|--------|-------------|
   | CORP-external-switch |      True      | &nbsp; | CORP-external-switch | 001B785768AA |  Aceptar  |   &nbsp;    |
   |         GESTIÓN          |      True      | &nbsp; |       VMSTEST        | E41D2D074071 |  Aceptar  |   &nbsp;    |

5. Ver propiedades adicionales de la NIC.

   ```powershell
   Get-NetAdapter
   ```

   _**Resultados**_


   |      Nombre       |        InterfaceDescription         | ifIndex | Estado |    MacAddress     | LinkSpeed |
   |-----------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (gestión) | Adaptador Ethernet Virtual de Hyper-V #2 |   28    |   Arriba   | E4-1D-2D-07-40-71 |  80 Gbps  |

## <a name="step-8-test-hyper-v-vswitch-rdma"></a>Paso 8. Probar RDMA de vSwitch de Hyper-V

En la imagen siguiente se muestra el estado actual de los hosts de Hyper-V, incluido el vSwitch en el host 1 de Hyper-V.

![Probar el conmutador virtual de Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. Establezca el etiquetado de prioridades en el host vNIC para complementar la configuración de VLAN anterior.

   ```powershell
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```

   _**Resultados**_

   Nombre: IeeePriorityTag: on

2. Cree dos VNIC de host para RDMA y conéctelas a la VMSTEST de vSwitch.

   ```powershell
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```

3. Ver las propiedades de la NIC de administración.

   ```powershell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Resultados**_


   |         Nombre         | IsManagementOs |        VMName        |  SwitchName  | MacAddress | Estado | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-external-switch |      True      | CORP-external-switch | 001B785768AA |    Aceptar    | &nbsp; |             |
   |         Gestión          |      True      |       VMSTEST        | E41D2D074071 |    Aceptar    | &nbsp; |             |
   |         SMB1         |      True      |       VMSTEST        | 00155D30AA00 |    Aceptar    | &nbsp; |             |
   |         SMB2         |      True      |       VMSTEST        | 00155D30AA01 |    Aceptar    | &nbsp; |             |

## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>Paso 9. Asignación de una dirección IP al host SMB VNIC vEthernet \( SMB1 \) y vEthernet \( SMB2\)

Los adaptadores físicos TEST-40G-1 y TEST-40G-2 todavía tienen configurada una VLAN de acceso de 101 y 102. Por este motivo, los adaptadores etiquetan el tráfico y el ping se realiza correctamente. Anteriormente, configuró ambos identificadores de VLAN de pNIC en cero y, a continuación, configure el vSwitch de VMSTEST en VLAN 101. Después, todavía podía hacer ping en el adaptador de VLAN 101 remoto mediante el vNIC de gestión, pero actualmente no hay ningún miembro de VLAN 102.



1. Quite la configuración de acceso de VLAN de la NIC física para impedir que se etiquete automáticamente el tráfico de salida con el identificador de VLAN incorrecto y para impedir que filtre el tráfico de entrada que no coincide con el identificador de VLAN de acceso.

   ```powershell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**Resultados**_

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

2. Pruebe el adaptador de VLAN 102 remoto.

   ```powershell
   Test-NetConnection 192.168.2.5
   ```

   _**Resultados**_

   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```

3. Agregue una nueva dirección IP para la interfaz vEthernet \( SMB2 \) .

   ```powershell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24
   ```

   _**Resultados**_

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

4. Vuelva a probar la conexión.


5. Coloque el host RDMA VNIC en la VLAN 102 ya existente.

   ```powershell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS

   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**Resultados**_

   ```
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102
      Mgt  Access   101
      SMB2 Access   102
      CORP-External-Switch Untagged
   ```

6. Inspeccione la asignación de SMB1 y SMB2 a las NIC físicas subyacentes en el equipo del conjunto de vSwitch.<p>La Asociación de host vNIC a NIC físicas es aleatoria y está sujeta a reequilibrio durante la creación y la destrucción. En este caso, puede usar un mecanismo indirecto para comprobar la Asociación actual. Las direcciones MAC de SMB1 y SMB2 están asociadas a la prueba de miembros del equipo NIC-40G-2. Esto no es lo ideal porque test-40G-1 no tiene un host SMB asociado vNIC y no permitirá el uso del tráfico RDMA a través del vínculo hasta que se le asigne un host SMB vNIC.

   ```powershell
   Get-NetAdapterVPort (Preferred)

   Get-NetAdapterVmqQueue
   ```

   _**Resultados**_

   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. Vea las propiedades del adaptador de red de la máquina virtual.

   ```powershell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Resultados**_

   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}
   Mgt  True  VMSTEST  E41D2D074071 {Ok}
   SMB1 True  VMSTEST  00155D30AA00 {Ok}
   SMB2 True  VMSTEST  00155D30AA01 {Ok}
   ```

8. Vea la asignación de equipo del adaptador de red.<p>Los resultados no deben devolver información porque todavía no ha realizado la asignación.

   ```powershell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```


9. Asigne SMB1 y SMB2 para separar los miembros del equipo NIC físico y ver los resultados de las acciones.

   >[!IMPORTANT]
   >Asegúrese de completar este paso antes de continuar o de que se produzca un error en la implementación.

   ```powershell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"

   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**Resultados**_

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

10. Confirme las asociaciones de MAC creadas anteriormente.

    ```powershell
    Get-NetAdapterVmqQueue
    ```

    _**Resultados**_

    ```
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. Pruebe la conexión desde el sistema remoto porque los dos hosts VNIC residen en la misma subred y tienen el mismo identificador de VLAN \( 102 \) .

    ```powershell
    Test-NetConnection 192.168.2.111
    ```

    _**Resultados**_

    ```
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    ```

    ```powershell
    Test-NetConnection 192.168.2.222
    ```

    _**Resultados**_

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    ```
12. Establezca el nombre, el nombre del conmutador y las etiquetas de prioridad.

    ```powershell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**Resultados**_

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

13. Vea las propiedades del adaptador de red vEthernet.

    ```powershell
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    ```

    _**Resultados**_

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

14. Habilite los adaptadores de red de vEthernet.

    ```powershell
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    ```

    _**Resultados**_

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>Paso 10. Validar la funcionalidad de RDMA

Desea validar la funcionalidad de RDMA desde el sistema remoto al sistema local, que tiene un vSwitch, a ambos miembros del equipo del conjunto de vSwitch.<p>Dado que tanto el host VNIC \( SMB1 como \) el SMB2 se asignan a la VLAN 102, puede seleccionar el adaptador de VLAN 102 en el sistema remoto. <p>En este ejemplo, la NIC test-40G-2 realiza RDMA en SMB1 (192.168.2.111) y SMB2 (192.168.2.222).

>[!TIP]
>Es posible que tenga que deshabilitar el firewall en este sistema.  Consulte la Directiva de su tejido para obtener más información.
>
>```powershell
>Set-NetFirewallProfile -All -Enabled False
>
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**Resultados**_
>
>```
>Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue
>----  -----------------------   --------------- -------------
> .
> .
>Test-40G-2VLAN ID102VlanID  {102}
>```

1. Ver las propiedades del adaptador de red.

   ```powershell
   Get-NetAdapter
   ```

   _**Resultados**_

   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```

2. Ver la información de RDMA del adaptador de red.

   ```powershell
   Get-NetAdapterRdma
   ```

   _**Resultados**_

   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True
   ```

3. Realice la prueba de tráfico RDMA en el primer adaptador físico.

   ```powershell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Resultados**_

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

4. Realice la prueba de tráfico RDMA en el segundo adaptador físico.

   ```powershell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Resultados**_

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

5. Pruebe el tráfico RDMA desde el equipo local al remoto.

    ```powershell
    Get-NetAdapter | ft –AutoSize
    ```

    _**Resultados**_

    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. Realice la prueba de tráfico RDMA en el primer adaptador virtual.

   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Resultados**_

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

7. Realice la prueba de tráfico RDMA en el segundo adaptador virtual.

   ```powershell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Resultados**_

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

La última línea de esta salida, "prueba de tráfico RDMA correcta: el tráfico RDMA se envió a 192.168.2.5", muestra que ha configurado correctamente la NIC convergente en el adaptador.

## <a name="related-topics"></a>Temas relacionados

- [Configuración de NIC convergente con un solo adaptador de red](cnic-single.md)
- [Configuración del conmutador físico para la NIC convergente](cnic-app-switch-config.md)
- [Solución de problemas de configuraciones de NIC convergentes](cnic-app-troubleshoot.md)
