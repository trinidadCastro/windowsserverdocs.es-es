---
title: Rendimiento de la puerta de enlace SLB de optimización en el Software de las redes definidas
description: Directrices de redes SDN para la optimización de rendimiento de puerta de enlace de SLB
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fede7d404ddbb4f465eff435cc340db1907ce9d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829936"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>Rendimiento de la puerta de enlace SLB de optimización en el Software de las redes definidas

Equilibrio de carga de software se proporciona mediante una combinación de un administrador del equilibrador de carga en las máquinas virtuales de controlador de red, el conmutador Virtual de Hyper-V y un conjunto de máquinas virtuales Multixplexor de equilibrador de carga (Mux).

Ninguna optimización del rendimiento adicional es necesaria para configurar la controladora de red o el host de Hyper-V para equilibrar más allá de lo que se describe en el [redes definidas por Software](index.md) sección a menos que se va a usar SR-IOV para el MUX tal como se describe a continuación.

## <a name="slb-mux-vm-configuration"></a>Configuración de máquina virtual de Mux SLB

Las máquinas virtuales de SLB/Mux se implementan en una configuración activo / activo.  Esto significa que cada VM Mux que se implementan y se agrega a la controladora de red puede procesar las solicitudes entrantes.  Por lo tanto, el rendimiento agregado total de todas las conexiones solo está limitado por el número de máquinas virtuales Mux de que haya implementado.  

Una conexión individual a una IP Virtual (VIP) siempre se enviarán a la mismo Mux, suponiendo que el número de MUX permanece constante y, como resultado se limitará al rendimiento de una sola VM Mux su capacidad de proceso.  Mux de procesa solo el tráfico entrante que se destina a una dirección VIP.  Paquetes de respuesta de ir directamente desde la máquina virtual que envía la respuesta al conmutador físico que lo reenvía al cliente.

En algunos casos cuando el origen de la solicitud se origina en un host SDN que se agrega a la misma controladora de red que administra la dirección VIP, mejorar la optimización de la ruta de acceso de entrada para la solicitud también se realiza lo que permite la mayoría de los paquetes para desplazarse directamente desde el cliente en el servidor, omitiendo la VM Mux completamente.  Ninguna configuración adicional es necesaria para esta optimización tenga lugar.

Cada máquina virtual Mux de SLB deben ajustarse según las instrucciones proporcionadas en la sección de requisitos SDN rol de máquina virtual de infraestructura de la [planear una infraestructura de red definida por Software](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md) tema.

## <a name="single-root-io-virtualization-sr-iov"></a>Virtualización de E/S de raíz única (SR-IOV)

Cuando se usa 40Gbit Ethernet, la posibilidad de que el conmutador virtual en los paquetes de proceso para la VM Mux pasa a ser el factor de limitación para el rendimiento de la VM Mux.  Por este motivo, se recomienda que SR-IOV esté habilitado en el adaptador de red de VM de SLB VM para asegurarse de que el conmutador virtual no es el cuello de botella.

Para habilitar SR-IOV, debe habilitarla en el conmutador virtual cuando se crea el conmutador virtual.  En este ejemplo, vamos a crear un conmutador virtual con SR-IOV y switch embedded teaming (SET):
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
A continuación, debe habilitarse en los adaptadores de red virtual de la máquina virtual Mux de SLB que procesan el tráfico de datos.  En este ejemplo, se está habilitando SR-IOV en todos los adaptadores:
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
