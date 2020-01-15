---
title: Redes de alto rendimiento
description: En este tema se proporciona información general sobre las tecnologías de descarga y optimización en Windows Server 2016, e incluye vínculos a instrucciones adicionales sobre estas tecnologías.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: d5a4d5f06cd433fa92c617a3cb36e95d09be3b27
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950451"
---
# <a name="hardware-only-ho-features-and-technologies"></a>Tecnologías y características de solo hardware

Estas aceleraciones de hardware mejoran el rendimiento de la red junto con el software, pero no forman parte de forma íntima de ninguna característica de software. Algunos ejemplos son moderación de interrupciones, control de flujo y descarga de suma de comprobación IPv4 del lado de recepción.

>[!TIP]
>Las características SH y HO están disponibles si la NIC instalada lo admite. En las descripciones de las características siguientes se explicará cómo saber si la NIC es compatible con la característica.

## <a name="address-checksum-offload"></a>Descarga de suma de comprobación de dirección

La descarga de la suma de comprobación de direcciones es una característica de NIC que descarga el cálculo de las sumas de comprobación de direcciones (IP, TCP y UDP) en el hardware de NIC para enviar y recibir.

En la ruta de acceso de recepción, la descarga de la suma de comprobación calcula las sumas de comprobación de los encabezados IP, TCP y UDP (según corresponda) e indica al sistema operativo Si se han superado o no las sumas de comprobación. Si la NIC valida que las sumas de comprobación son válidas, el sistema operativo acepta el paquete incuestionado. Si la NIC valida las sumas de comprobación no válidas o no se comprueban, la pila IP/TCP/UDP calcula internamente de nuevo las sumas de comprobación. Si se produce un error en la suma de comprobación calculada, el paquete se descarta.

En la ruta de acceso de envío, la descarga de la suma de comprobación calcula e inserta las sumas de comprobación en el encabezado IP, TCP o UDP según corresponda.

Deshabilitar descarga de suma de comprobación en la ruta de acceso de envío no deshabilita el cálculo y la inserción de suma de comprobación para los paquetes enviados al controlador de minipuerto mediante la característica de descarga de envío grande (LSO).  Para deshabilitar todos los cálculos de descarga de suma de comprobación, el usuario también debe deshabilitar la LSO.

_**Administrar descargas de suma de comprobación de direcciones**_

En las propiedades avanzadas hay varias propiedades distintas:

-   Descarga de suma de comprobación IPv4

-   Descarga de suma de comprobación TCP (IPv4)

-   Descarga de suma de comprobación TCP (IPv6)

-   Descarga de suma de comprobación UDP (IPv4)

-   Descarga de suma de comprobación UDP (IPv6)

De forma predeterminada, siempre están habilitadas. Se recomienda habilitar siempre todas estas descargas.

La descarga de la suma de comprobación se puede administrar mediante los cmdlets enable-NetAdapterChecksumOffload y Disable-NetAdapterChecksumOffload. Por ejemplo, el siguiente cmdlet habilita los cálculos de suma de comprobación de TCP (IPv4) y UDP (IPv4):

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**Sugerencias sobre el uso de descargas de suma de comprobación de direcciones**_

La descarga de la suma de comprobación de direcciones siempre debe estar habilitada independientemente de la carga de trabajo o circunstancia. Esta más básica de todas las tecnologías de descarga siempre mejora el rendimiento de la red. También se requiere la descarga de la suma de comprobación para que otras descargas sin estado funcionen, como el ajuste de escala en lado de recepción (RSS), la fusión de segmentos de recepción (RSC) y la descarga de envío grande (LSO).

## <a name="interrupt-moderation-im"></a>Moderación de interrupciones (IM)

IM almacena en búfer varios paquetes recibidos antes de interrumpir el sistema operativo. Cuando una NIC recibe un paquete, inicia un temporizador. Cuando el búfer está lleno o el temporizador expira, lo que suceda primero, la NIC interrumpe el sistema operativo. 

Muchas NIC admiten algo más que ON/OFF para la moderación de interrupciones. La mayoría de las NIC admiten los conceptos de una tasa baja, media y alta para la mensajería instantánea. Las distintas tasas representan temporizadores más cortos o largos y ajustes de tamaño de búfer adecuados para reducir la latencia (moderación baja de interrupción) o reducir interrupciones (moderación de interrupción alta).

Hay un equilibrio entre reducir las interrupciones y retrasar excesivamente la entrega de paquetes. Por lo general, el procesamiento de paquetes es más eficaz con la moderación de interrupciones habilitada. Las aplicaciones de alto rendimiento o de baja latencia pueden necesitar evaluar el impacto de la deshabilitación o la reducción de la moderación de interrupciones.

## <a name="jumbo-frames"></a>Tramas gigantes

Las tramas gigantes son una característica de red y NIC que permite a una aplicación enviar fotogramas que son mucho mayores que los 1500 bytes predeterminados. Normalmente, el límite de tramas gigantes es aproximadamente 9000 bytes, pero puede ser menor.

No hubo cambios en la compatibilidad con el trama gigante en Windows Server 2012 R2.

En Windows Server 2016 hay una nueva descarga: MTU_for_HNV. Esta nueva descarga funciona con la configuración de tramas Jumbo para garantizar que el tráfico encapsulado no requiere la segmentación entre el host y el conmutador adyacente. Esta nueva característica de la pila de SDN tiene la NIC que calcula automáticamente qué MTU se debe anunciar y qué MTU se debe usar en la conexión. Estos valores de MTU son diferentes si se está usando cualquier descarga de HNV. (En la tabla de compatibilidad de características, la tabla 1, MTU_for_HNV tendría las mismas interacciones que las descargas de HNVv2, ya que está directamente relacionada con las descargas de HNVv2).

## <a name="large-send-offload-lso"></a>Descarga de envío grande (LSO)

LSO permite a una aplicación pasar un gran bloque de datos a la NIC y la NIC divide los datos en paquetes que caben dentro de la unidad de transmisión máxima (MTU) de la red.

## <a name="receive-segment-coalescing-rsc"></a>Fusión de segmentos de recepción (RSC)

La fusión de segmentos de recepción, también conocida como descarga de recepción de gran tamaño, es una característica de NIC que toma paquetes que forman parte de la misma secuencia que llegan entre interrupciones de red y los fusiona en un solo paquete antes de entregarlos al sistema operativo. RSC no está disponible en las NIC que están enlazadas al conmutador virtual de Hyper-V. Para obtener más información, vea [combinación de segmentos de recepción (RSC)](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).