---
title: Crear un nuevo equipo NIC en un equipo Host o máquina virtual
description: En este tema proporciona información sobre la configuración del equipo NIC para que comprendan las selecciones que debe hacer cuando estés configurando un nuevo equipo NIC en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a9866d1f4e72b3c77c3233b5e5582d250cfe6a2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Crear un nuevo equipo NIC en un equipo Host o máquina virtual

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema proporciona información sobre la configuración del equipo NIC para que comprendan las selecciones que debe hacer cuando estés configurando un nuevo equipo NIC. Este tema contiene las siguientes secciones.  
  
-   [Elección de un modo de equipo](#bkmk_teaming)  
  
-   [Elección de un modo de equilibrio de carga](#bkmk_lb)  
  
-   [Elegir una opción de configuración del adaptador de reserva](#bkmk_standby)  
  
-   [Uso de la propiedad de la interfaz principal del equipo](#bkmk_primary)  
  
> [!NOTE]  
> Si ya conoces estos elementos de configuración, puedes usar los siguientes procedimientos para configurar el equipo NIC.  
>   
> -   [Crear un nuevo equipo NIC en una máquina virtual](../../technologies/nic-teaming/Create-a-New-NIC-Team-in-a-VM.md)  
> -   [Crear un nuevo equipo NIC](../../technologies/nic-teaming/Create-a-New-NIC-Team.md)  
  
Cuando creas un nuevo equipo NIC, debes configurar las siguientes propiedades de equipo NIC.  
  
-   Nombre de equipo  
  
-   Adaptadores de miembro  
  
-   Modo de equipo  
  
-   Modo de equilibrio de carga  
  
-   Adaptador de reserva  
  
Opcionalmente, también puedes configurar la interfaz principal del equipo y configurar un número de LAN (VLAN) virtual.  
  
Estas propiedades de equipo NIC se muestran en la siguiente ilustración, que contiene los valores de ejemplo para algunas propiedades de equipo NIC.  
  
![Propiedades de equipo NIC](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  
  
## <a name="bkmk_teaming"></a>Elección de un modo de equipo  
Las opciones para el modo de equipos de adaptadores son **conmutador independientes**, **agrupación estático**, y **protocolo de Control de agregación de vínculo (LACP)**. Agrupación estáticos y LACP son modos conmutador dependientes. Para un mejor rendimiento del equipo NIC con los tres modos de equipos de adaptadores, se recomienda usar un modo de equilibrio de carga de distribución dinámica.  
  
**Modificador independiente**  
  
Con el modo de cambiar independientes, el conmutador o cambia a la que el equipo de NIC están conectados los miembros no son conscientes de la presencia del equipo NIC y no determinan cómo distribuir el tráfico de red NIC integrantes - en su lugar, el equipo NIC distribuye el tráfico de red entrantes a través de los miembros del equipo NIC.  
  
Cuando usas el modo independiente modificador con distribución dinámica, la carga de tráfico de red se distribuye según el hash de dirección de los puertos TCP modificado por el algoritmo de equilibrio de carga dinámica. El algoritmo de equilibrio de carga dinámica redistribuye flujos para optimizar el uso de ancho de banda de miembro de equipo para que las transmisiones de flujo individuales puedan moverse de un miembro del equipo activo a otro. El algoritmo tiene en cuenta la posibilidad pequeña que redistribuir tráfico podría provocar fuera de la secuencia distribución de paquetes, para que forme pasos para minimizar esta posibilidad.  
  
**Modificador dependientes**  
  
Con los modos de cambiar dependiente, el modificador para que los miembros del equipo de NIC están conectados determina cómo distribuir el tráfico de red entrantes entre los miembros del equipo NIC. El modificador tiene total independencia para determinar cómo distribuir el tráfico de red a través de los miembros del equipo NIC.  
  
> [!IMPORTANT]  
> Modificador dependientes agrupación requiere que todos los miembros del equipo están conectados al mismo modificador físico o cambiar un chasis múltiples que comparte un identificador de cambiar entre varios chasis.  
  
Equipos de adaptadores estático requiere que configurar manualmente el conmutador y el host para identificar qué se vincula desde el equipo. Dado que es una solución configurada estáticamente, no hay ningún protocolo adicional para ayudar al conmutador y el host de identificar incorrectamente conectado cables u otros errores que pueden hacer que el equipo no se puedan realizar. Este modo normalmente es compatible con modificadores de la clase de servidor.  
  
A diferencia de la agrupación estático, el modo de agrupación LACP dinámicamente identifica vínculos que se conectan entre el host y el modificador. Esta conexión dinámica permite la creación automática de un equipo y, en teoría, pero rara vez aparecen en la práctica, la expansión y la reducción de un equipo simplemente por la transmisión o la recepción de los paquetes LACP de la entidad de sistema del mismo nivel. Todos los conmutadores de clase de servidor admiten LACP y requieren el operador de red habilitar administrativa LACP en el puerto del conmutador. Cuando se configura un modo de equipos de adaptadores de LACP, siempre equipo NIC funciona en modo activo de LACP con un temporizador corto.  Ninguna opción está disponible actualmente para modificar el temporizador o cambiar el modo LACP.  
  
Al usar modos conmutador dependientes con distribución dinámica, la carga de tráfico de red se distribuye según el hash de dirección TransportPorts modificado por el algoritmo de equilibrio de carga dinámica.  El algoritmo de equilibrio de carga dinámica redistribuye flujos para optimizar el uso de ancho de banda de miembro de equipo. Las transmisiones de flujo individuales pueden mover de un activo miembro del equipo a otro como parte de la distribución dinámica. El algoritmo tiene en cuenta la posibilidad pequeña que redistribuir tráfico podría provocar fuera de la secuencia distribución de paquetes, para que forme pasos para minimizar esta posibilidad.  
  
Con todas las configuraciones dependientes de cambio, el modificador determina cómo distribuir el tráfico entrante entre los miembros del equipo.  Se espera que el conmutador para realizar un trabajo razonable de distribuir el tráfico entre los miembros del equipo, pero tiene total independencia para determinar cómo lo hace.  
  
## <a name="bkmk_lb"></a>Elección de un modo de equilibrio de carga  
Las opciones para el modo de distribución de equilibrio de carga son **Hash de dirección**, **puerto de Hyper-V**, y **dinámico**.  
  
**Hash de dirección**  
  
Este modo de equilibrio de carga crea un hash que se basa en componentes de la dirección del paquete. A continuación, asigna los paquetes que tienen ese valor de hash a uno de los adaptadores disponibles. Por lo general este mecanismo solo es suficiente para crear un equilibrio razonable a través de los adaptadores disponibles.  
  
Puedes usar Windows PowerShell para especificar valores para los siguientes componentes de la función hash.  
  
-   Direcciones IP de origen y de destino y puertos TCP de origen y destino. Este es el valor predeterminado cuando se selecciona **Hash de dirección** como el modo de equilibrio de carga.  
  
-   Origen y destino solo direcciones IP.  
  
-   Origen y destino MAC direcciones solo.  
  
El hash de puertos TCP crea la distribución más granular de flujos de tráfico, lo que resulta en secuencias más pequeño que se pueden mover independientemente entre los miembros del equipo NIC. Sin embargo, no puedes usar el hash de puertos TCP para el tráfico que no sea TCP o UDP, o donde los puertos TCP y UDP están ocultos en la pila, como con tráfico protegido por IPsec. En estos casos, el hash automáticamente usa el hash de dirección IP o, si el tráfico no está el tráfico IP, se usa el hash de dirección MAC.  
  
**Puerto de Hyper-V**  
  
Hay una ventaja en usar el modo de puerto de Hyper-V para equipos NIC que están configurados en el host de Hyper-V. Dado que las máquinas virtuales tienen independientes direcciones MAC, dirección MAC de la máquina virtual - o bien el puerto de que la máquina virtual está conectada a en el conmutador Virtual de Hyper-V - puede ser la base sobre la que dividir el tráfico de red entre NIC integrantes.  
  
> [!IMPORTANT]  
> No se puede configurar el NIC los equipos que se crea en máquinas virtuales con el modo de equilibrio de carga de Hyper-V puerto. Usa el Hash de dirección cargar modo equilibrio en su lugar.  
  
Dado que el modificador adyacentes siempre ve una dirección MAC determinada en un puerto, el modificador distribuye la carga de entrada (el tráfico desde el conmutador para el host) en varios vínculos en función de la dirección MAC (VM MAC) de destino. Esto es especialmente útil cuando se usan las colas de máquina Virtual (VMQs), porque una cola puede colocarse en la NIC específica donde el tráfico se espera a que llegue.  
  
Sin embargo, si el host tiene solo unos virtuales, este modo podría no ser lo suficientemente precisa para lograr una distribución equilibrada. Este modo también siempre limitará una sola máquina virtual (es decir, el tráfico de un puerto del conmutador) al ancho de banda que está disponible en una única interfaz. El equipo NIC usa el puerto del conmutador Virtual Hyper-V como identificador en lugar de usar la dirección MAC de origen porque, en algunos casos, una máquina virtual puede configurarse con más de una dirección MAC en un puerto del conmutador.  
  
**Dinámico**  
  
Este modo de equilibrio de carga en utiliza los mejores aspectos de cada uno de los otros dos modos y los combina en un modo de solo:  
  
-   Carga de salida se distribuye en función de un hash de las direcciones IP y los puertos TCP.  Modo dinámico vuelve a equilibrar también se carga en tiempo real para que un determinado flujo de salida puede avanzar y retroceder entre los miembros del equipo.  
  
-   En la misma manera que el modo de puerto de Hyper-V se distribuyen cargas entrantes.  
  
Carga de la salida de este modo se dinámicamente equilibrada basándose en el concepto de flowlets. Al igual que voz humana tiene saltos naturales de los extremos de palabras y frases, flujos TCP (secuencias de comunicación TCP) también tienen saltos naturales. La parte del flujo entre estos dos saltos TCP se conoce como un flowlet.  
  
Cuando el algoritmo de modo dinámico detecta que se ha detectado un límite flowlet - por ejemplo, cuando se produce un salto de longitud suficiente en el flujo TCP - el algoritmo automáticamente vuelve a equilibrar el flujo a otro miembro del equipo si es necesario.  En algunas circunstancias el algoritmo es posible que también periódicamente equilibrar los flujos que no contienen ningún flowlets. Por este motivo, la afinidad entre miembro TCP de flujo y el equipo puede cambiar en cualquier momento, como el algoritmo de equilibrio dinámico funciona para equilibrar la carga de trabajo de los miembros del equipo.  
  
Si el equipo está configurado con conmutador independientes o uno de los modos conmutador dependiente, se recomienda que puedes usar el modo de distribución dinámicas para un mejor rendimiento.  
  
Hay una excepción a esta regla cuando el equipo NIC tiene tan solo dos miembros del equipo, está configurado en el modo independiente del conmutador y tiene habilitado, con una NIC active el modo activo/en espera y el otro configurado para el modo de espera. Con esta configuración de equipo NIC, distribución de Hash de dirección proporciona un poco mejor rendimiento que dinámico distribución.  
  
## <a name="bkmk_standby"></a>Elegir una opción de configuración del adaptador de reserva  
Las opciones de modo de espera adaptador son ninguno (todos los adaptadores activo) o la selección de un adaptador de red específica en el equipo NIC que actuará como un adaptador de modo de espera, mientras que otros miembros del equipo no seleccionado activo. Cuando se configura una NIC como un adaptador de modo de espera, el tráfico de red se envía o se procesa el adaptador, a menos que y hasta el momento en que cuando se produce un error en una NIC activo. Después de que se produce un error en una NIC activo, se activa la NIC de modo de espera y procesos del tráfico de red. Si todos los integrantes se restauran al servicio, el miembro del equipo en modo de espera se devuelve al estado de suspensión.  
  
Si tienes un equipo de dos NIC y decide configurar una NIC como un adaptador de modo de espera, perderás el ancho de banda ventajas de agregación que existen con dos NIC activas.  
  
> [!IMPORTANT]  
> No es necesario designar un adaptador de modo de espera para lograr tolerancia a errores; siempre está presente cuando hay al menos dos adaptadores de red en un equipo NIC tolerancia a errores.  
  
## <a name="bkmk_primary"></a>Uso de la propiedad de la interfaz principal del equipo  
Para tener acceso en el cuadro de diálogo de la interfaz principal de equipo, debe hacer clic en el vínculo que está resaltado en la ilustración siguiente.  
  
![Propiedad de la interfaz principal del equipo](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
Después de hacer clic en el vínculo resaltado, el siguiente **nueva interfaz de equipo** abre el cuadro de diálogo.  
  
![Cuadro de diálogo interfaz equipo nuevo](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
Si estás usando VLAN, puedes usar este cuadro de diálogo para especificar un número VLAN.  
  
Si no estás usando VLAN, puede especificar un nombre de tNIC para el equipo NIC.  
  
## <a name="see-also"></a>Consulta también  
[Crear un nuevo equipo NIC en una máquina virtual](../../technologies/nic-teaming/Create-a-New-NIC-Team-in-a-VM.md)  
[El equipo NIC](NIC-Teaming.md)  
  


