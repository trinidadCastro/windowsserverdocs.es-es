---
title: Formación de equipos NIC
description: En este tema, le proporcionamos una visión general de formación de equipos de tarjeta de interfaz de red (NIC) en Windows Server 2016. Formación de equipos NIC permite agrupar entre 1 y 32 adaptadores de red Ethernet físicos en uno o más adaptadores de red virtual basada en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.date: 09/10/2018
ms.openlocfilehash: cf58956ead8e8a47b8ec6d189bf23e5c576d5f15
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812186"
---
# <a name="nic-teaming"></a>Formación de equipos NIC

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, le proporcionamos una visión general de formación de equipos de tarjeta de interfaz de red (NIC) en Windows Server 2016. Formación de equipos NIC permite agrupar entre 1 y 32 adaptadores de red Ethernet físicos en uno o más adaptadores de red virtual basada en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.  
  
> [!IMPORTANT]
> Debe instalar a adaptadores de red del miembro de equipo NIC en el mismo equipo host físico. 

> [!TIP]  
> Un equipo NIC que contiene solo un adaptador de red no puede proporcionar equilibrio de carga y conmutación por error. Sin embargo, con un adaptador de red, puede usar la formación de equipos NIC para separar el tráfico de red cuando también se usa redes de área Local virtual (VLAN).  
  
Al configurar los adaptadores de red en un equipo NIC, se conectan a la NIC formación de equipos solución base común, que, a continuación, presenta uno o más adaptadores virtuales (también denominados equipo NIC [tNICs] o interfaces del equipo) en el sistema operativo. 

Dado que Windows Server 2016 admite hasta 32 interfaces del equipo por equipo, hay una variedad de algoritmos que distribuyen el tráfico saliente (cargar) entre las NIC.  La siguiente ilustración muestra un equipo NIC con varias tNICs.  
  
![Equipo de NIC con varias tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
Además, puede conectar su las NIC asociadas al mismo conmutador o distintos conmutadores. Si se conecten la NIC a diferentes conmutadores, deben ser ambos modificadores en la misma subred.  
  
## <a name="availability"></a>Disponibilidad  
Formación de equipos NIC está disponible en todas las versiones de Windows Server 2016. Puede usar una variedad de herramientas para administrar la formación de equipos NIC desde equipos que ejecutan un sistema operativo de cliente, como: • los cmdlets de Windows PowerShell • escritorio remoto • herramientas de administración remota del servidor  
  
## <a name="supported-and-unsupported-nics"></a>NIC compatibles y no compatibles   
Puede usar cualquier NIC de Ethernet que ha transcurrido la calificación de Hardware de Windows y el logotipo de prueba (pruebas WHQL) en un equipo NIC en Windows Server 2016.  
  
Las siguientes NIC no se pueden colocar en un equipo NIC:
  
-   Adaptadores de red virtual de Hyper-V que están expuestos como NIC en la partición del host de los puertos de conmutador Virtual de Hyper-V.  
  
    > [!IMPORTANT]  
    > No coloque las NIC virtuales de Hyper-V expuestas en la partición del host (VNIC) en un equipo. Formación de equipos de VNIC dentro de la partición del host no se admite en ninguna otra configuración. Intentos de VNIC de equipo podrían causar una pérdida completa de comunicación si se producen errores de red.  
  
-   Adaptador de red de depuración de kernel (KDNIC).  
  
-   NIC que se usa para el arranque de red.  
  
-   NIC que usan tecnologías diferentes de Ethernet, como WWAN, WLAN, Wi-Fi, Bluetooth e Infiniband, incluido el protocolo de Internet a través de las NIC de Infiniband (IPoIB).  
  
## <a name="compatibility"></a>Compatibilidad  
Formación de equipos NIC es compatible con todas las tecnologías de red en Windows Server 2016 con las siguientes excepciones.  
  
-   **Virtualización de E/S de raíz única (SR-IOV)** . Para SR-IOV, los datos se entregan directamente a la NIC sin que pasen a través de la pila de red (en el sistema operativo de host, en el caso de virtualización). Por lo tanto, no es posible que el equipo NIC para inspeccionar o redirigir los datos a otra ruta de acceso en el equipo.  
  
-   **Calidad de servicio (QoS) del host nativo**. Al establecer directivas de QoS en nativo o el sistema host y esas directivas invocan las limitaciones de ancho de banda mínimo, el rendimiento general de un equipo NIC es menor que sería sin las directivas de ancho de banda en su lugar.  
  
-   **TCP Chimney**. TCP Chimney no es compatible con la formación de equipos NIC porque descarga de TCP Chimney toda la pila de red directamente a la NIC.  
  
-   **Autenticación mediante 802.1X**. No debe usar la autenticación 802.1X con formación de equipos NIC porque algunos modificadores no permiten la configuración de autenticación 802.1X y formación de equipos NIC en el mismo puerto.  
  
Para obtener información sobre cómo usar la formación de equipos NIC dentro de máquinas virtuales (VM) que se ejecutan en un host de Hyper-V, consulte [crear un nuevo equipo NIC en un equipo host o máquina virtual](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md).
  
## <a name="virtual-machine-queues-vmqs"></a>Colas de máquina virtual (Vmq)  

Vmq es una característica NIC que se asigna una cola para cada máquina virtual.  Siempre que tenga habilitado; de Hyper-V También debe habilitar VMQ. En Windows Server 2016, Vmq usa puertos de conmutador de NIC virtuales con una sola cola asignada a la vPort para proporcionar la misma funcionalidad. 

Según el modo de configuración del conmutador y el algoritmo de distribución de carga, la formación de equipos NIC presenta el menor número de colas disponibles y se admite ningún adaptador en el equipo (modo de Min-colas) o el número total de las colas disponibles a través de todos los equipo miembros (modo de suma de colas).  

Si el equipo está en modo de formación de equipos independientes y establecer la distribución de carga al modo de puerto de Hyper-V o dinámico, el número de colas notificados es la suma de todas las colas disponibles en los miembros del equipo (modo de suma de colas). En caso contrario, el número de colas notificado es el menor número de colas admitidos por cualquier miembro del equipo (modo de Min-colas).

Aquí es la razón:  
  
-   Cuando el equipo independiente del conmutador está en modo de puerto de Hyper-V o en modo dinámico el tráfico entrante para un puerto de conmutador de Hyper-V (VM) siempre llega en el mismo miembro de equipo. El host puede predecir y control qué miembro recibe el tráfico para una máquina virtual específica para que la formación de equipos NIC puede ser un estudio más profundo acerca de las colas que VMQ desea asignar a un miembro del equipo determinado. Formación de equipos NIC, trabajar con el conmutador de Hyper-V, se establece el VMQ para una máquina virtual en exactamente un miembro del equipo y se sabe que el tráfico entrante llega a esa cola.  
  
-   Cuando el equipo está en cualquier modo dependientes del conmutador (formación de equipos estática o LACP la formación de equipos), el conmutador al que el equipo está conectado a controla la distribución del tráfico entrante. El software de formación de equipos NIC del host no puede predecir qué equipo miembro Obtiene el tráfico entrante para una máquina virtual y es posible que el conmutador distribuye el tráfico de una máquina virtual a través de todos los miembros del equipo. Como consecuencia de que el software de formación de equipos NIC, trabajar con el conmutador de Hyper-V, programas de una cola para la máquina virtual en cada miembro del equipo, no solo uno integrante del grupo.  
  
-   Cuando el equipo está en modo independiente del conmutador y el hash de dirección usa equilibrio de carga, el tráfico entrante siempre se incluye en una NIC (el miembro del equipo principal) - todo en miembro de un solo equipo. Dado que otros miembros del equipo no se tratan con el tráfico entrante, obtener programarse con las mismas colas como el miembro principal para que si se produce un error en el miembro principal, se puede usar cualquier otro miembro del equipo para recoger el tráfico entrante y las colas ya están instalados.  

- La mayoría de las NIC tienen colas para escalado de lado de recepción (RSS) o VMQ, pero no al mismo tiempo. Algunas opciones de configuración de VMQ parecen ser la configuración de colas RSS, pero están la configuración en las colas genéricas que use RSS y VMQ dependiendo de qué característica está actualmente en uso. Cada NIC tiene en sus propiedades avanzadas, los valores para * RssBaseProcNumber y \*MaxRssProcessors. Siguientes son algunas opciones de configuración de VMQ que proporcionan un mejor rendimiento del sistema.  
  
-   Idealmente, cada NIC debe tener el * RssBaseProcNumber establecido en un número par mayor o igual a dos (2). El primer procesador físico, Core 0 (procesadores lógicos 0 y 1), normalmente realiza la mayor parte del procesamiento del sistema por lo que debe dirigir el procesamiento de red fuera de este procesador físico. Algunas arquitecturas de equipo no tienen dos procesadores lógicos por procesador físico, por lo que para dichas máquinas, el procesador de base debe ser mayor o igual que 1. Si tiene dudas suponer el host está usando un procesador lógico 2 según la arquitectura de procesador físico.  
  
-   Si el equipo está en modo de suma de colas procesadores de los miembros del equipo deben ser no superpuestos. Por ejemplo, en un host de 4 núcleos (8 procesadores lógicos) con un equipo de 2 NIC de 10 Gbps, establecer la primera de ellas para utilizar el procesador de 2 base y usar 4 núcleos; el segundo se establecería en Utilice procesador base 6 y 2 núcleos.  
  
-   Si el equipo está en modo de colas de Min los conjuntos de procesadores utilizados por los miembros del equipo deben ser idénticos.  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Virtualización de red de Hyper-V (HNV)  
Formación de equipos NIC es totalmente compatible con la virtualización de red de Hyper-V (HNV).  El sistema de administración de HNV proporciona información del controlador de formación de equipos NIC que permite la formación de equipos NIC distribuir la carga de forma que optimiza el tráfico de HNV.  
  
## <a name="live-migration"></a>Migración en vivo  
Formación de equipos NIC en máquinas virtuales no afecta a la migración en vivo. Las mismas reglas existen para la migración en vivo, si la configuración de formación de equipos NIC en la máquina virtual.  


## <a name="virtual-local-area-networks-vlans"></a>Redes de área Local virtual (VLAN)
Cuando se usa la formación de equipos NIC, la creación de varias interfaces de equipo permite a un host para conectarse a distintas VLAN simultáneamente. Configure su entorno mediante las siguientes directrices:
  
- Antes de habilitar la formación de equipos NIC, configurar los puertos de conmutador físico conectados al host de formación de equipos para usar el modo de tronco (promiscuo). El conmutador físico debe pasar todo el tráfico al host para el filtrado sin modificar el tráfico.  

- No configure filtros VLAN en las tarjetas NIC mediante el uso de la NIC de propiedades Configuración avanzada. Permitir que el conmutador Virtual de Hyper-V (si existe) o el software de formación de equipos NIC realizar el filtrado de VLAN.  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>Use redes VLAN con formación de equipos NIC en una máquina virtual  
Cuando un equipo se conecta a un conmutador Virtual de Hyper-V, la segregación de todas las VLAN debe realizarse en el conmutador Virtual de Hyper-V en lugar de formación de equipos NIC.  

Va a usar las VLAN en una máquina virtual configurada con un equipo de NIC con las siguientes directrices:
  
-   El método preferido de admitir varias VLAN en una máquina virtual es configurar la máquina virtual con varios puertos en el conmutador Virtual de Hyper-V y asociar cada puerto a una VLAN. Porque si lo hace a problemas de comunicación de red del equipo nunca estos puertos en la máquina virtual.  

-   Si la máquina virtual tiene varias funciones virtuales de SR-IOV (VFs), asegúrese de que están en la misma VLAN antes de formación de equipos de ellos en la máquina virtual. Es posible configurar la VFs diferentes en distintas VLAN fácilmente y si lo hace a problemas de comunicación de red.  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>Administrar interfaces de red y redes VLAN 
Si debe tener más de una VLAN que se expone en un sistema operativo invitado, considere la posibilidad de cambiar el nombre de las interfaces Ethernet para clarificar la VLAN asignada a la interfaz. Por ejemplo, si asocia **Ethernet** interfaz con VLAN 12 y el **Ethernet 2** interfaz con VLAN 48, cambie el nombre de la interfaz Ethernet para **EthernetVLAN12** y el otro a **EthernetVLAN48**. 

Cambiar el nombre de las interfaces mediante el comando de Windows PowerShell **Rename-NetAdapter** o mediante el procedimiento siguiente:
  
1.  En el administrador del servidor, en **propiedades** para el adaptador de red que desea cambiar el nombre, haga clic en el vínculo a la derecha del nombre del adaptador de red. 
  
2.  Haga clic en el adaptador de red que desea cambiar y seleccione **cambiar el nombre**.  
  
3.  Escriba el nuevo nombre para el adaptador de red y presione ENTRAR.  


## <a name="virtual-machines-vms"></a>Máquinas virtuales (VM)

Si desea usar la formación de equipos NIC en una máquina virtual, debe conectarse a Hyper-V conmutadores virtuales externos sólo los adaptadores de red virtual en la máquina virtual. Esto permite que la máquina virtual mantener la conectividad de red incluso en el caso cuando uno de los adaptadores de red físico conectado a un conmutador virtual se produce un error o se desconecta. Adaptadores de red virtuales conectados a conmutadores virtuales de Hyper-V interno o privado no están posible conectarse al conmutador cuando están en un equipo y se produce un error en la red para la máquina virtual.  
  
Formación de equipos NIC en Windows Server 2016 es compatible con los equipos con dos miembros en las máquinas virtuales. Puede crear los equipos más grandes, pero no hay compatibilidad para los equipos más grandes. Cada miembro del equipo debe conectarse a un externo otro conmutador Virtual de Hyper-V y las interfaces de red de la máquina virtual deben configurarse para permitir la formación de equipos.

  
Si está configurando un equipo NIC en una máquina virtual, debe seleccionar un **modo de formación de equipos** de _independiente del conmutador_ y un **modo de equilibrio de carga** de _deHashdedirección_.   
  
  
## <a name="sr-iov-capable-network-adapters"></a>Adaptadores de red compatible con SR-IOV  
Un equipo NIC en o en el host de Hyper-V no puede proteger el tráfico de SR-IOV, ya que no pasa a través del conmutador de Hyper-V.  Con la opción de formación de equipos NIC de máquina virtual, puede configurar dos conmutadores virtuales de Hyper-V externo, cada uno conectado a su propio NIC compatible con SR-IOV.  
  
![Formación de equipos con adaptadores de red compatible con SR-IOV NIC](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
Cada máquina virtual puede tener una función virtual (VF) de uno o ambos NIC SR-IOV y, si se produce una desconexión NIC, conmutación por error desde el VF principal para el adaptador de copia de seguridad (VF). Como alternativa, la máquina virtual puede tener un VF de una NIC y una vmNIC-VF no conectados a otro conmutador virtual. Si se desconecta la NIC asociada con el VF, el tráfico puede conmutar por error a otro conmutador sin pérdida de conectividad.  
  
Dado que el tráfico enviado con la dirección MAC de lo otro vmNIC puede provocar conmutación por error entre las NIC de una máquina virtual, cada puerto de conmutador Virtual de Hyper-V asociado a una máquina virtual con la formación de equipos NIC debe establecerse para permitir la formación de equipos. 


## <a name="related-topics"></a>Temas relacionados

- [Administración y uso de direcciones MAC de la formación de equipos NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): Al configurar un equipo NIC con el modo independiente de conmutador y hash de dirección o distribución de carga dinámica, el equipo usa que la media access control dirección (MAC) del miembro del equipo NIC principal en el tráfico saliente. El miembro del equipo NIC principal es un adaptador de red seleccionado por el sistema operativo desde el conjunto inicial de los miembros del equipo.

- [Configuración de formación de equipos NIC](nic-teaming-settings.md): En este tema, nos ofrecerle una visión general de las propiedades del equipo NIC como formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles sobre la configuración del adaptador en espera y la propiedad de la interfaz principal del equipo. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador de modo de espera para tolerancia a errores.
  
- [Crear un nuevo equipo NIC en un equipo host o máquina virtual](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): En este tema, creará un nuevo equipo NIC en un equipo host o en una máquina virtual de Hyper-V (VM) que ejecuta Windows Server 2016.

- [Solución de problemas de formación de equipos NIC](Troubleshooting-NIC-Teaming.md): En este tema, se describen formas de solucionar la formación de equipos NIC, como hardware, los valores de conmutador físico y habilitación o deshabilitación de los adaptadores de red mediante Windows PowerShell. 
 
