---
title: Configuración de formación de equipos NIC
description: En este tema, nos ofrecerle una visión general de las propiedades del equipo NIC como formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles sobre la configuración del adaptador en espera y la propiedad de la interfaz principal del equipo. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador de modo de espera para tolerancia a errores.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: dd222cdbcd8b4eee19da6b79e12bd11f6bdd8629
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283726"
---
# <a name="nic-teaming-settings"></a>Configuración de formación de equipos NIC
En este tema, nos ofrecerle una visión general de las propiedades del equipo NIC como formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles sobre la configuración del adaptador en espera y la propiedad de la interfaz principal del equipo. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador de modo de espera para tolerancia a errores.


  
![Propiedades del equipo NIC](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  

## <a name="teaming-modes"></a>Modos de formación de equipos 
Las opciones de modo de formación de equipos son **independiente del conmutador** y **dependientes del conmutador**. Incluye el modo dependientes del conmutador **formación de equipos estática** y **protocolo de Control de agregación de vínculos (LACP)** . 

>[!TIP]
>Para obtener un mejor rendimiento del equipo NIC, se recomienda que utilice un modo de equilibrio de carga de distribución dinámica.  
  
### <a name="switch-independent"></a>Independiente del conmutador
  
[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)] 
  
Al utilizar el modo independiente del conmutador con distribución dinámica, se distribuye la carga de tráfico de red según el valor de hash de dirección de los puertos TCP como modificada por el algoritmo de equilibrio de carga dinámica. El algoritmo de equilibrio dinámico de carga redistribuye flujos para optimizar el uso de ancho de banda de miembro de equipo para que las transmisiones de flujo individual pueden mover de un miembro del equipo activo a otro. El algoritmo tiene en cuenta la posibilidad pequeña que redistribuir el tráfico podría provocar desorden de entrega de paquetes, por lo que tendrá los pasos para minimizar esa posibilidad.  
  
### <a name="switch-dependent"></a>Dependiente del conmutador  

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]  
  
> [!IMPORTANT]  
> Formación de equipos dependientes conmutador requiere que todos los miembros del equipo están conectados al mismo conmutador físico o cambiar un chasis múltiples que comparte un Id. de conmutador entre el chasis múltiples.


- **Formación de equipos estática.** Formación de equipos estática, deberá configurar manualmente el conmutador y el host para identificar qué vínculos forman el equipo. Se trata de una solución configurada estáticamente, no hay ningún protocolo adicional para ayudarle en el conmutador y el host para identificar de forma incorrecta conectado cables u otros errores que podrían provocar que el equipo no se pueda realizar. Este modo lo suelen ofrecer los conmutadores de clase de servidor.

- **Protocolo de Control de agregación de vínculos (LACP).** A diferencia de formación de equipos estática, modo de formación de equipos LACP dinámicamente identifica los vínculos que están conectados entre el host y el conmutador. Esta conexión dinámica permite la creación automática de un equipo y, en teoría, pero rara vez en la práctica, la expansión y reducción de un equipo simplemente mediante la transmisión o recepción de paquetes LACP de la entidad del mismo nivel. Todos los conmutadores de clase de servidor admiten LACP y todos requieren que el operador de red habilitar administrativamente LACP en el puerto del conmutador. Al configurar un modo de formación de equipos de LACP, formación de equipos NIC funciona siempre en modo activo del LACP con un temporizador corto.  Ninguna opción está actualmente disponible para modificar el temporizador o cambiar el modo LACP.


Cuando usas una modos dependientes del conmutador con distribución dinámica, se distribuye la carga de tráfico de red según el valor de hash de dirección TransportPorts como modificada por el algoritmo de equilibrio de carga dinámica.  El algoritmo de equilibrio dinámico de carga redistribuye flujos para optimizar el uso de ancho de banda de miembro de equipo. Las transmisiones de flujo individual pueden mover de miembro de un equipo activo a otro como parte de la distribución dinámica. El algoritmo tiene en cuenta la posibilidad pequeña que redistribuir el tráfico podría provocar desorden de entrega de paquetes, por lo que tendrá los pasos para minimizar esa posibilidad.  
  
Como con todas las configuraciones dependientes de modificador, el modificador determina cómo distribuir el tráfico entrante entre los miembros del equipo.  Se espera el conmutador para realizar un trabajo razonable de distribuir el tráfico entre los miembros del equipo, pero tiene total independencia para determinar cómo lo hace.  


## <a name="load-balancing-modes"></a>Modos de equilibrio de carga  
Las opciones de equilibrio de carga de modo de distribución son **Hash de dirección**, **puerto Hyper-V**, y **dinámica**.  
  
### <a name="address-hash"></a>Hash de dirección
  
[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
  
Use Windows PowerShell para especificar valores para los siguientes componentes de la función hash.  
  
-   Las direcciones IP de origen y destino y puertos TCP de origen y destino. Este es el valor predeterminado cuando se selecciona **Hash de dirección** como el modo de equilibrio de carga.  
  
-   Origen y destino únicamente de direcciones IP.  
  
-   Origen y destino MAC únicamente de direcciones.  
  
El hash de puertos TCP, crea la distribución más granular de flujos de tráfico, lo que resulta en flujos más pequeños que se pueden mover entre los miembros del equipo NIC. Sin embargo, no se puede usar el hash de puertos TCP para el tráfico que no sea TCP o UDP, o donde los puertos TCP y UDP están ocultos de la pila, como con tráfico protegido por IPsec. En estos casos, el hash utiliza automáticamente el hash de dirección IP o, si el tráfico no es tráfico IP, se utiliza el hash de dirección MAC.  
  
### <a name="hyper-v-port"></a>Puerto Hyper-V
  
[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]  
  
Dado que el conmutador adyacente ve siempre una dirección MAC determinada en un puerto, el conmutador distribuye la carga de entrada (el tráfico del conmutador al host) en varios vínculos basándose en el destino de dirección MAC (MAC de la máquina virtual). Esto es especialmente útil cuando se utilizan colas de máquina Virtual (Vmq), dado que una cola puede colocarse en la NIC específica donde se espera que llegue el tráfico.  
  
Sin embargo, si el host tiene solo algunas máquinas virtuales, este modo podría no ser suficientemente pormenorizado como para lograr una distribución bien equilibrada. Este modo también siempre limitará a una sola máquina virtual (es decir, el tráfico desde un puerto del conmutador) al ancho de banda que está disponible en una sola interfaz. Formación de equipos NIC usa el puerto del conmutador Virtual Hyper-V como identificador en lugar de usar la dirección MAC de origen porque, en algunos casos, se puede configurar una máquina virtual con más de una dirección MAC en un puerto de conmutador.  
  
### <a name="dynamic"></a>Dinámico
  
[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
  
Las cargas salientes en este modo dinámicamente se equilibran según el concepto de flowlets. Al igual que la voz humana tiene las divisiones naturales en los extremos de palabras y frases, flujos TCP (secuencias de comunicación de TCP) también tienen quiebres naturales. La parte de un flujo TCP entre dos saltos de este tipo se conoce como un flowlet.  
  
Cuando el algoritmo de modo dinámico detecta que se ha encontrado un límite de flowlet - por ejemplo, cuando se produce un salto de longitud suficiente en el flujo TCP: el algoritmo automáticamente vuelve a equilibrar el flujo a otro miembro del equipo si es necesario.  En algunas circunstancias el algoritmo también periódicamente podría volver a equilibrar flujos que no contienen ningún flowlets. Por este motivo, la afinidad entre los miembros de equipo y de flujo TCP puede cambiar en cualquier momento como funciona el algoritmo de equilibrio dinámico para equilibrar la carga de trabajo de los miembros del equipo.  
  
Si el equipo está configurado con independiente de conmutador o uno de los modos dependientes del conmutador, se recomienda que use el modo de distribución dinámica para un mejor rendimiento.  
  
Hay una excepción a esta regla cuando el equipo NIC tiene solo dos miembros del equipo, está configurado en modo independiente del conmutador y tiene habilitado, con una NIC active el modo activo/en espera y el otro configurado para modo de espera. Con esta configuración de equipo NIC, distribución de Hash de dirección proporciona un rendimiento ligeramente mejor que dinámica una distribución.  


## <a name="standby-adapter-setting"></a>Configuración del adaptador en espera  
Las opciones de adaptador del modo de espera son **None (todos los adaptadores activos)** o la selección de un adaptador de red específico en el equipo NIC que actúa como un adaptador de modo de espera. Cuando se configura una NIC como un adaptador de modo de espera, todos los otros miembros del equipo no seleccionados están activos y se envía ningún tráfico de red o se procesa el adaptador hasta que se produce un error en un adaptador de red activo. Una vez que se produce un error en un adaptador de red activo, la NIC en espera se activa y los procesos de tráfico de red. Cuando se restauran todos los miembros del equipo al servicio, el miembro del equipo en espera se devuelve al código de estado en espera.  

Si tiene un equipo de dos NIC y decide configurar una NIC como un adaptador de modo de espera, perderá el ancho de banda ventajas de agregación que existen con dos NIC activas.  No es necesario designar un adaptador de modo de espera para conseguir tolerancia a errores; siempre está presente siempre que hay al menos dos adaptadores de red en un equipo NIC de tolerancia a errores.
 
  
## <a name="primary-team-interface-property"></a>Propiedad de interfaz de equipo principal  
Para obtener acceso en el cuadro de diálogo de interfaz de equipo principal, debe hacer clic en el vínculo que está resaltado en la ilustración siguiente.  
  
![Propiedad de la interfaz de equipo principal](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
Tras hacer clic en el vínculo resaltado, el siguiente **nueva interfaz de equipo** abre el cuadro de diálogo.  
  
![Cuadro de diálogo Nueva interfaz de equipo](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
Si usa redes VLAN, puede usar este cuadro de diálogo para especificar un número de VLAN.  
  
Si usa redes VLAN, puede especificar un nombre tNIC para el equipo NIC.  
  


---