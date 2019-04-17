---
title: El equipo NIC
description: Este tema proporciona una visión general de los equipos de adaptadores de tarjeta de interfaz de red (NIC) en Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 142f56153187368effdb802c0c1b50359fffc36a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming"></a>El equipo NIC

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema proporciona una visión general de los equipos de adaptadores de tarjeta de interfaz de red (NIC) en Windows Server 2016.

> [!NOTE]  
> Además de este tema, el siguiente contenido equipo NIC está disponible.  
>   
> - [Formación de equipos en máquinas virtuales & #40; NIC Máquinas virtuales & #41;](nict-vms.md)
> - [El equipo NIC y Virtual redes de área Local & #40; VLAN & #41;](nict-and-vlans.md)
> - [Agrupación de administración y el uso de la dirección MAC del NIC](NIC-Teaming-MAC-Address-Use-and-Management.md)
> - [Solución de problemas de equipo NIC](Troubleshooting-NIC-Teaming.md) 
> - [Crear un nuevo equipo NIC en un equipo Host o máquina virtual](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)
> - [Formación de equipos Cmdlets (NetLBFO) en Windows PowerShell NIC](https://technet.microsoft.com/library/jj130849.aspx)
> - Descarga de la Galería de TechNet: [Windows Server 2016 NIC y cambiar la agrupación incrustado Guía del usuario de](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)
  
## <a name="bkmk_over"></a>Información general de agrupación de NIC  
El equipo NIC permite agrupar entre uno y treinta dos físicos adaptadores de red Ethernet en uno o varios adaptadores de red virtual basado en software. Estos adaptadores de red virtual proporcionan rápido rendimiento y la tolerancia a errores en caso de error de adaptador de red.  
  
Adaptadores de red de NIC equipo miembro deben instalarse en el mismo equipo host físico debe colocarse en un equipo.  
  
> [!NOTE]  
> Un equipo NIC que contiene solo un adaptador de red no puede proporcionar equilibrio de carga y la conmutación por error; Sin embargo con un adaptador de red, puedes usar el equipo NIC de separación de tráfico de red cuando se usan también virtual redes de área Local (VLAN).  
  
Al configurar adaptadores de red en un equipo NIC, están conectados al NIC colaboración solución comunes núcleo, que, a continuación, presenta uno o varios adaptadores virtuales (también denominados equipo NIC [tNICs] o interfaces de equipo) para el sistema operativo. Windows Server 2016 admite hasta 32 interfaces de equipo por equipo. Hay una variedad de algoritmos que distribuir el tráfico saliente (cargar) entre el NIC.  
  
La ilustración siguiente muestra un equipo NIC con varios tNICs.  
  
![Equipo NIC con varios tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
Además, puede conectar las NIC asociadas al mismo conmutador o modificadores diferentes. Si te conectas NIC a distintos modificadores, ambos conmutadores deben ser en la misma subred.  
  
## <a name="bkmk_avail"></a>Disponibilidad de agrupación de NIC  
El equipo NIC está disponible en todas las versiones de Windows Server 2016. Además, puedes usar comandos, escritorio remoto y herramientas de administración remota del servidor de Windows PowerShell para administrar el equipo NIC desde equipos que ejecuten un sistema operativo de cliente en el que se admiten las herramientas.  
  
## <a name="bkmk_nics"></a>NIC admitidas y para el equipo NIC  
Puedes usar cualquier NIC Ethernet que superado la prueba de logotipo y la calificación de Hardware de Windows (WHQL pruebas) en un equipo NIC en Windows Server 2016.  
  
Las siguientes NIC no se pueden colocar en un equipo NIC.  
  
-   Adaptadores de red virtual de Hyper-V que se exponen como NIC en la partición del host de puertos de conmutador Virtual de Hyper-V.  
  
    > [!IMPORTANT]  
    > NIC virtuales de Hyper-V que se exponen en la partición de host (vNICs) no deben colocarse en un equipo. No se admite la agrupación de vNICs dentro de la partición de host en ninguna configuración o una combinación. Intenta vNICs equipo podría provocar una pérdida de comunicación completa si se producen errores de red.  
  
-   El adaptador de red de depuración de kernel (KDNIC).  
  
-   NIC que se usan para el arranque de red.  
  
-   NIC que usan tecnologías que no sean Ethernet, como WWAN, WLAN, Wi-Fi, Bluetooth y Infiniband, incluido el protocolo de Internet sobre NIC Infiniband (IPoIB).  
  
## <a name="bkmk_compat"></a>Compatibilidad de agrupación de NIC  
El equipo NIC es compatible con todas las tecnologías de redes en Windows Server 2016 con las siguientes excepciones.  
  
-   **Virtualización de E/S de raíz única (SR-IOV)**. Para SR-IOV, los datos se entregan directamente a la NIC sin pasar por la pila de red (en el sistema operativo, en el caso de virtualización). Por lo tanto, no es posible que el equipo NIC inspeccionar o redirigir los datos a otra ruta en el equipo.  
  
-   **Host nativo de calidad de servicio (QoS)**. Cuando se establecen las directivas de QoS en uno nativo o sistema host y las directivas invocación limitaciones de ancho de banda mínimo, el rendimiento general de un equipo NIC será menor que sería sin las directivas de ancho de banda en su lugar.  
  
-   **Chimeneas TCP**. Chimeneas de TCP no es compatible con el equipo NIC porque TCP chimeneas descarga toda la pila de red directamente a la NIC.  
  
-   **Autenticación de 802.1X**. La autenticación 802.1X no debe usarse con el equipo NIC. Algunos conmutadores no permiten la configuración de la autenticación 802.1X y el equipo en el mismo puerto NIC.  
  
Para obtener información sobre cómo usar el equipo NIC dentro de máquinas virtuales (VM) que se ejecutan en un host de Hyper-V, consulta [equipo NIC en máquinas virtuales & #40; Máquinas virtuales & #41;](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md).  
  
## <a name="bkmk_vmq"></a>El equipo NIC y colas de máquina Virtual (VMQs)  
VMQ y el equipo NIC funcionan bien juntos; VMQ debe estar habilitada en cualquier momento Hyper-V está habilitado. Según el modo de configuración del conmutador y el algoritmo de distribución de carga, el equipo NIC bien presentará funcionalidades VMQ al modificador Hyper-V que muestren el número de colas disponibles al menor número de colas admitidos por cualquier tarjeta en el equipo (modo Min colas) o el número total de las colas disponibles en todos los miembros del equipo (modo de suma de colas).  
  
Específicamente, si el equipo está en independientes del conmutador de agrupación de modo y la distribución de carga se establece en modo de puerto de Hyper-V o dinámico, el número de colas notificado es la suma de todas las colas disponibles en los miembros del equipo (modo de suma de colas); de lo contrario, el número de colas notificado es al menor número de colas compatibles con todos los miembros del equipo (modo Min colas).  
  
Estas son las razones:  
  
-   Cuando el equipo independientes conmutador en modo de puerto de Hyper-V o dinámicos siempre llegará el tráfico entrante para el puerto de un conmutador de Hyper-V (VM) en el miembro del mismo equipo. El host puede predecir/control qué miembro recibirá el tráfico de una máquina virtual determinada para que el equipo NIC puede ser más detenidamente en qué colas VMQ asignar en un miembro del equipo en particular. El equipo NIC, trabajar con el modificador de Hyper-V, establecerá el VMQ para una máquina virtual en exactamente un miembro del equipo y saber que el tráfico entrante llegará a esa cola.  
  
-   Cuando el equipo está en cualquier modo de cambiar dependientes (agrupación estático o agrupación LACP), el modificador que el equipo está conectado a controla la distribución de tráfico entrante. El software de equipo NIC del host no puede predecir qué equipo miembro obtendrá el tráfico entrante para una máquina virtual y es posible que el modificador distribuye el tráfico de una máquina virtual en todos los miembros del equipo. Como resultado el software de equipo NIC, trabajar con el modificador de Hyper-V, programas de una cola para la máquina virtual en cada miembro del equipo, no solo equipo miembro.  
  
-   Cuando el equipo está en modo de cambiar independiente y es mediante un algoritmo de distribución de carga de hash de dirección, el tráfico entrante siempre provendrá una NIC (los miembros del equipo principal) - todo en solo un miembro del equipo. Dado que no se tratan de otros miembros del equipo con el tráfico entrante que obtengan programar con la misma pone en cola como el miembro principal para que si el miembro principal se produce un error cualquier otro miembro del equipo puede usarse para retomar el tráfico entrante y las colas ya están en su lugar.  
  
La mayoría de NIC tienen colas que pueden usarse para recibir lado ajuste de escala (RSS) o VMQ, pero no ambas al mismo tiempo. Algunas opciones de configuración VMQ parecen ser la configuración de las colas RSS pero realmente son opciones de configuración en las colas genéricas que se usa tanto de RSS como VMQ según la función que actualmente está en uso. Cada NIC tiene, en el ha propiedades avanzadas, valores de * RssBaseProcNumber y \*MaxRssProcessors. Siguientes son algunas opciones de configuración de VMQ que proporcionan un mejor rendimiento del sistema.  
  
-   Lo ideal es que debe tener cada NIC la * RssBaseProcNumber se establece en un número par mayor o igual a dos (2). Esto es porque el primer procesador físico, núcleo 0 (procesadores lógicos 0 y 1), por lo general cambia la mayoría de procesamiento del sistema para que el procesamiento de red debe realizar giros fuera de este procesador físico. (Algunas arquitecturas de equipo no tienen dos procesadores lógicos por procesador físico para dichos equipos base procesador debe ser mayor o igual a 1. Si tienes dudas suponiendo que el host está usando un procesador lógico 2 por arquitectura de procesador físico.)  
  
-   Si el equipo está en modo de suma de colas deben ser procesadores de los miembros del equipo, para la extensión resulta práctico, no se superponen. Por ejemplo, en un host de 4 núcleos (8 procesadores lógicos) con un equipo de 2 10 Gbps NIC, Establece la primera de ellas para usar el procesador de base de 2 y usar 4 núcleos; el segundo se establecería en Utilice procesador base 6 y 2 núcleos.  
  
-   Si el equipo está en modo de Min colas los conjuntos de procesadores utilizados por los miembros del equipo deben ser idénticos.  
  
## <a name="bkmk_hnv"></a>Virtualización de la red equipo NIC e Hyper-V (HNV)  
El equipo NIC es totalmente compatible con la virtualización de red de Hyper-V (HNV).  El sistema de administración de HNV proporciona información al controlador de equipo NIC que permite que el equipo NIC distribuir la carga de manera que está optimizada para el tráfico HNV.  
  
## <a name="bkmk_live"></a>El equipo NIC y la migración en vivo  
El equipo NIC en máquinas virtuales no afecta la migración en vivo. Las mismas reglas existen para la migración en vivo o no el equipo NIC se configura en la máquina virtual.  
  
## <a name="see-also"></a>Consulta también  
[Formación de equipos en máquinas virtuales & #40; NIC Máquinas virtuales & #41;](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md)  
  


