---
title: Ajuste del rendimiento de puerta de enlace de SLB en redes definidas por software
description: Directrices para la optimización del rendimiento de puerta de enlace de SLB en redes SDN
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9a0d239da2ca321333ec757db22bbaf9a9b8ba30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383460"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>Ajuste del rendimiento de puerta de enlace de SLB en redes definidas por software

El equilibrio de carga de software se proporciona mediante una combinación de un administrador de equilibrador de carga en las máquinas virtuales de la controladora de red, el conmutador virtual de Hyper-V y un conjunto de máquinas virtuales Load Balancer Multixplexor (MUX).

No se requiere ninguna optimización del rendimiento adicional para configurar el controlador de red o el host de Hyper-V para el equilibrio de carga más allá de lo que se describe en la sección [redes definidas por software](index.md) , a menos que vaya a usar SR-IOV para MUX, tal y como se describe a continuación.

## <a name="slb-mux-vm-configuration"></a>Configuración de VM MUX de SLB

Las máquinas virtuales SLB MUX se implementan en una configuración activa-activa.  Esto significa que todas las máquinas virtuales de MUX que se implementan y se agregan a la controladora de red pueden procesar las solicitudes entrantes.  Por lo tanto, el rendimiento agregado total de todas las conexiones solo está limitado por el número de máquinas virtuales de MUX que ha implementado.  

Una conexión individual a una IP virtual (VIP) siempre se enviará al mismo MUX, suponiendo que el número de MUX permanezca constante y, como resultado, su rendimiento se limitará al rendimiento de una sola máquina virtual de Mux.  MUX solo procesa el tráfico entrante destinado a una dirección VIP.  Los paquetes de respuesta van directamente desde la máquina virtual que está enviando la respuesta al conmutador físico que lo reenvía al cliente.

En algunos casos, cuando el origen de la solicitud se origina en un host de SDN que se agrega a la misma controladora de red que administra la VIP, también se realiza una optimización adicional de la ruta de acceso entrante para la solicitud, lo que permite que la mayoría de los paquetes viajen directamente desde el cliente al servidor, omitiendo completamente la máquina virtual de Mux.  No es necesario realizar ninguna configuración adicional para que esta optimización tenga lugar.

Cada máquina virtual MUX de SLB debe tener el tamaño de acuerdo con las instrucciones proporcionadas en la sección requisitos del rol de máquina virtual de infraestructura de SDN del tema [planear una infraestructura de red definida por software](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md) .

## <a name="single-root-io-virtualization-sr-iov"></a>Virtualización de e/s de raíz única (SR-IOV)

Al usar 40 Gbits Ethernet, la capacidad del conmutador virtual para procesar paquetes para la máquina virtual de MUX se convierte en el factor de limitación para el rendimiento de la máquina virtual de Mux.  Debido a esto, se recomienda habilitar SR-IOV en el adaptador de red de VM de la máquina virtual de SLB para asegurarse de que el conmutador virtual no es el cuello de botella.

Para habilitar SR-IOV, debe habilitarlo en el conmutador virtual cuando se cree el conmutador virtual.  En este ejemplo, vamos a crear un conmutador virtual con switch Embedded Teaming (SET) y SR-IOV:
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
A continuación, debe estar habilitado en los adaptadores de red virtual de la máquina virtual del MUX de SLB que procesan el tráfico de datos.  En este ejemplo, se habilita SR-IOV en todos los adaptadores:
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
