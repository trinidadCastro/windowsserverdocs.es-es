---
title: Configuración de la formación de equipos NIC
description: En este tema se proporciona información general sobre las propiedades del equipo NIC, como la formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles acerca de la configuración del adaptador en espera y la propiedad de la interfaz de equipo principal. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador en espera para la tolerancia a errores.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: ab9a8e309c8031108d58c73d82357e913d5ce398
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396471"
---
# <a name="nic-teaming-settings"></a>Configuración de la formación de equipos NIC
En este tema se proporciona información general sobre las propiedades del equipo NIC, como la formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles acerca de la configuración del adaptador en espera y la propiedad de la interfaz de equipo principal. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador en espera para la tolerancia a errores.


  
![Propiedades del equipo NIC](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  

## <a name="teaming-modes"></a>Modos de formación de equipos 
Las opciones para el modo de formación de equipos son **independiente del conmutador** y **dependiente del conmutador**. El modo dependiente del conmutador incluye la **formación de equipos estáticos** y el **Protocolo de control de agregación de vínculos (LACP)** . 

>[!TIP]
>Para mejorar el rendimiento del equipo NIC, se recomienda usar un modo de equilibrio de carga de distribución dinámica.  
  
### <a name="switch-independent"></a>Independiente del conmutador
  
[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)] 
  
Cuando se usa el modo independiente de conmutador con distribución dinámica, la carga de tráfico de red se distribuye en función del hash de dirección de puertos TCP modificado por el algoritmo de equilibrio de carga dinámico. El algoritmo de equilibrio de carga dinámico redistribuye los flujos para optimizar el uso del ancho de banda de los miembros del equipo para que las transmisiones de flujo individuales puedan moverse de un miembro del equipo activo a otro. El algoritmo tiene en cuenta la pequeña posibilidad de que la redistribución del tráfico pueda provocar la entrega desordenada de paquetes, por lo que se requieren pasos para minimizar esa posibilidad.  
  
### <a name="switch-dependent"></a>Dependiente del conmutador  

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]  
  
> [!IMPORTANT]  
> Cambiar la formación de equipos dependiente requiere que todos los miembros del equipo estén conectados al mismo conmutador físico o a un conmutador de varios chasis que comparta un identificador de conmutador entre el chasis múltiple.


- **Formación de equipos estática.** La formación de equipos estática requiere la configuración manual del conmutador y el host para identificar qué vínculos forman el equipo. Dado que se trata de una solución configurada de forma estática, no hay ningún protocolo adicional para ayudar al conmutador y el host a identificar cables conectados incorrectamente u otros errores que puedan hacer que el equipo no funcione. Este modo lo suelen ofrecer los conmutadores de clase de servidor.

- **Protocolo de control de agregación de vínculos (LACP).** A diferencia de la formación de equipos estática, el modo de formación de equipos de LACP identifica dinámicamente los vínculos que están conectados entre el host y el conmutador. Esta conexión dinámica permite la creación automática de un equipo y, en teoría, pero raramente en la práctica, la expansión y reducción de un equipo simplemente por la transmisión o recepción de paquetes LACP de la entidad del mismo nivel. Todos los conmutadores de clase de servidor admiten LACP y todos requieren que el operador de red Habilite administrativamente LACP en el puerto del conmutador. Cuando se configura un modo de formación de equipos de LACP, la formación de equipos NIC siempre funciona en el modo activo de LACP con un temporizador breve.  Actualmente no hay disponible ninguna opción para modificar el temporizador o cambiar el modo LACP.


Cuando se usan modos dependientes del conmutador con distribución dinámica, la carga del tráfico de red se distribuye según el hash de la dirección TransportPorts, tal y como lo ha modificado el algoritmo de equilibrio de carga dinámico.  El algoritmo de equilibrio de carga dinámico redistribuye los flujos para optimizar el uso del ancho de banda de los miembros del equipo. Las transmisiones de flujo individuales pueden pasar de un miembro de equipo activo a otro como parte de la distribución dinámica. El algoritmo tiene en cuenta la pequeña posibilidad de que la redistribución del tráfico pueda provocar la entrega desordenada de paquetes, por lo que se requieren pasos para minimizar esa posibilidad.  
  
Al igual que con todas las configuraciones dependientes del modificador, el modificador determina cómo distribuir el tráfico entrante entre los miembros del equipo.  Se espera que el modificador realice una tarea razonable de distribuir el tráfico entre los miembros del equipo, pero tiene total independencia para determinar cómo lo hace.  


## <a name="load-balancing-modes"></a>Modos de equilibrio de carga  
Las opciones del modo de distribución de equilibrio de carga son **hash de dirección**, **Puerto de Hyper-V**y **dinámico**.  
  
### <a name="address-hash"></a>Hash de dirección
  
[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
  
Use Windows PowerShell para especificar valores para los siguientes componentes de función hash.  
  
-   Puertos TCP de origen y de destino, y direcciones IP de origen y de destino. Este es el valor predeterminado cuando selecciona **hash de dirección** como modo de equilibrio de carga.  
  
-   Solo direcciones IP de origen y destino.  
  
-   Solo direcciones MAC de origen y destino.  
  
El hash de puertos TCP crea la distribución más granular de flujos de tráfico, lo que da lugar a secuencias más pequeñas que se pueden trasladar de forma independiente entre los miembros del equipo NIC. Sin embargo, no se puede usar el hash de puertos TCP para el tráfico que no está basado en TCP o UDP, o donde los puertos TCP y UDP están ocultos de la pila, como con el tráfico protegido por IPsec. En estos casos, el hash utiliza automáticamente el hash de la dirección IP o, si el tráfico no es el tráfico IP, se usa el hash de dirección MAC.  
  
### <a name="hyper-v-port"></a>Puerto de Hyper-V
  
[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]  
  
Dado que el conmutador adyacente siempre ve una dirección MAC determinada en un puerto, el conmutador distribuye la carga de entrada (el tráfico del conmutador al host) en varios vínculos en función de la dirección MAC de destino (MAC de VM). Esto es especialmente útil cuando se usan colas de máquinas virtuales (VMQ), ya que se puede colocar una cola en la NIC específica en la que se espera que llegue el tráfico.  
  
Sin embargo, si el host tiene solo algunas máquinas virtuales, este modo podría no ser lo suficientemente granular para lograr una distribución bien equilibrada. Este modo también limitará siempre una única máquina virtual (es decir, el tráfico de un solo puerto de conmutador) al ancho de banda que está disponible en una sola interfaz. La formación de equipos NIC usa el puerto del conmutador virtual de Hyper-V como identificador en lugar de usar la dirección MAC de origen porque, en algunos casos, una máquina virtual puede estar configurada con más de una dirección MAC en un puerto de conmutador.  
  
### <a name="dynamic"></a>Dinámico
  
[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
  
Las cargas salientes en este modo se equilibran dinámicamente según el concepto de flowlets. Del mismo modo que la voz humana tiene saltos naturales en los extremos de palabras y oraciones, los flujos TCP (secuencias de comunicación TCP) también tienen interrupciones naturales. La parte de un flujo TCP entre dos saltos de este tipo se conoce como flowlet.  
  
Cuando el algoritmo de modo dinámico detecta que se ha encontrado un límite de flowlet, como cuando se ha producido una interrupción de longitud suficiente en el flujo TCP, el algoritmo vuelve a equilibrar automáticamente el flujo con otro miembro del equipo si es necesario.  En algunas circunstancias, el algoritmo también podría reequilibrar periódicamente los flujos que no contienen ningún flowlets. Por este motivo, la afinidad entre el flujo TCP y el miembro del equipo puede cambiar en cualquier momento, ya que el algoritmo de equilibrio dinámico funciona para equilibrar la carga de trabajo de los miembros del equipo.  
  
Tanto si el equipo está configurado con un conmutador independiente como uno de los modos dependientes del conmutador, se recomienda utilizar el modo de distribución dinámica para obtener el mejor rendimiento.  
  
Existe una excepción a esta regla cuando el equipo NIC tiene solo dos miembros del equipo, se configura en el modo independiente del conmutador y tiene habilitado el modo activo/en espera, con una NIC activa y la otra en espera. Con esta configuración de equipo NIC, la distribución de hash de direcciones proporciona un rendimiento ligeramente mejor que la distribución dinámica.  


## <a name="standby-adapter-setting"></a>Configuración del adaptador en espera  
Las opciones del adaptador en espera son **ninguno (todos los adaptadores activos)** o la selección de un adaptador de red específico en el equipo NIC que actúa como adaptador en espera. Cuando se configura una NIC como adaptador en espera, todos los demás miembros del equipo no seleccionados están activos, y el adaptador no envía ni procesa ningún tráfico de red hasta que se produce un error en una NIC activa. Una vez que se produce un error en una NIC activa, la NIC en espera se convierte en activa y procesa el tráfico de red. Cuando todos los miembros del equipo se restauran en el servicio, el miembro del equipo en espera vuelve al estado en espera.  

Si tiene un equipo con dos NIC y decide configurar una NIC como adaptador en espera, perderá las ventajas de agregación de ancho de banda que existen con dos NIC activas.  No es necesario designar un adaptador en espera para lograr la tolerancia a errores; la tolerancia a errores siempre está presente siempre que haya al menos dos adaptadores de red en un equipo NIC.
 
  
## <a name="primary-team-interface-property"></a>Propiedad de interfaz de equipo principal  
Para tener acceso al cuadro de diálogo principal de la interfaz de equipo, debe hacer clic en el vínculo que se resalta en la ilustración siguiente.  
  
![Propiedad de interfaz de equipo principal](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
Después de hacer clic en el vínculo resaltado, se abre el siguiente cuadro de diálogo **nueva interfaz de equipo** .  
  
![Cuadro de diálogo Nueva interfaz de equipo](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
Si usa redes VLAN, puede utilizar este cuadro de diálogo para especificar un número de VLAN.  
  
Tanto si usa redes VLAN como si no, puede especificar un nombre tNIC para el equipo NIC.  
  


---