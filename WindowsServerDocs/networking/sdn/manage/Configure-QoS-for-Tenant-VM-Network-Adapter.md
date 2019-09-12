---
title: Configurar la calidad de servicio (QoS) para un adaptador de red de máquina virtual de inquilino
description: Al configurar QoS para un adaptador de red de máquina virtual de inquilino, tiene la opción de elegir entre el protocolo de puente del centro de datos DCB o la QoS de redes definidas por software.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d783ff6-7dd5-496c-9ed9-5c36612c6859
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 99ef286b91bec4bcb008bfd9f62003e75a5a5921
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870020"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configurar la calidad de servicio (QoS) para un adaptador de red de máquina virtual de inquilino

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Al configurar QoS para un adaptador de red de máquina virtual de inquilino, tiene la opción de elegir \(entre\)el protocolo de puente del\) centro de datos DCB o la QoS de redes \(definidas por software.

1.  **DCB**. Puede configurar DCB mediante los cmdlets de NetQoS de Windows PowerShell. Para obtener un ejemplo, vea la sección sobre cómo habilitar el protocolo de puente del centro de datos en el tema [acceso directo a memoria remota (RDMA) y switch Embedded Teaming (Set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.  **QoS de Sdn**. Puede habilitar la QoS de SDN mediante el uso de la controladora de red, que se puede establecer para limitar el ancho de banda en una interfaz virtual para impedir que una máquina virtual de tráfico alto bloquee a otros usuarios.  También puede configurar la QoS de SDN para reservar una cantidad específica de ancho de banda para una máquina virtual con el fin de asegurarse de que la máquina virtual sea accesible independientemente de la cantidad de tráfico de red.  

Aplique la configuración de QoS de SDN a través de la configuración de puerto de las propiedades de la interfaz de red. Consulte la tabla siguiente para obtener más detalles.

|Nombre del elemento|Descripción|
|------------|-----------| 
|macSpoofing| Permite que las máquinas virtuales cambien \(la\) dirección Mac de origen Media Access Control en los paquetes salientes a una dirección Mac no asignada a la máquina virtual.<p>Valores permitidos:<ul><li>Habilitado: Use una dirección MAC diferente.</li><li>Deshabilitado: solo se usa la dirección MAC asignada.</li></ul>|
|arpGuard| Permite que ARP Guard solo las direcciones especificadas en ArpFilter para pasar a través del puerto.<p>Valores permitidos:<ul><li>Habilitado: permitido</li><li>Deshabilitado: no permitido</li></ul>|
|dhcpGuard| Permite o quita los mensajes DHCP de una máquina virtual que notifica a un servidor DHCP. <p>Valores permitidos:<ul><li>Habilitado: quita los mensajes DHCP porque el servidor DHCP virtualizado se considera que no es de confianza.</li><li>Deshabilitado: permite que se reciba el mensaje porque el servidor DHCP virtualizado se considera de confianza.</li></ul>|
|stormLimit| El número de paquetes (difusión, multidifusión y unidifusión desconocida) por segundo que una máquina virtual puede enviar a través del adaptador de red virtual. Los paquetes que superan el límite durante ese intervalo de un segundo se quitan. Un valor de cero \(0\) significa que no hay límite.|
|portFlowLimit| Número máximo de flujos que se pueden ejecutar para el puerto. Un valor en blanco o cero \(0\) significa que no hay ningún límite. |
|vmqWeight| El peso relativo describe la afinidad del adaptador de red virtual para utilizar Virtual Machine Queue (VMQ). El intervalo de valores es de 0 a 100.<p>Valores permitidos:<ul><li>0: deshabilita VMQ en el adaptador de red virtual.</li><li>1-100: habilita VMQ en el adaptador de red virtual.</li></ul>|
|iovWeight| La ponderación relativa establece la afinidad del adaptador de red virtual con la función \(\) virtual de la virtualización de e/s de la virtualización de e/s de raíz única asignada. <p>Valores permitidos:<ul><li>0: deshabilita SR-IOV en el adaptador de red virtual.</li><li>1-100: habilita SR-IOV en el adaptador de red virtual.</li></ul>|
|iovInterruptModeration|<p>Valores permitidos:<ul><li>predeterminada: la configuración del proveedor del adaptador de red físico determina el valor.</li><li>contrario </li><li>Desactivado </li><li>bajo</li><li>medio</li><li>alta</li></ul><p>Si elige **predeterminada**, la configuración del proveedor del adaptador de red físico determina el valor.  Si elige, **Adaptive**, el patrón de tráfico en tiempo de ejecución determina la velocidad de moderación de interrupciones.|
|iovQueuePairsRequested| El número de pares de colas de hardware asignados a una función virtual de SR-IOV. Si se requiere el ajuste \(de\) escala en lado de recepción RSS y el adaptador de red físico que se enlaza al conmutador virtual admite RSS en las funciones virtuales de SR-IOV, se necesita más de un par de cola. <p>Valores permitidos: de 1 a 4294967295.|
|QosSettings| Configure las siguientes opciones de QoS, que son opcionales: <ul><li>**outboundReservedValue** : si outboundReservedMode es "Absolute", el valor indica el ancho de banda, en Mbps, garantizado en el puerto virtual para la transmisión (salida). Si outboundReservedMode es "Weight", el valor indica la parte ponderada del ancho de banda garantizado.</li><li>**outboundMaximumMbps** : indica el ancho de banda máximo permitido del lado de envío, en Mbps, para el puerto virtual (salida).</li><li>**InboundMaximumMbps** : indica el ancho de banda de recepción máximo permitido para el puerto virtual (entrada) en Mbps.</li></ul> |

---