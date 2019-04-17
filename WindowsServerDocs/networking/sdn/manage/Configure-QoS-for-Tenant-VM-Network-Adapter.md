---
title: Configurar la calidad de servicio (QoS) para un adaptador de red de la máquina virtual de inquilino
description: Este tema es parte de la guía definido redes Software sobre cómo administrar cargas de trabajo de inquilino y redes virtuales en Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: cde4e21beaec58a98a5d5fbe5c4631e2f113dbf7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configurar la calidad de servicio (QoS) para un adaptador de red de la máquina virtual de inquilino

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Cuando configuras QoS para un adaptador de red de la máquina virtual de inquilinos, tienes una elección entre puentes de centro de datos \ (DCB\) o Software definido redes \(SDN\) QoS.

1.  **DCB**. Puedes configurar DCB mediante los cmdlets de Windows PowerShell NetQoS. Por ejemplo, consulta la sección "Habilitar puente de centro de datos" en el tema [memoria de acceso directo remoto (RDMA) y cambiar incrustado agrupación (conjunto)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.  **SDN QoS**. Puedes habilitar SDN QoS mediante el controlador de red, que se puede establecer para limitar el ancho de banda de una interfaz virtual para impedir que otros usuarios de bloqueo una máquina virtual de alto volumen de tráfico.  También puedes configurar SDN QoS para reservar una cantidad concreta de ancho de banda para una máquina virtual para garantizar que la máquina virtual será accesible independientemente de la cantidad de tráfico de red.  

Todas las configuraciones de Qos SDN se aplican a través de la configuración del puerto de las propiedades de interfaz de red según la siguiente tabla.

|Nombre de elemento|Descripción|
|------------|-----------| 
|macSpoofing|Especifica si las máquinas virtuales pueden cambiar la dirección de origen media access control \(MAC\) en paquetes salientes a una dirección MAC que no se asigna a la máquina virtual. Permite valores son "enabled" \ (lo que permite la máquina virtual usar un address\ MAC diferente) y "deshabilitada" \ (lo que permite la máquina virtual usar solo la dirección MAC que se asigna a it\).|
|arpGuard|Especifica si está habilitado guard ARP.  Guard ARP permite solo las direcciones que se especifican en ArpFilter para pasar a través del puerto.  Valores permitidos son "enabled" o "deshabilitados".
|dhcpGuard|Especifica si se debe colocar mensajes DHCP de una máquina virtual que dice ser un servidor DHCP. Valores permitidos son "enabled", que coloca mensajes DHCP porque el servidor DHCP virtualizado se considera que no son de confianza, o "deshabilitada", lo que permite que el mensaje se reciba porque el servidor DHCP virtualizado se considera de confianza.
|stormLimit|Especifica el número de paquetes de difusión, multidifusión y desconocido unidifusión por segundo que puede enviar a través del adaptador de red virtual especificado una máquina virtual. Paquetes de difusión, multidifusión y desconocido unidifusión encima del límite durante ese intervalo de un segundo se eliminan. Un valor de cero \(0\) significa que no hay ningún límite.
|portFlowLimit|Especifica el número máximo de flujos que se pueden ejecutar para el puerto.  Un valor de espacio en blanco o cero \(0\) significa que no hay ningún límite.
|vmqWeight|Especifica si la cola de máquina virtual (VMQ) está habilitada en el adaptador de red virtual. El peso relativo, describe la afinidad del adaptador de red virtual usar VMQ. El intervalo de valor es de 0a 100. Especifica 0 para desactivar VMQ en el adaptador de red virtual.
|iovWeight|Especifica si la virtualización de E/S de raíz única \(SR-IOV\) está habilitado en este adaptador de red virtual. El peso relativo, Establece la afinidad del adaptador de red virtual a la función de SR-IOV virtual asignada. El intervalo del valor es 0a 100. Especifica 0 para desactivar SR-IOV en el adaptador de red virtual. 
|iovInterruptModeration|Especifica el valor de interrupción con moderación para una función para el \(SR-IOV\) virtual de raíz única E/S virtualización asignada a un adaptador de red virtual. Valores permitidos son "default", "adaptable", "off", "baja", "medianos" y "alto".   Si se elige predeterminado, el valor se determina mediante la configuración del proveedor del adaptador de red física.  Si se elige adaptable, la velocidad de moderación de interrupción se basa en el patrón de tráfico en tiempo de ejecución. 
|iovQueuePairsRequested|Especifica el número de pares de cola de hardware que se asignará a una función virtual de SR-IOV. Funciones, a continuación, en más de par de una cola se requiere si es necesario lado recibir escalado \(RSS\), y si el adaptador de red física que se enlaza con el conmutador virtual admite RSS en SR-IOV virtual. Valores permitidos oscilan entre 1 y 4294967295. 
|QosSettings|Puedes configurar la siguiente configuración de Qos, que son todos opcionales:  <br/><br />**outboundReservedValue:**<br/>Si es "absoluta" outboundReservedMode el valor indica el ancho de banda en Mbps, garantizada que el puerto virtual para la transmisión (salida). Si outboundReservedMode es "rebelar", a continuación, el valor indica la parte del ancho de banda garantizado ponderada. <br/><br />**outboundMaximumMbps:**  <br/>Indica que el número máximo permitido ancho de banda de lado de envío, en Mbps, para el puerto virtual (salida). <br/><br/>**InboundMaximumMbps:**  <br/>Indica que el máximo permitido de ancho de banda lateral recibir para el puerto virtual (entrada) en Mbps. |