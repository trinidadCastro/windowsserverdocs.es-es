---
title: Formación de equipos NIC
description: En este tema se proporciona información general sobre la formación de equipos de tarjeta de interfaz de red (NIC) en Windows Server 2016. La formación de equipos NIC le permite agrupar entre uno y 32 adaptadores de red Ethernet físicos en uno o varios adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-nict
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: lizross
author: eross-msft
ms.date: 09/10/2018
ms.openlocfilehash: 13607bedb436b794e03e3b2ef67ca0e90d865ed7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854738"
---
# <a name="nic-teaming"></a>Formación de equipos NIC

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona información general sobre la formación de equipos de tarjeta de interfaz de red (NIC) en Windows Server 2016. La formación de equipos NIC le permite agrupar entre uno y 32 adaptadores de red Ethernet físicos en uno o varios adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.  
  
> [!IMPORTANT]
> Debe instalar los adaptadores de red de miembros del equipo NIC en el mismo equipo host físico. 

> [!TIP]  
> Un equipo NIC que solo contiene un adaptador de red no puede proporcionar equilibrio de carga y conmutación por error. Sin embargo, con un adaptador de red, puede usar la formación de equipos NIC para separar el tráfico de red cuando también usa redes de área local virtual (VLAN).  
  
Cuando se configuran los adaptadores de red en un equipo NIC, se conectan a la solución de formación de equipos NIC núcleo común, que luego presenta uno o más adaptadores virtuales (también denominados NIC de equipo [tNICs] o interfaces de equipo) al sistema operativo. 

Dado que Windows Server 2016 admite hasta 32 interfaces de equipo por equipo, hay una variedad de algoritmos que distribuyen el tráfico saliente (carga) entre las NIC.  En la ilustración siguiente se muestra un equipo NIC con varios tNICs.  
  
![Equipo NIC con varios tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
Además, puede conectar las NIC del equipo al mismo conmutador o a distintos conmutadores. Si conecta las NIC a diferentes conmutadores, ambos conmutadores deben estar en la misma subred.  
  
## <a name="availability"></a>Disponibilidad  
La formación de equipos NIC está disponible en todas las versiones de Windows Server 2016. Puede usar diversas herramientas para administrar la formación de equipos NIC desde equipos que ejecutan un sistema operativo cliente, como:
*    Cmdlets de Windows PowerShell
*    Escritorio remoto
*    Herramientas de administración remota del servidor  
  
## <a name="supported-and-unsupported-nics"></a>NIC admitidas y no admitidas   
Puede usar cualquier NIC Ethernet que haya superado la prueba de logotipo y de calificación de hardware de Windows (pruebas de WHQL) en un equipo NIC en Windows Server 2016.  
  
No puede colocar las siguientes NIC en un equipo NIC:
  
-   Adaptadores de red virtual de Hyper-V que son puertos de conmutador virtual de Hyper-V expuestos como NIC en la partición del host.  
  
    > [!IMPORTANT]  
    > No coloque las NIC virtuales de Hyper-V expuestas en la partición de host (VNIC) en un equipo. La formación de equipos de VNIC dentro de la partición del host no se admite en ninguna configuración. Los intentos de VNIC de equipo pueden provocar una pérdida completa de la comunicación si se producen errores en la red.  
  
-   Adaptador de red de depuración de kernel (KDNIC).  
  
-   NIC usadas para el arranque de red.  
  
-   Las NIC que usan tecnologías distintas de Ethernet, como WWAN, WLAN/Wi-Fi, Bluetooth e InfiniBand, incluidas las NIC del Protocolo de Internet sobre Infiniband (IPoIB).  
  
## <a name="compatibility"></a>Compatibilidad  
La formación de equipos NIC es compatible con todas las tecnologías de red de Windows Server 2016 con las excepciones siguientes.  
  
-   **Virtualización de e/s de raíz única (SR-IOV)** . En el caso de SR-IOV, los datos se entregan directamente a la NIC sin pasarlos a través de la pila de red (en el sistema operativo host, en el caso de la virtualización). Por lo tanto, no es posible que el equipo NIC Inspeccione o Redirija los datos a otra ruta de acceso del equipo.  
  
-   **Calidad de servicio (QoS) del host nativo**. Cuando se establecen directivas de QoS en un sistema host o nativo, y esas directivas invocan limitaciones de ancho de banda mínimo, el rendimiento general de un equipo NIC es inferior al que sería sin las directivas de ancho de banda vigentes.  
  
-   **TCP Chimney**. TCP Chimney no es compatible con la formación de equipos NIC, ya que TCP Chimney descarga toda la pila de red directamente en la NIC.  
  
-   **autenticación de 802.1 x**. No debe usar la autenticación 802.1 X con la formación de equipos NIC porque algunos conmutadores no permiten la configuración de la autenticación 802.1 X y la formación de equipos NIC en el mismo puerto.  
  
Para obtener información sobre el uso de la formación de equipos NIC en máquinas virtuales (VM) que se ejecutan en un host de Hyper-V, consulte [creación de un nuevo equipo NIC en un equipo host o máquina virtual](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md).
  
## <a name="virtual-machine-queues-vmqs"></a>Colas de máquinas virtuales (VMQ)  

VMQ es una característica de NIC que asigna una cola para cada máquina virtual.  Siempre que haya habilitado Hyper-V; también debe habilitar VMQ. En Windows Server 2016, VMQ usar el conmutador NIC vPorts con una sola cola asignada a vPort para proporcionar la misma funcionalidad. 

Según el modo de configuración del conmutador y el algoritmo de distribución de la carga, la formación de equipos NIC presenta el número más pequeño de colas disponibles y admitidas por cualquier adaptador del equipo (modo min-Queues) o el número total de colas disponibles en todo el equipo. Members (modo de suma de colas).  

Si el equipo está en modo de formación de equipos independiente del conmutador y establece la distribución de la carga en modo de puerto de Hyper-V o en modo dinámico, el número de colas que se indican es la suma de todas las colas disponibles en los miembros del equipo (modo de suma de colas). De lo contrario, el número de colas indicado es el número más pequeño de colas admitidas por cualquier miembro del equipo (modo min-Queues).

Este es el motivo:  
  
-   Cuando el equipo independiente del conmutador está en modo de puerto de Hyper-V o en modo dinámico, el tráfico entrante para un puerto de conmutador de Hyper-V siempre llega al mismo miembro del equipo. El host puede predecir o controlar qué miembro recibe el tráfico de una máquina virtual determinada, por lo que la formación de equipos NIC puede ser más meditada sobre qué colas de VMQ se asignan a un miembro del equipo determinado. Formación de equipos NIC, trabajar con el conmutador de Hyper-V, establece VMQ para una máquina virtual en un miembro del equipo precisamente y sabe que el tráfico entrante llega a esa cola.  
  
-   Cuando el equipo está en cualquier modo dependiente del conmutador (formación de equipos estáticos o de formación de equipos), el conmutador al que está conectado el equipo controla la distribución del tráfico entrante. El software de formación de equipos NIC del host no puede predecir qué miembro del equipo obtiene el tráfico entrante para una máquina virtual y puede ser que el conmutador distribuya el tráfico de una máquina virtual entre todos los miembros del equipo. Como resultado del software de formación de equipos NIC, al trabajar con el conmutador de Hyper-V, programa una cola para la máquina virtual en cada miembro del equipo, no solo un miembro del equipo.  
  
-   Cuando el equipo está en modo independiente del conmutador y usa el equilibrio de carga de hash de dirección, el tráfico entrante siempre entra en una NIC (el miembro del equipo principal), todo ello en un solo miembro del equipo. Dado que otros miembros del equipo no están tratando con el tráfico entrante, se programan con las mismas colas que el miembro principal, de modo que si se produce un error en el miembro principal, cualquier otro miembro del equipo puede usarse para recoger el tráfico entrante y las colas ya están en su lugar.  

- La mayoría de las NIC tienen colas usadas para el ajuste de escala en lado de recepción (RSS) o VMQ, pero no al mismo tiempo. Parece que algunas opciones de VMQ son la configuración de las colas de RSS pero que son la configuración de las colas genéricas que usan RSS y VMQ, dependiendo de qué característica esté actualmente en uso. Cada NIC tiene, en sus propiedades avanzadas, los valores de * RssBaseProcNumber y \*MaxRssProcessors. A continuación se muestran algunos valores de VMQ que proporcionan un mejor rendimiento del sistema.  
  
-   Idealmente, cada NIC debe tener el valor * RssBaseProcNumber establecido en un número par mayor o igual que dos (2). El primer procesador físico, Core 0 (procesadores lógicos 0 y 1), normalmente realiza la mayor parte del procesamiento del sistema, por lo que el procesamiento de la red debe dirigirse fuera de este procesador físico. Algunas arquitecturas de máquina no tienen dos procesadores lógicos por procesador físico, por lo que para tales máquinas, el procesador base debe ser mayor o igual que 1. En caso de duda, supongamos que el host usa un procesador lógico 2 por cada arquitectura de procesador física.  
  
-   Si el equipo está en modo de suma de colas, los procesadores de los miembros del equipo no se superponen. Por ejemplo, en un host de 4 núcleos (8 procesadores lógicos) con un equipo de 2 NIC de 10 Gbps, podría establecer el primero para que use el procesador base de 2 y para usar 4 núcleos; la segunda se establecería para usar el procesador base 6 y usar 2 núcleos.  
  
-   Si el equipo está en modo de colas mín., los conjuntos de procesadores que usan los miembros del equipo deben ser idénticos.  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Virtualización de red de Hyper-V (HNV)  
La formación de equipos NIC es totalmente compatible con virtualización de red de Hyper-V (HNV).  El sistema de administración de HNV proporciona información al controlador de formación de equipos NIC que permite a la formación de equipos NIC distribuir la carga de forma que optimice el tráfico de HNV.  
  
## <a name="live-migration"></a>Migración en vivo  
La formación de equipos NIC en máquinas virtuales no afecta a Migración en vivo. Existen las mismas reglas para Migración en vivo tanto si se configura la formación de equipos NIC en la máquina virtual como si no.  


## <a name="virtual-local-area-networks-vlans"></a>Redes de área local virtual (VLAN)
Cuando se usa la formación de equipos NIC, la creación de varias interfaces de equipo permite a un host conectarse a distintas VLAN al mismo tiempo. Configure el entorno con las siguientes directrices:
  
- Antes de habilitar la formación de equipos NIC, configure los puertos de conmutador físico conectados al host de formación de equipos para usar el modo troncal (promiscuo). El conmutador físico debe pasar todo el tráfico al host para filtrar sin modificar el tráfico.  

- No configure filtros de VLAN en las NIC mediante la configuración de propiedades avanzadas de la NIC. Permita que el software de formación de equipos NIC o el conmutador virtual de Hyper-V (si existe) realice el filtrado de VLAN.  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>Uso de redes VLAN con formación de equipos NIC en una máquina virtual  
Cuando un equipo se conecta a un conmutador virtual de Hyper-V, todas las segregaciones de VLAN deben realizarse en el conmutador virtual de Hyper-V en lugar de en la formación de equipos NIC.  

Planea el uso de redes VLAN en una máquina virtual configurada con un equipo NIC mediante las siguientes directrices:
  
-    El método preferido para admitir varias VLAN en una máquina virtual es configurar la máquina virtual con varios puertos en el conmutador virtual de Hyper-V y asociar cada puerto a una VLAN. Nunca debe agrupar estos puertos en la máquina virtual porque esto provoca problemas de comunicación de red.  

-    Si la máquina virtual tiene varias funciones virtuales de SR-IOV (VFs), asegúrese de que se encuentran en la misma VLAN antes de agruparlas en la máquina virtual. Es fácil configurar el VFs diferente para que esté en diferentes VLAN y hacerlo provoca problemas de comunicación de red.  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>Administrar interfaces de red y VLAN 
Si debe tener más de una VLAN expuesta en un sistema operativo invitado, considere la posibilidad de cambiar el nombre de las interfaces Ethernet para aclarar la VLAN asignada a la interfaz. Por ejemplo, si asocia la interfaz **Ethernet** a la VLAN 12 y la interfaz **Ethernet 2** con la VLAN 48, cambie el nombre de la interfaz Ethernet a **EthernetVLAN12** y la otra a **EthernetVLAN48**. 

Cambie el nombre de las interfaces con el comando **de Windows PowerShell Rename-NetAdapter** o mediante el procedimiento siguiente:
  
1.  En Administrador del servidor, en las **propiedades** del adaptador de red al que desea cambiar el nombre, haga clic en el vínculo situado a la derecha del nombre del adaptador de red. 
  
2.  Haga clic con el botón secundario en el adaptador de red cuyo nombre desea cambiar y seleccione **cambiar nombre**.  
  
3.  Escriba el nuevo nombre del adaptador de red y presione Entrar.  


## <a name="virtual-machines-vms"></a>Virtual Machines (VM)

Si desea usar la formación de equipos NIC en una máquina virtual, debe conectar los adaptadores de red virtual de la máquina virtual solo a conmutadores virtuales de Hyper-V externos. Esto permite que la máquina virtual mantenga la conectividad de red incluso en caso de que se produzca un error en uno de los adaptadores de red físicos conectados a un conmutador virtual o se desconecte. Los adaptadores de red virtual conectados a conmutadores virtuales de Hyper-V internos o privados no pueden conectarse al conmutador cuando están en un equipo y se produce un error de red en la máquina virtual.  
  
La formación de equipos NIC en Windows Server 2016 admite equipos con dos miembros en máquinas virtuales. Puede crear equipos más grandes, pero no se admiten equipos más grandes. Cada miembro del equipo debe conectarse a un conmutador virtual externo de Hyper-V y las interfaces de red de la máquina virtual deben configurarse para permitir la formación de equipos.

  
Si está configurando un equipo NIC en una máquina virtual, debe seleccionar un **modo de formación de equipos** _independiente_ y un modo de **equilibrio de carga** de hash de _Dirección_.   
  
  
## <a name="sr-iov-capable-network-adapters"></a>Adaptadores de red compatibles con SR-IOV  
Un equipo NIC en o en el host de Hyper-V no puede proteger el tráfico de SR-IOV porque no pasa por el conmutador de Hyper-V.  Con la opción de formación de equipos NIC de VM, puede configurar dos conmutadores virtuales de Hyper-V externos, cada uno conectado a su propia NIC compatible con SR-IOV.  
  
![Formación de equipos NIC con adaptadores de red compatibles con SR-IOV](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
Cada máquina virtual puede tener una función virtual (VF) de una o ambas NIC de SR-IOV y, en el caso de una desconexión de la NIC, la conmutación por error del VF principal al adaptador de copia de seguridad (VF). Como alternativa, la máquina virtual puede tener un FV de una NIC y un vmNIC que no sea de VF conectado a otro conmutador virtual. Si la NIC asociada con el VF se desconecta, el tráfico puede conmutar por error al otro conmutador sin pérdida de conectividad.  
  
Dado que la conmutación por error entre las NIC de una máquina virtual puede provocar que el tráfico se envíe con la dirección MAC del otro vmNIC, cada puerto de conmutador virtual de Hyper-V asociado a una máquina virtual que usa la formación de equipos NIC debe configurarse para permitir la formación de equipos. 


## <a name="related-topics"></a>Temas relacionados

- [Administración y uso de direcciones MAC de formación de equipos NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): cuando se configura un equipo NIC con el modo independiente del conmutador y con una distribución de carga dinámica o de hash de dirección, el equipo usa la dirección Media Access Control (Mac) del miembro del equipo NIC principal en el tráfico saliente. El miembro del equipo NIC principal es un adaptador de red seleccionado por el sistema operativo del conjunto inicial de miembros del equipo.

- [Configuración de la formación de equipos NIC](nic-teaming-settings.md): en este tema se proporciona información general de las propiedades del equipo NIC, como la formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles acerca de la configuración del adaptador en espera y la propiedad de la interfaz de equipo principal. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador en espera para la tolerancia a errores.
  
- [Crear un nuevo equipo NIC en un equipo host o una máquina](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)virtual: en este tema, creará un nuevo equipo NIC en un equipo host o en una máquina virtual (VM) de Hyper-V que ejecute Windows Server 2016.

- [Solución de problemas de formación de equipos NIC](Troubleshooting-NIC-Teaming.md): en este tema se describen las formas de solucionar problemas de formación de equipos NIC, como hardware, los valores de los conmutadores físicos y la deshabilitación o habilitación de adaptadores de red con Windows PowerShell. 
 
