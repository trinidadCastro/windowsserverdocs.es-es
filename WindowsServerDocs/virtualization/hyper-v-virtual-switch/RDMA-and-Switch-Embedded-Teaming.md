---
title: Acceso directo a memoria remota (RDMA) y Switch Embedded Teaming (SET)
description: En este tema se proporciona información sobre cómo configurar las interfaces de acceso directo a memoria remota (RDMA) con Hyper-V en Windows Server 2016, además de información sobre cómo cambiar la formación de equipos incrustados (SET).
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: lizross
author: eross-msft
ms.openlocfilehash: cfa8076b84a2fc62cec2a709fc15d3dc5be8eb77
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307994"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>Acceso directo a memoria remota \(\) RDMA y cambiar la formación de equipos incrustados \(establecida\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona información sobre cómo configurar el acceso directo a memoria remota \(las interfaces de RDMA\) con Hyper-V en Windows Server 2016, además de información sobre cómo cambiar la formación de equipos incrustados \(conjunto de\).  

> [!NOTE]
> Además de este tema, está disponible el siguiente contenido incrustado de formación de equipos. 
> - Descarga de la Galería [de TechNet: Guía de usuario de Windows Server 2016 NIC y switch Embedded Teaming](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="configuring-rdma-interfaces-with-hyper-v"></a><a name="bkmk_rdma"></a>Configuración de interfaces RDMA con Hyper-V  

En Windows Server 2012 R2, el uso de RDMA y Hyper-V en el mismo equipo que los adaptadores de red que proporcionan servicios RDMA no se puede enlazar a un conmutador virtual de Hyper-V. Esto aumenta el número de adaptadores de red físicos que deben instalarse en el host de Hyper-V.

>[!TIP]
>En las ediciones de Windows Server anteriores a Windows Server 2016, no es posible configurar RDMA en los adaptadores de red que están enlazados a un equipo NIC o a un conmutador virtual de Hyper-V. En Windows Server 2016, puede habilitar RDMA en los adaptadores de red que están enlazados a un conmutador virtual de Hyper-V con o sin establecer.

En Windows Server 2016, puede usar menos adaptadores de red mientras usa RDMA con o sin establecer.

En la imagen siguiente se muestran los cambios de la arquitectura de software entre Windows Server 2012 R2 y Windows Server 2016.

![Cambios de arquitectura](../media/RDMA-and-SET/rdma_over.jpg)

En las secciones siguientes se proporcionan instrucciones sobre cómo usar los comandos de Windows PowerShell para habilitar el protocolo de puente del centro de datos (DCB), crear un conmutador virtual de Hyper-V con una NIC virtual de RDMA \(vNIC\)y crear un conmutador virtual de Hyper-V con SET y RDMA VNIC.

### <a name="enable-data-center-bridging-dcb"></a>Habilitar el protocolo de puente del centro de datos \(DCB\)

Antes de usar cualquier RDMA a través de Ethernet convergente \(RoCE\) versión de RDMA, debe habilitar DCB.  Aunque no es necesario para el protocolo RDMA de área extensa de Internet \(iWARP\) Networks, las pruebas han determinado que todas las tecnologías RDMA basadas en Ethernet funcionan mejor con DCB. Por ello, debe considerar la posibilidad de usar DCB incluso para las implementaciones de iWARP RDMA.

En los siguientes comandos de ejemplo de Windows PowerShell se muestra cómo habilitar y configurar DCB para SMB directo.

Activar DCB

    Install-WindowsFeature Data-Center-Bridging

Establecer una directiva para SMB-Direct:

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

Active el control de flujo para SMB:

    Enable-NetQosFlowControl  -Priority 3

Asegúrese de que el control de flujo esté desactivado para otro tráfico:

    Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7

Aplique la Directiva a los adaptadores de destino:

    Enable-NetAdapterQos  -Name "SLOT 2"

Proporcione a SMB directo el 30% del ancho de banda mínimo:

`New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS`  

Si tiene un depurador de kernel instalado en el sistema, debe configurar el depurador para permitir que se establezca QoS ejecutando el comando siguiente.

Invalidar el depurador: de forma predeterminada, el depurador bloquea NetQos:
 
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>Crear un conmutador virtual de Hyper-V con un vNIC RDMA

Si SET no es necesario para la implementación, puede usar los siguientes comandos de Windows PowerShell para crear un conmutador virtual de Hyper-V con un vNIC de RDMA.

> [!NOTE]
> El uso de conjuntos de equipos con NIC físicas compatibles con RDMA proporciona más recursos de RDMA para que los VNIC los consuman.

    New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"

Agregue host VNIC y haga que sean compatibles con RDMA:

    Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
    Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"

Comprobar las capacidades de RDMA:

    Get-NetAdapterRdma

###  <a name="create-a-hyper-v-virtual-switch-with-set-and-rdma-vnics"></a><a name="bkmk_set-rdma"></a>Crear un conmutador virtual de Hyper-V con SET y RDMA VNIC

Para usar RDMA rastreo en los adaptadores de red virtual del host de Hyper-V \(VNIC\) en un conmutador virtual de Hyper-V que admita la formación de equipos de RDMA, puede usar estos comandos de ejemplo de Windows PowerShell.

    New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true

Agregar host VNIC:

    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS

Muchos conmutadores no pasarán información de clase de tráfico en el tráfico de VLAN sin etiquetar, por lo que debe asegurarse de que los adaptadores de host para RDMA están en VLAN. En este ejemplo se asignan los dos adaptadores virtuales de host SMB_ * a la VLAN 42.
    
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
    

Habilitar RDMA en el host VNIC:

    Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"

Comprobar las capacidades de RDMA; Asegúrese de que las funcionalidades son distintas de cero:

    Get-NetAdapterRdma | fl *


## <a name="switch-embedded-teaming-set"></a>Cambiar la formación de equipos incrustada (SET)  

En esta sección se proporciona información general sobre Switch Embedded Teaming (SET) en Windows Server 2016 y contiene las siguientes secciones.

- [ESTABLECER información general](#bkmk_over)

- [ESTABLECER disponibilidad](#bkmk_avail)

- [NIC admitidas y no admitidas para SET](#bkmk_nics)

- [ESTABLECER compatibilidad con las tecnologías de red de Windows Server](#bkmk_compat)

- [ESTABLECER modos y valores](#bkmk_modes)

- [ESTABLECER y colas de máquinas virtuales (VMQ)](#bkmk_vmq)

- [CONFIGURACIÓN de virtualización de red de Hyper-V (HNV)](#bkmk_hnv)

- [ESTABLECER y Migración en vivo](#bkmk_live)

- [Uso de direcciones MAC en los paquetes transmitidos](#bkmk_mac)

- [Administrar un equipo conjunto](#bkmk_manage)

## <a name="set-overview"></a><a name="bkmk_over"></a>ESTABLECER información general

SET es una solución alternativa para la formación de equipos NIC que se puede usar en entornos que incluyen Hyper-V y las redes definidas por software \(SDN\) stack en Windows Server 2016. El conjunto integra la funcionalidad de formación de equipos NIC en el conmutador virtual de Hyper-V.

El conjunto le permite agrupar entre uno y ocho adaptadores de red Ethernet físicos en uno o varios adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.

Los adaptadores de red de miembros deben estar instalados en el mismo host físico de Hyper-V que se van a colocar en un equipo.

> [!NOTE]
> El uso de SET solo se admite en el conmutador virtual de Hyper-V en Windows Server 2016. No se puede implementar SET en Windows Server 2012 R2.

Puede conectar las NIC del equipo al mismo conmutador físico o a diferentes conmutadores físicos. Si conecta las NIC a diferentes conmutadores, ambos conmutadores deben estar en la misma subred.

En la ilustración siguiente se muestra la arquitectura de conjunto.

![ESTABLECER arquitectura](../media/RDMA-and-SET/set_architecture.jpg)

Dado que el conjunto está integrado en el conmutador virtual de Hyper-V, no se puede usar el conjunto dentro de una máquina virtual (VM). Sin embargo, puede usar la formación de equipos NIC dentro de las máquinas virtuales.

Para obtener más información, consulte [formación de equipos NIC en virtual machines (VM)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms).

Además, SET Architecture no expone interfaces de equipo. En su lugar, debe configurar los puertos del conmutador virtual de Hyper-V.

## <a name="set-availability"></a><a name="bkmk_avail"></a>ESTABLECER disponibilidad

SET está disponible en todas las versiones de Windows Server 2016 que incluyen Hyper-V y la pila de SDN. Además, puede usar los comandos de Windows PowerShell y las conexiones Escritorio remoto para administrar el conjunto de equipos remotos que ejecutan un sistema operativo cliente en el que se admiten las herramientas.

## <a name="supported-nics-for-set"></a><a name="bkmk_nics"></a>NIC admitidas para SET

Puede usar cualquier NIC Ethernet que haya superado el logotipo y la calificación de hardware de Windows \(WHQL\) test en un equipo conjunto en Windows Server 2016. SET requiere que todos los adaptadores de red que son miembros de un equipo conjunto deben ser idénticos \(es decir, el mismo fabricante, el mismo modelo, el mismo firmware y\)de controlador. El conjunto admite entre uno y ocho adaptadores de red en un equipo.
  
## <a name="set-compatibility-with-windows-server-networking-technologies"></a><a name="bkmk_compat"></a>ESTABLECER compatibilidad con las tecnologías de red de Windows Server

SET es compatible con las siguientes tecnologías de red de Windows Server 2016.

- Puente del centro de \(DCB\)
  
- Virtualización de red de Hyper-V: NV-GRE y VxLAN se admiten en Windows Server 2016.  
- Descarga de suma de comprobación de recepción \(IPv4, IPv6,\) TCP: se admiten si alguno de los miembros del equipo los admite.

- Acceso directo a memoria remota \(RDMA\)

- Virtualización de e/s de raíz única \(SR-IOV\)

- Descargas de sumas de comprobación de transmisión \(IPv4, IPv6,\) TCP: se admiten si todos los miembros del equipo lo admiten.

- Colas de máquinas virtuales \(VMQ\)

- Ajuste de escala en lado de recepción virtual \(RSS\)

SET no es compatible con las siguientes tecnologías de red de Windows Server 2016.

- autenticación de 802.1 x. el conmutador virtual de Hyper\-V quita automáticamente los paquetes de 802.1 x de autenticación extensible \(EAP\) en escenarios establecidos.
 
- Descarga de tareas de IPsec \(\)de IPsec. Se trata de una tecnología heredada que no es compatible con la mayoría de los adaptadores de red y donde existe, está deshabilitada de forma predeterminada.

- Usar QoS \(\) de pacer. exe en sistemas operativos de host o nativos. Estos escenarios de QoS no son escenarios de Hyper\-V, por lo que las tecnologías no se intersecan. Además, QoS está disponible pero no está habilitado de forma predeterminada; debe habilitar de manera intencionada QoS.

- Reciba la fusión lateral \(\)RSC. RSC se deshabilita automáticamente mediante el conmutador virtual de Hyper\-V.

- Ajuste de escala en lado de recepción \(RSS\). Como Hyper-V usa las colas para VMQ y VMMQ, RSS siempre está deshabilitado cuando se crea un conmutador virtual.

- Descarga de TCP Chimney. Esta tecnología está deshabilitada de forma predeterminada.

- QoS de máquinas virtuales \(VM:\)QoS. QoS de VM está disponible pero deshabilitada de forma predeterminada. Si configura la QoS de máquina virtual en un entorno establecido, la configuración de QoS producirá resultados imprevisibles.

## <a name="set-modes-and-settings"></a><a name="bkmk_modes"></a>ESTABLECER modos y valores

A diferencia de la formación de equipos NIC, cuando se crea un equipo de conjunto, no se puede configurar un nombre de equipo. Además, se admite el uso de un adaptador en espera en la formación de equipos NIC, pero no se admite en el conjunto. Cuando se implementa SET, todos los adaptadores de red están activos y ninguno está en modo de espera.

Otra diferencia importante entre la formación de equipos NIC y el conjunto es que la formación de equipos NIC ofrece la opción de tres modos diferentes de formación de equipos, mientras que el conjunto solo admite el modo **independiente del conmutador** . Con el modo independiente del conmutador, el conmutador o los conmutadores a los que están conectados los miembros del equipo no saben la presencia del equipo establecido y no determinan cómo se distribuye el tráfico de red para establecer los miembros del equipo. en su lugar, el equipo de conjunto distribuye la red de entrada. tráfico a través de los miembros del equipo de conjunto.

Al crear un nuevo equipo de conjunto, debe configurar las siguientes propiedades del equipo.

- Adaptadores de miembros

- Modo de equilibrio de carga

### <a name="member-adapters"></a>Adaptadores de miembros

Al crear un equipo de conjunto, debe especificar hasta ocho adaptadores de red idénticos que están enlazados al conmutador virtual de Hyper-V como adaptadores de miembros del equipo.

### <a name="load-balancing-mode"></a>Modo de equilibrio de carga

Las opciones para establecer el modo de distribución de equilibrio de carga de equipo son **Puerto de Hyper-V** y **dinámico**.

**Puerto de Hyper-V**

Las máquinas virtuales están conectadas a un puerto del conmutador virtual de Hyper-V. Al usar el modo de puerto de Hyper-V para conjuntos de equipos, el puerto del conmutador virtual de Hyper-V y la dirección MAC asociada se usan para dividir el tráfico de red entre los miembros del equipo de conjunto.

> [!NOTE]
> Cuando se usa SET junto con Packet Direct, el **modificador** de modo de formación de equipos independiente y el **Puerto de Hyper-V** del modo de equilibrio de carga son obligatorios.

Dado que el conmutador adyacente siempre ve una dirección MAC determinada en un puerto determinado, el conmutador distribuye la carga de entrada (el tráfico del conmutador al host) al puerto en el que se encuentra la dirección MAC. Esto es especialmente útil cuando se usan colas de máquinas virtuales (VMQ), ya que se puede colocar una cola en la NIC específica en la que se espera que llegue el tráfico.

Sin embargo, si el host tiene solo algunas máquinas virtuales, este modo podría no ser lo suficientemente granular para lograr una distribución bien equilibrada. Este modo también limitará siempre una única máquina virtual (es decir, el tráfico de un solo puerto de conmutador) al ancho de banda que está disponible en una sola interfaz.

**Dinámica**

Este modo de equilibrio de carga proporciona las siguientes ventajas.

- Las cargas salientes se distribuyen en función de un hash de los puertos TCP y las direcciones IP.  El modo dinámico también vuelve a equilibrar las cargas en tiempo real para que un flujo de salida determinado pueda moverse entre los miembros del equipo.

- Las cargas de entrada se distribuyen de la misma manera que el modo de puerto de Hyper-V.

Las cargas salientes en este modo se equilibran dinámicamente según el concepto de flowlets. Del mismo modo que la voz humana tiene saltos naturales en los extremos de palabras y oraciones, los flujos TCP (secuencias de comunicación TCP) también tienen interrupciones naturales. La parte de un flujo TCP entre dos saltos de este tipo se conoce como flowlet.

Cuando el algoritmo de modo dinámico detecta que se ha encontrado un límite de flowlet (por ejemplo, cuando se ha producido una interrupción de longitud suficiente en el flujo TCP), el algoritmo vuelve a equilibrar automáticamente el flujo con otro miembro del equipo si es necesario.  En algunas circunstancias poco frecuentes, el algoritmo también podría reequilibrar periódicamente los flujos que no contienen ningún flowlets. Por este motivo, la afinidad entre el flujo TCP y el miembro del equipo puede cambiar en cualquier momento, ya que el algoritmo de equilibrio dinámico funciona para equilibrar la carga de trabajo de los miembros del equipo.

## <a name="set-and-virtual-machine-queues-vmqs"></a><a name="bkmk_vmq"></a>ESTABLECER y colas de máquinas virtuales (VMQ)

VMQ y establecer juntos funcionan bien y debe habilitar VMQ siempre que use Hyper-V y establezca.

> [!NOTE]
> SET siempre presenta el número total de colas que están disponibles en todos los miembros del equipo de conjunto. En la formación de equipos NIC, esto se denomina modo de suma de colas.

La mayoría de los adaptadores de red tienen colas que se pueden usar para el ajuste de escala en lado de recepción \(RSS\) o VMQ, pero no ambos al mismo tiempo.
  
Parece que algunas opciones de VMQ son la configuración de las colas de RSS pero que son realmente la configuración en las colas genéricas que usan RSS y VMQ, dependiendo de qué característica esté actualmente en uso. Cada NIC tiene, en sus propiedades avanzadas, los valores para `*RssBaseProcNumber` y `*MaxRssProcessors`.

A continuación se muestran algunos valores de VMQ que proporcionan un mejor rendimiento del sistema.

- Idealmente, cada NIC debe tener la `*RssBaseProcNumber` establecida en un número par mayor o igual que dos (2). Esto se debe a que el primer procesador físico, Core 0 \(procesadores lógicos 0 y 1\), normalmente realiza la mayor parte del procesamiento del sistema, por lo que el procesamiento de la red se debe dirigir fuera de este procesador físico. 

>[!NOTE]
>Algunas arquitecturas de máquina no tienen dos procesadores lógicos por procesador físico, por lo que para tales equipos el procesador base debe ser mayor o igual que 1. En caso de duda, suponga que el host usa un procesador lógico 2 por cada arquitectura de procesador físico.

- Los procesadores de los miembros del equipo deben ser, en la medida en que sea práctico, no superpuesto. Por ejemplo, en un host de 4 núcleos \(8 procesadores lógicos\) con un equipo de 2 NIC de 10 Gbps, podría establecer el primero para usar el procesador base de 2 y para usar 4 núcleos; la segunda se establecería para usar el procesador base 6 y usar 2 núcleos.

## <a name="set-and-hyper-v-network-virtualization-hnv"></a><a name="bkmk_hnv"></a>ESTABLECER y virtualización de red de Hyper-V \(HNV\)

SET es totalmente compatible con la virtualización de red de Hyper-V en Windows Server 2016. El sistema de administración de HNV proporciona información al controlador de conjunto que permite que el conjunto distribuya la carga de tráfico de red de una manera optimizada para el tráfico de HNV.
  
## <a name="set-and-live-migration"></a><a name="bkmk_live"></a>ESTABLECER y Migración en vivo

Migración en vivo es compatible con Windows Server 2016.

## <a name="mac-address-use-on-transmitted-packets"></a><a name="bkmk_mac"></a>Uso de direcciones MAC en los paquetes transmitidos

Al configurar un equipo de conjunto con la distribución de carga dinámica, los paquetes de un solo origen \(como una sola máquina virtual\) se distribuyen simultáneamente entre varios miembros del equipo. 

Para evitar confundir los conmutadores y evitar las alarmas de oscilación de MAC, establezca reemplaza la dirección MAC de origen por una dirección MAC diferente en los fotogramas que se transmiten a los miembros del equipo que no sean el miembro del equipo afinidad con. Por este motivo, cada miembro del equipo utiliza una dirección MAC diferente y los conflictos de direcciones MAC se evitan a menos que se produzca un error.

Cuando se detecta un error en la NIC principal, el software de formación de equipos comienza a usar la dirección MAC de la máquina virtual en el miembro del equipo que se elige para que actúe como el miembro del equipo afinidad con temporal \(es decir, el que ahora se mostrará al conmutador como\)de interfaz de la máquina virtual.

Este cambio solo se aplica al tráfico que se va a enviar en el miembro del equipo de afinidad con de la máquina virtual con la dirección MAC de la máquina virtual como su dirección MAC de origen. Se sigue enviando otro tráfico con cualquier dirección MAC de origen que hubiera usado antes del error.

A continuación se muestran listas que describen el comportamiento de sustitución de direcciones MAC de formación de equipos, en función de cómo esté configurado el equipo:

- En modo independiente del conmutador con distribución de puertos de Hyper-V

    - Cada puerto vmSwitch se afinidad con a un miembro del equipo
  
    - Cada paquete se envía en el miembro del equipo al que se afinidad con el puerto.  
  
    - No se ha realizado ningún reemplazo de MAC de origen  
  
- En modo independiente del conmutador con distribución dinámica
  
    - Cada puerto vmSwitch se afinidad con a un miembro del equipo  
  
    - Todos los paquetes ARP/NS se envían en el miembro del equipo al que se afinidad con el puerto.  
  
    - Los paquetes enviados en el miembro del equipo que es el miembro del equipo afinidad con no tienen ninguna sustitución de dirección MAC de origen.  
  
    - Los paquetes enviados por un miembro del equipo que no sea el miembro del equipo afinidad con tendrán el reemplazo de dirección MAC de origen.  
  
## <a name="managing-a-set-team"></a><a name="bkmk_manage"></a>Administrar un equipo conjunto

Se recomienda usar System Center Virtual Machine Manager \(\) de VMM para administrar los equipos del conjunto; sin embargo, también puede usar Windows PowerShell para administrar el conjunto. En las secciones siguientes se proporcionan los comandos de Windows PowerShell que puede usar para administrar el conjunto.

Para obtener información sobre cómo crear un equipo de conjunto mediante VMM, consulte la sección "configuración de un conmutador lógico" en el tema de la biblioteca VMM de System Center [crear conmutadores lógicos](https://docs.microsoft.com/system-center/vmm/network-switch).
  
### <a name="create-a-set-team"></a>Crear un equipo de conjunto

Debe crear un equipo de conjunto al mismo tiempo que crea el conmutador virtual de Hyper-V mediante el comando de Windows PowerShell **New-VMSwitch** .

Al crear el conmutador virtual de Hyper-V, debe incluir el nuevo parámetro **EnableEmbeddedTeaming** en la sintaxis del comando. En el ejemplo siguiente, se crea un conmutador de Hyper-V denominado **TeamedvSwitch** con formación de equipos incrustada y dos miembros de equipo iniciales.
  
```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true  
```  
  
Windows PowerShell asume el parámetro **EnableEmbeddedTeaming** cuando el argumento de **NetAdapterName** es una matriz de NIC en lugar de una sola NIC. Como resultado, puede revisar el comando anterior de la siguiente manera.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"  
```  

Si desea crear un conmutador compatible con SET con un solo miembro del equipo para poder agregar un miembro del equipo en un momento posterior, debe usar el parámetro EnableEmbeddedTeaming.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true  
```  

### <a name="adding-or-removing-a-set-team-member"></a>Adición o eliminación de un miembro del equipo

El comando **set-VMSwitchTeam** incluye la opción **NetAdapterName** . Para cambiar los miembros del equipo en un equipo conjunto, escriba la lista deseada de miembros del equipo después de la opción **NetAdapterName** . Si **TeamedvSwitch** se creó originalmente con NIC 1 y NIC 2, el siguiente comando de ejemplo elimina el miembro del equipo "NIC 2" y agrega el nuevo miembro del equipo "NIC 3".
  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"  
```  

### <a name="removing-a-set-team"></a>Quitar un equipo establecido

Solo puede quitar un equipo de conjunto si quita el conmutador virtual de Hyper-V que contiene el equipo establecido.  Use el tema [Remove-VMSwitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch) para obtener información sobre cómo quitar el conmutador virtual de Hyper-V. En el ejemplo siguiente se quita un conmutador virtual denominado **SETvSwitch**.

```  
Remove-VMSwitch "SETvSwitch"  
```  

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>Cambiar el algoritmo de distribución de carga para un equipo conjunto

El cmdlet **set-VMSwitchTeam** tiene una opción **LoadBalancingAlgorithm** . Esta opción toma uno de dos valores posibles: **HyperVPort** o **Dynamic**. Use esta opción para establecer o cambiar el algoritmo de distribución de carga de un equipo insertado en un conmutador. 

En el ejemplo siguiente, el VMSwitchTeam denominado **TeamedvSwitch** usa el algoritmo de equilibrio de carga **dinámico** .  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic  
```  
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>Estableciendo interfaces virtuales a miembros del equipo físico

El conjunto le permite crear una afinidad entre una interfaz virtual \(es decir, el puerto del conmutador virtual de Hyper-V\) y una de las NIC físicas del equipo. 

Por ejemplo, si crea dos VNIC de host para SMB\-Direct, como en la sección [creación de un conmutador virtual de Hyper-V con set y RDMA VNIC](#bkmk_set-rdma), puede asegurarse de que los dos VNIC utilicen distintos miembros del equipo. 

Al agregar el script en esa sección, puede usar los siguientes comandos de Windows PowerShell.

    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”

Este tema se examina con más detalle en la sección 4.2.5 de la [Guía de usuario de Windows Server 2016 NIC y switch Embedded Teaming](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0).
