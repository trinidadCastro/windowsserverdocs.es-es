---
title: Configuración de calidad de servicio (QoS) para un adaptador de red de máquina virtual de inquilino
description: Al configurar QoS para un adaptador de red de máquina virtual de inquilino, puede optar entre el puente del centro de datos \(DCB\)o redes definidas por Software \(SDN\) QoS.
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
ms.openlocfilehash: 0b9ce318c3d249b23d7560e0b6bb90a83e60d64d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880606"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configuración de calidad de servicio (QoS) para un adaptador de red de máquina virtual de inquilino

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Al configurar QoS para un adaptador de red de máquina virtual de inquilino, puede optar entre el puente del centro de datos \(DCB\)o redes definidas por Software \(SDN\) QoS.

1.  **DCB**. Puede configurar mediante los cmdlets de Windows PowerShell NetQoS DCB. Para obtener un ejemplo, vea la sección "Habilitar el puente de centros de datos" en el tema [remoto acceso directo a memoria (RDMA) y Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.  **QoS de SDN**. Puede habilitar la QoS de SDN mediante el uso de la controladora de red, que se pueden establecer para limitar el ancho de banda en una interfaz virtual para evitar que una máquina virtual de tráfico elevado de bloquear a otros usuarios.  También puede configurar QoS de SDN para reservar una cantidad específica de ancho de banda para una máquina virtual para asegurarse de que la máquina virtual sea accesible sin tener en cuenta la cantidad de tráfico de red.  

Se aplican a toda la configuración de QoS de SDN a través de la configuración del puerto de las propiedades de la interfaz de red. Consulte la tabla siguiente para obtener más detalles.

|Nombre del elemento|Descripción|
|------------|-----------| 
|macSpoofing| Permite que las máquinas virtuales cambiar el control de acceso de origen multimedia \(MAC\) direcciones en paquetes salientes a una dirección MAC no está asignado a la máquina virtual.<p>Valores permitidos:<ul><li>Habilitado: Use una dirección MAC diferente.</li><li>Deshabilitado: utilice solo la dirección de MAC asignada a él.</li></ul>|
|arpGuard| Permite la protección de ARP solo las direcciones especificadas en ArpFilter para pasar a través del puerto.<p>Valores permitidos:<ul><li>Habilitada: permitido</li><li>Deshabilitado: no permitido</li></ul>|
|dhcpGuard| Permite o rechaza los mensajes DHCP de una máquina virtual que aparenta ser un servidor DHCP. <p>Valores permitidos:<ul><li>Habilitado: quita los mensajes DHCP porque se considera que el servidor DHCP virtualizado no es de confianza.</li><li>Deshabilitado: permite que el mensaje poder recibir porque el servidor DHCP virtualizado se considere de confianza.</li></ul>|
|stormLimit| El número de paquetes (difusión, multidifusión y unidifusión desconocida) por segundo de una máquina virtual tiene permiso para enviar a través del adaptador de red virtual. Se quitan los paquetes más allá del límite durante ese intervalo de un segundo. Un valor de cero \(0\) significa que no hay ningún límite...|
|portFlowLimit| El número máximo de flujos pueda ejecutarse para el puerto. Un valor de espacio en blanco o cero \(0\) significa que no hay ningún límite. |
|vmqWeight| El peso relativo describe la afinidad del adaptador de red virtual para usar virtual machine queue (VMQ). El intervalo de valor es de 0 a 100.<p>Valores permitidos:<ul><li>0: deshabilita la VMQ en el adaptador de red virtual.</li><li>1-100: habilita la VMQ en el adaptador de red virtual.</li></ul>|
|iovWeight| El peso relativo establece la afinidad del adaptador de red virtual en la virtualización de E/S de raíz única asignada \(SR-IOV\) función virtual. <p>Valores permitidos:<ul><li>0: deshabilita la SR-IOV en el adaptador de red virtual.</li><li>1-100 – permite SR-IOV en el adaptador de red virtual.</li></ul>|
|iovInterruptModeration|<p>Valores permitidos:<ul><li>valor predeterminado: configuración del proveedor del adaptador de red físico determina el valor.</li><li>adaptable: </li><li>Desactivado </li><li>Baja</li><li>mediano</li><li>Alta</li></ul><p>Si elige **predeterminada**, configuración del proveedor del adaptador de red físico determina el valor.  Si elige, **adaptable**, el tráfico en tiempo de ejecución de patrón determina la velocidad de moderación de interrupción.|
|iovQueuePairsRequested| El número de pares de cola de hardware asignado a una función virtual SR-IOV. Si el lado de recepción escalado \(RSS\) es necesario y si el adaptador de red físico que se enlaza al conmutador virtual admite RSS en las funciones virtuales SR-IOV, entonces se requiere más de un par de cola. <p>Valores permitidos: 1 a 4294967295.|
|QosSettings| Configure las siguientes opciones de Qos, todos ellos son opcionales: <ul><li>**outboundReservedValue** : si es "absolute" outboundReservedMode, a continuación, el valor indica el ancho de banda en Mbps, garantiza que el puerto virtual para la transmisión (salida). Si outboundReservedMode es "peso", a continuación, el valor indica la parte ponderada del ancho de banda garantizado.</li><li>**outboundMaximumMbps** -indica el número máximo permitido ancho de banda del lado de envío, en Mbps, para el puerto virtual (salida).</li><li>**InboundMaximumMbps** -indica el máximo permitido de ancho de banda del lado de recepción para el puerto virtual (entrada) en Mbps.</li></ul> |

---