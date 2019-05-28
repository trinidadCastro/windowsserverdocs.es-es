---
title: Acceso directo a memoria remota (RDMA) y Switch Embedded Teaming (SET)
description: En este tema se proporciona información sobre cómo configurar las interfaces de acceso de memoria directa remota (RDMA) con Hyper-V en Windows Server 2016, además de información sobre Switch Embedded Teaming (SET).
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 485da451eb092336ec93eddfadc6ffa0e677452b
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222749"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>Acceso a memoria directa remota \(RDMA\) y Switch Embedded Teaming \(establecido\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema proporciona información sobre cómo configurar el acceso a memoria directa remota \(RDMA\) interfaces con Hyper-V en Windows Server 2016, además de información sobre Switch Embedded Teaming \(establecer\).  

> [!NOTE]
> Además de este tema, el siguiente contenido Switch Embedded Teaming está disponible. 
> - Descarga de la Galería de TechNet: [NIC de Windows Server 2016 y Switch Embedded Teaming Guía del usuario](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="bkmk_rdma"></a>Configurar las Interfaces RDMA con Hyper-V  

En Windows Server 2012 R2, uso de RDMA y Hyper-V en el mismo equipo que los adaptadores de red que proporcionan servicios RDMA no se pueden enlazar a un conmutador Virtual de Hyper-V. Esto aumenta el número de adaptadores de red físicos que deben instalarse en el host de Hyper-V.

>[!TIP]
>En las ediciones de Windows Server anteriores a Windows Server 2016, no es posible configurar RDMA en los adaptadores de red que están enlazados a un equipo NIC o a un conmutador Virtual de Hyper-V. En Windows Server 2016, puede habilitar RDMA en los adaptadores de red que están enlazados a un conmutador Virtual de Hyper-V con o sin establecer.

En Windows Server 2016, puede usar menos adaptadores de red al usar RDMA con o sin establecer.

La imagen siguiente muestra los cambios de arquitectura de software entre Windows Server 2012 R2 y Windows Server 2016.

![Cambios de arquitectura](../media/RDMA-and-SET/rdma_over.jpg)

Las secciones siguientes proporcionan instrucciones sobre cómo usar los comandos de Windows PowerShell para habilitar el protocolo de puente del centro de datos (DCB), cree un conmutador Virtual de Hyper-V con una NIC virtual RDMA \(vNIC\)y crear un conmutador Virtual de Hyper-V con SET y RDMA VNIC.

### <a name="enable-data-center-bridging-dcb"></a>Puente de centros de datos habilitar \(DCB\)

Antes de usar cualquier RDMA sobre Ethernet convergente \(RoCE\) versión de RDMA, debe habilitar DCB.  Aunque no es necesario para el protocolo de Internet amplia área RDMA \(iWARP\) redes, las pruebas determinó que todas las tecnologías basadas en Ethernet RDMA funcionen mejor con DCB. Por este motivo, considere la posibilidad de usar DCB incluso para las implementaciones de iWARP RDMA.

Los siguientes comandos de Windows PowerShell en el ejemplo muestran cómo habilitar y configurar DCB para SMB directo.

Activar DCB

    Install-WindowsFeature Data-Center-Bridging

Establecer una directiva para SMB-Direct:

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

Activar el Control de flujo para SMB:

    Enable-NetQosFlowControl  -Priority 3

Asegúrese de que el control de flujo está desactivado para el resto del tráfico:

    Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7

Aplicar la directiva a los adaptadores de destino:

    Enable-NetAdapterQos  -Name "SLOT 2"

Asigne a SMB-Direct el 30% del ancho de banda mínimo:

`New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS`  

Si tiene un depurador del núcleo instalado en el sistema, debe configurar el depurador para permitir que QoS debe establecerse, ejecute el comando siguiente.

Invalidar al depurador: de forma predeterminada, el depurador bloquea NetQos:
 
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>Crear un conmutador Virtual de Hyper-V con una vNIC RDMA

Si el conjunto no es necesario para la implementación, puede usar los siguientes comandos de Windows PowerShell para crear un conmutador Virtual de Hyper-V con una vNIC RDMA.

> [!NOTE]
> Uso conjunto de equipos con NIC físicas compatibles con RDMA proporciona más recursos RDMA para las VNIC consumir.

    New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"

Agregue las vNICs de host y hacerlos RDMA compatible con:

    Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
    Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"

Compruebe las funcionalidades de RDMA:

    Get-NetAdapterRdma

###  <a name="bkmk_set-rdma"></a>Crear un conmutador Virtual de Hyper-V con VNIC RDMA y

Al hacer uso de RDMA capabilies en Hyper-V host adaptadores de red virtual \(VNIC\) en un conmutador Virtual de Hyper-V que es compatible con RDMA la formación de equipos, puede usar estos comandos de Windows PowerShell de ejemplo.

    New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true

Agregue las vNICs de host:

    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS

Muchos conmutadores no pasar la información de clase de tráfico en el tráfico no etiquetado de VLAN, así que asegúrese de que los adaptadores de host para RDMA están en VLAN. En este ejemplo, se asigna los dos SMB_ * virtuales adaptadores de host a VLAN 42.
    
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
    

Habilitar RDMA VNIC de Host:

    Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"

Comprobar las funcionalidades RDMA; Asegúrese de que las funciones son distintos de cero:

    Get-NetAdapterRdma | fl *


## <a name="switch-embedded-teaming-set"></a>Switch Embedded Teaming (SET)  

Esta sección proporciona información general de Switch Embedded Teaming (SET) en Windows Server 2016 y contiene las siguientes secciones.

- [Introducción al conjunto](#bkmk_over)

- [CONJUNTO de disponibilidad](#bkmk_avail)

- [NIC compatibles y no compatibles para el conjunto](#bkmk_nics)

- [ESTABLECER la compatibilidad con tecnologías de redes de Windows Server](#bkmk_compat)

- [Modos de conjunto y la configuración](#bkmk_modes)

- [CONJUNTO y las colas de máquina Virtual (Vmq)](#bkmk_vmq)

- [CONJUNTO y virtualización de red de Hyper-V (HNV)](#bkmk_hnv)

- [Migración en vivo y establezca](#bkmk_live)

- [Uso de direcciones MAC en paquetes transmitidos](#bkmk_mac)

- [Administrar un equipo de conjunto](#bkmk_manage)

## <a name="bkmk_over"></a>Introducción al conjunto

CONJUNTO es una solución alternativa de formación de equipos NIC que puede usar en entornos que incluyen Hyper-V y las redes de definidas por Software \(SDN\) pila en Windows Server 2016. CONJUNTO integra alguna funcionalidad de formación de equipos NIC en el conmutador Virtual de Hyper-V.

CONJUNTO permite agrupar entre uno y ocho Ethernet adaptadores de red físicos en uno o más adaptadores de red virtual basada en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.

CONJUNTO de adaptadores de red de miembro deben instalarse en el mismo host físico de Hyper-V que se colocarán en un equipo.

> [!NOTE]
> Solo se admite el uso del conjunto en el conmutador Virtual de Hyper-V en Windows Server 2016. No se puede implementar el conjunto en Windows Server 2012 R2.

Puede conectar su las NIC asociadas al mismo conmutador físico o a diferentes conmutadores físicos. Si se conecten la NIC a diferentes conmutadores, deben ser ambos modificadores en la misma subred.

La siguiente ilustración muestra la arquitectura del conjunto.

![Arquitectura de conjunto](../media/RDMA-and-SET/set_architecture.jpg)

Como conjunto está integrado en el conmutador Virtual de Hyper-V, no se puede usar el conjunto dentro de una máquina virtual (VM). Sin embargo, puede usar la formación de equipos NIC dentro de las máquinas virtuales.

Para obtener más información, consulte [formación de equipos NIC en máquinas virtuales (VM)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms).

Además, la arquitectura de conjunto no expone interfaces del equipo. En su lugar, debe configurar los puertos de conmutador Virtual de Hyper-V.

## <a name="bkmk_avail"></a>CONJUNTO de disponibilidad

CONJUNTO está disponible en todas las versiones de Windows Server 2016 que incluyen Hyper-V y la pila de SDN. Además, puede usar comandos de Windows PowerShell y las conexiones de escritorio remoto para administrar el conjunto de equipos remotos que ejecutan un sistema operativo de cliente en el que se admiten las herramientas.

## <a name="bkmk_nics"></a>NIC compatibles para el conjunto

Puede usar cualquier NIC de Ethernet que ha pasado la calificación de Hardware de Windows y el logotipo \(WHQL\) probar en un conjunto de equipo en Windows Server 2016. CONJUNTO requiere que todos los adaptadores de red que son miembros del equipo de un conjunto deben ser idénticos \(es decir, mismo fabricante, mismo modelo, mismo firmware y controlador\). Es compatible con un conjunto de entre uno y ocho adaptadores de red en un equipo.
  
## <a name="bkmk_compat"></a>ESTABLECER la compatibilidad con tecnologías de redes de Windows Server

CONJUNTO es compatible con las siguientes tecnologías de redes en Windows Server 2016.

- Protocolo de puente de centros de datos \(DCB\)
  
- Virtualización de red de Hyper-V-NV GRE y VxLAN se admiten en Windows Server 2016.  
- Descarga de suma de comprobación del lado de recepción \(IPv4, IPv6, TCP\) -son compatibles si cualquiera de los miembros del equipo conjunto admitirlos.

- Acceso a memoria directa remota \(RDMA\)

- Virtualización de E/S de raíz única \(SR-IOV\)

- Descarga de suma de comprobación del lado de transmisión \(IPv4, IPv6, TCP\) -son compatibles si todos los miembros del equipo conjunto son compatibles con ellos.

- Las colas de máquina virtual \(VMQ\)

- Virtual escala en lado de recepción \(RSS\)

CONJUNTO no es compatible con las siguientes tecnologías de redes en Windows Server 2016.

- Autenticación 802.1X. Protocolo de autenticación Extensible mediante 802.1X \(EAP\) Hyper descarta automáticamente los paquetes\-V Virtual Switch en escenarios de conjunto.
 
- Descarga de tareas de IPsec \(IPsecTO\). Se trata de una tecnología heredada que no es compatible con la mayoría de los adaptadores de red, y donde existe, está deshabilitado de forma predeterminada.

- Con QoS \(pacer.exe\) en host o sistemas operativos nativos. Estos escenarios de QoS no son Hyper\-escenarios V, por lo que las tecnologías no forman una intersección. Además, calidad de servicio está disponible pero no están habilitadas de forma predeterminada: debe habilitar intencionadamente QoS.

- Fusión de lado de recepción \(RSC\). RSC se deshabilita automáticamente por Hyper\-V Virtual Switch.

- Escala en lado de recepción \(RSS\). Dado que Hyper-V usa las colas para VMQ y VMMQ, RSS siempre está deshabilitado cuando creas un conmutador virtual.

- Descarga TCP Chimney. Esta tecnología está deshabilitada de forma predeterminada.

- QoS de la máquina virtual \(QoS de la máquina virtual\). QoS de la máquina virtual está disponible pero está deshabilitado de forma predeterminada. Si configura la QoS de la máquina virtual en un entorno de conjunto, la configuración de QoS provocará resultados impredecibles.

## <a name="bkmk_modes"></a>Modos de conjunto y la configuración

A diferencia de formación de equipos NIC, cuando se crea un conjunto de equipo, no se puede configurar un nombre de equipo. Además, se admite el uso de un adaptador en espera en la formación de equipos NIC, pero no se admite en conjunto. Cuando se implementa el conjunto, todos los adaptadores de red están activos y ninguno está en modo de espera.

Otra diferencia clave entre la formación de equipos NIC y el conjunto es que la formación de equipos NIC brinda la opción de tres modos de formación de equipos diferentes, aunque solo admite el conjunto **independiente del conmutador** modo. Con el modo independiente de conmutador, el conmutador o conmutadores a los que están conectados los miembros del equipo de conjunto no están conscientes de la presencia del equipo del conjunto y no determinan cómo distribuir el tráfico de red para establecer A los miembros del equipo: en su lugar, el equipo conjunto distribuye red entrante tráfico entre los miembros del conjunto.

Cuando se crea un nuevo conjunto de equipo, debe configurar las siguientes propiedades de equipo.

- Adaptadores de miembros

- Modo de equilibrio de carga

### <a name="member-adapters"></a>Adaptadores de miembros

Cuando se crea un conjunto de equipo, debe especificar un máximo de ocho adaptadores de red idénticos que se enlazan al conmutador Virtual de Hyper-V como conjunto de adaptadores de miembros del equipo.

### <a name="load-balancing-mode"></a>Modo de equilibrio de carga

Las opciones de conjunto de equilibrio de carga son el modo de distribución del equipo **puerto Hyper-V** y **dinámica**.

**Puerto Hyper-V**

Las máquinas virtuales están conectadas a un puerto en el conmutador Virtual de Hyper-V. Al utilizar el modo de puerto de Hyper-V para el conjunto de equipos, el puerto de conmutador Virtual de Hyper-V y la dirección MAC asociada se usan para dividir el tráfico de red entre los miembros del equipo de conjunto.

> [!NOTE]
> Cuando usas conjunto junto con Packet Direct, el modo de formación de equipos **independiente del conmutador** y el modo de equilibrio de carga **puerto Hyper-V** son necesarios.

Dado que el conmutador adyacente ve siempre una dirección MAC determinada en un puerto determinado, el conmutador distribuye la carga de entrada (el tráfico del conmutador al host) para el puerto donde se encuentra la dirección MAC. Esto es especialmente útil cuando se utilizan colas de máquina Virtual (Vmq), dado que una cola puede colocarse en la NIC específica donde se espera que llegue el tráfico.

Sin embargo, si el host tiene solo algunas máquinas virtuales, este modo podría no ser suficientemente pormenorizado como para lograr una distribución bien equilibrada. Este modo también siempre limitará a una sola máquina virtual (es decir, el tráfico desde un puerto del conmutador) al ancho de banda que está disponible en una sola interfaz.

**Dynamic**

Este modo de equilibrio de carga proporciona las siguientes ventajas.

- Carga de salida se distribuye según un valor hash de las direcciones IP y puertos TCP.  Modo dinámico también vuelve a equilibra las cargas en tiempo real para que un flujo de salida determinado puede avanzar y retroceder entre los miembros del equipo de conjunto.

- En la misma manera que el modo de puerto de Hyper-V se distribuyen cargas entrantes.

Las cargas salientes en este modo dinámicamente se equilibran según el concepto de flowlets. Al igual que la voz humana tiene las divisiones naturales en los extremos de palabras y frases, flujos TCP (secuencias de comunicación de TCP) también tienen quiebres naturales. La parte de un flujo TCP entre dos saltos de este tipo se conoce como un flowlet.

Cuando el algoritmo de modo dinámico detecta que se ha encontrado un límite de flowlet - por ejemplo cuando se produce un salto de longitud suficiente en el flujo TCP: el algoritmo automáticamente vuelve a equilibrar el flujo a otro miembro del equipo si es necesario.  En algunas circunstancias poco comunes, el algoritmo también periódicamente podría volver a equilibrar flujos que no contienen ningún flowlets. Por este motivo, la afinidad entre los miembros de equipo y de flujo TCP puede cambiar en cualquier momento como funciona el algoritmo de equilibrio dinámico para equilibrar la carga de trabajo de los miembros del equipo.

## <a name="bkmk_vmq"></a>CONJUNTO y las colas de máquina Virtual (Vmq)

VMQ y establezca funcionan bien juntos, y debe habilitar VMQ cada vez que usa Hyper-V y configurar.

> [!NOTE]
> ESTABLECER siempre presenta el número total de las colas que están disponibles en el conjunto de todos los miembros del equipo. En la formación de equipos NIC, esto se denomina modo de suma de colas.

La mayoría de los adaptadores de red tiene las colas que se pueden usar para cualquier escala de recepción \(RSS\) o VMQ, pero no ambos al mismo tiempo.
  
Algunas opciones de configuración de VMQ parecen ser la configuración de colas RSS pero realmente están configuración en las colas genéricas que use RSS y VMQ dependiendo de qué característica está actualmente en uso. Cada NIC tiene en sus propiedades avanzadas, los valores de `*RssBaseProcNumber` y `*MaxRssProcessors`.

Siguientes son algunas opciones de configuración de VMQ que proporcionan un mejor rendimiento del sistema.

- Lo ideal es que cada NIC debe tener el `*RssBaseProcNumber` establecido en un número par mayor o igual a dos (2). Esto es porque el primer procesador físico, Core 0 \(procesadores lógicos 0 y 1\), normalmente realiza la mayoría del procesamiento del sistema para el procesamiento de red debe derivarse fuera de este procesador físico. 

>[!NOTE]
>Algunas arquitecturas de equipo no tienen dos procesadores lógicos por procesador físico, por lo que para estas máquinas el procesador de base debe ser mayor o igual que 1. En caso de duda, se supone que el host está usando un procesador lógico 2 según la arquitectura de procesador físico.

- Procesadores de los miembros del equipo deben ser, en la medida en que resulta práctico, no superpuestos. Por ejemplo, en un host de 4 núcleos \(8 procesadores lógicos\) con un equipo de 2 NIC de 10 Gbps, puede establecer la primera de ellas para usar el procesador de base de 2 y usar 4 núcleos; el segundo se establecería en Utilice procesador base 6 y 2 núcleos.

## <a name="bkmk_hnv"></a>Virtualización de red de Hyper-V y el conjunto \(HNV\)

CONJUNTO es totalmente compatible con virtualización de red de Hyper-V en Windows Server 2016. El sistema de administración de HNV proporciona información del controlador de conjunto que permite distribuir la carga de tráfico de red de forma que está optimizado para el tráfico HNV.
  
## <a name="bkmk_live"></a>Migración en vivo y establezca

Migración en vivo se admite en Windows Server 2016.

## <a name="bkmk_mac"></a>Uso de direcciones MAC en paquetes transmitidos

Al configurar un equipo de conjunto con la distribución de carga dinámica, los paquetes de un único origen \(como una sola máquina virtual\) simultáneamente se distribuyen entre varios miembros del equipo. 

Para evitar que los modificadores confundirse y para evitar que las alarmas oscilación de MAC, establezca reemplaza la dirección MAC de origen con una dirección MAC diferente en los marcos que se transmiten en los miembros del equipo que no sea miembro del equipo con afinidad. Por este motivo, cada miembro del equipo usa una dirección MAC diferente y conflictos de direcciones MAC se les hasta que se produce un error.

Cuando se detecta un error en la NIC principal, el conjunto de formación de equipos de software inicia con dirección MAC de la máquina virtual en el miembro del equipo que se elige para que actúe como miembro del equipo con afinidad temporal \(es decir, lo que aparecerá ahora en el conmutador como la máquina virtual interfaz\).

Este cambio solo se aplica al tráfico que se va a enviar en miembro del equipo con afinidad de la máquina virtual con la dirección MAC de la VM como su dirección MAC de origen. Continúa el tráfico restante se envíe con cualquier dirección MAC habrían usado antes del error de origen.

Siguientes son las listas que describen el comportamiento de sustitución de dirección MAC, según cómo esté configurado el equipo de la formación de equipos de conjunto:

- En el modo independiente de conmutador con la distribución de puerto de Hyper-V

    - Cada puerto vmSwitch es afinidad con un miembro del equipo
  
    - Todos los paquetes se envían en el miembro del equipo al que se afinidad con el puerto  
  
    - No se realiza ninguna sustitución de MAC de origen  
  
- En el modo independiente de conmutador con distribución dinámica
  
    - Cada puerto vmSwitch es afinidad con un miembro del equipo  
  
    - Se envían todos los paquetes de ARP y NS en el miembro del equipo al que se afinidad con el puerto  
  
    - Los paquetes enviados en el miembro del equipo que sea miembro del equipo con afinidad no tienen ningún origen de reemplazo de dirección MAC realiza  
  
    - Paquetes enviados a un miembro del equipo que no sea miembro del equipo con afinidad tendrá el reemplazo de dirección MAC de origen hace  
  
## <a name="bkmk_manage"></a>Administrar un equipo de conjunto

Se recomienda que use System Center Virtual Machine Manager \(VMM\) administrar el conjunto de equipos, pero también puede usar Windows PowerShell para administrar el conjunto. Las secciones siguientes se proporcionan que comandos de Windows PowerShell que puede usar para administrar el conjunto.

Para obtener información sobre cómo crear un conjunto de equipo mediante VMM, consulte la sección "Configurar un conmutador lógico" en el tema de la biblioteca de System Center VMM [crear conmutadores lógicos](https://docs.microsoft.com/system-center/vmm/network-switch).
  
### <a name="create-a-set-team"></a>Crear un conjunto de equipo

Debe crear un conjunto de equipo al mismo tiempo que crea el conmutador Virtual de Hyper-V mediante la **New-VMSwitch** comando de Windows PowerShell.

Cuando se crea el conmutador Virtual de Hyper-V, debe incluir el nuevo **EnableEmbeddedTeaming** parámetro en la sintaxis del comando. En el ejemplo siguiente, se denomina un conmutador de Hyper-V **TeamedvSwitch** con la formación de equipos incrustado y dos equipo inicial se crea miembros.
  
```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true  
```  
  
El **EnableEmbeddedTeaming** se supone que el parámetro mediante Windows PowerShell cuando el argumento **NetAdapterName** es una matriz de NIC en lugar de una sola NIC. Como resultado, puede revisar el comando anterior de la manera siguiente.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"  
```  

Si desea crear un conjunto compatible con conmutador con un único miembro del equipo para que puedan agregar un miembro del equipo en un momento posterior, debe usar el parámetro EnableEmbeddedTeaming.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true  
```  

### <a name="adding-or-removing-a-set-team-member"></a>Agregar o quitar a un miembro del grupo de equipo

El **conjunto VMSwitchTeam** comando incluye el **NetAdapterName** opción. Para cambiar los miembros del equipo en un conjunto de equipo, escriba la lista deseada de los miembros del equipo después de la **NetAdapterName** opción. Si **TeamedvSwitch** se creó originalmente con NIC 1 y 2 NIC, a continuación, el siguiente comando de ejemplo elimina el miembro del grupo de equipo "NIC 2" y agrega el nuevo miembro del grupo de equipo "NIC 3".
  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"  
```  

### <a name="removing-a-set-team"></a>Quitar un equipo de conjunto

Puede quitar un equipo conjunto quitando el conmutador Virtual de Hyper-V que contiene el conjunto de equipo.  Consulte el tema [Remove-VMSwitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch) para obtener información sobre cómo quitar el conmutador Virtual de Hyper-V. En el ejemplo siguiente se quita un conmutador Virtual denominado **SETvSwitch**.

```  
Remove-VMSwitch "SETvSwitch"  
```  

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>Cambiar el algoritmo de distribución de carga para un conjunto de equipo

El **conjunto VMSwitchTeam** cmdlet tiene un **LoadBalancingAlgorithm** opción. Esta opción toma uno de dos valores posibles: **HyperVPort** o **dinámica**. Para establecer o cambiar el algoritmo de distribución de carga para un equipo incrustado en el conmutador, use esta opción. 

En el ejemplo siguiente, se denomina el VMSwitchTeam **TeamedvSwitch** usa el **dinámica** algoritmo de equilibrio de carga.  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic  
```  
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>Interfaces virtuales ajustándola a los miembros del equipo físico

CONJUNTO le permite crear una afinidad entre una interfaz virtual \(es decir, el puerto de conmutador Virtual de Hyper-V\) y una de las NIC físicas en el equipo. 

Por ejemplo, si crea dos de host VNIC para SMB\-directo, como se muestra en la sección [crear un conmutador Virtual de Hyper-V con VNIC RDMA y](#bkmk_set-rdma), puede asegurarse de que las dos VNIC utilizar diferentes miembros del equipo. 

Agregar a la secuencia de comandos en esa sección, puede usar los siguientes comandos de Windows PowerShell.

    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”

En este tema se examina con más detalle en la sección 4.2.5 de la [NIC de Windows Server 2016 y Guía de usuario de formación de equipos incrustado del conmutador](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0).
