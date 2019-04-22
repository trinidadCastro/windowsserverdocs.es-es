---
title: Redes de alto rendimiento
description: En este tema proporciona información general sobre la descarga y las tecnologías de optimización en Windows Server 2016 e incluye vínculos a guías adicionales acerca de estas tecnologías.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 09e18a41452baa0add8e055ceb22d2f5c2ad886e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815836"
---
## <a name="hardware-only-ho-features-and-technologies"></a>Tecnologías y características de solo hardware

Estos aceleración de hardware, mejora el rendimiento de red junto con el software pero íntimamente no forman parte de cualquier característica de software. Ejemplos de estas incluyen la moderación de interrupciones, flujo de Control y la descarga de suma de comprobación de IPv4 de lado de recepción.

>[!TIP]
>SH y HO características están disponibles si la NIC instalada lo admite. Las siguientes descripciones de característica tratará cómo saber si la NIC es compatible con la característica.

### <a name="address-checksum-offload"></a>Descarga de suma de comprobación de dirección

Descarga de suma de comprobación de direcciones es una característica NIC que descarga el cálculo de las sumas de comprobación de dirección (IP, TCP, UDP) en el hardware NIC para enviar y recibir.

En la ruta de acceso de recepción, la descarga de suma de comprobación calcula las sumas de comprobación en los encabezados IP, TCP y UDP (según corresponda) e indica al sistema operativo si las sumas de comprobación pasaron, error o no protegido. Si la NIC se afirma que las sumas de comprobación son válidas, el sistema operativo acepta el paquete no representan ningún riesgo. Si la NIC aserciones que las sumas de comprobación no son válidas o no activado, la pila TCP/IP o UDP nuevo calcula internamente las sumas de comprobación. Si se produce un error en la suma de comprobación calculada, se descarta el paquete.

En la ruta de acceso de envío, la descarga de suma de comprobación calcula e inserta las sumas de comprobación en el encabezado IP, TCP o UDP, según corresponda.

Deshabilitar las descargas de suma de comprobación en la ruta de acceso de envío no deshabilita el cálculo de suma de comprobación y la inserción de los paquetes enviados al controlador de minipuerto mediante la característica de descarga de envío grande (LSO).  Para deshabilitar todos los cálculos de descarga de suma de comprobación, el usuario también debe deshabilitar LSO.

_**Administrar las descargas de suma de comprobación de dirección**_

En las propiedades avanzadas, hay varias propiedades distintas:

-   Descarga de suma de comprobación de IPv4

-   Descarga de suma de comprobación TCP (IPv4)

-   Descarga de suma de comprobación TCP (IPv6)

-   Descarga de suma de comprobación UDP (IPv4)

-   Descarga de suma de comprobación UDP (IPv6)

De forma predeterminada, estos todo siempre están habilitados. Se recomienda habilitar siempre todas estas descargas.

La descarga de suma de comprobación pueden administrarse mediante los cmdlets Enable-NetAdapterChecksumOffload y Disable-NetAdapterChecksumOffload. Por ejemplo, el siguiente cmdlet habilita el TCP (IPv4) y los cálculos de suma de comprobación UDP (IPv4):

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**Sugerencias sobre el uso de descarga de suma de comprobación de dirección**_

SIEMPRE se debe habilitar la descarga de la suma de comprobación en la dirección independientemente circunstancia o carga de trabajo. Esto descarga basic la mayoría de todas las tecnologías siempre mejoran el rendimiento de red. También es necesaria para otras descargas sin estado para que funcione como la descarga de suma de comprobación (RSS) de escala en lado de recepción, recibir de fusión de segmentos (RSC) y la descarga de envío grande (LSO).

### <a name="interrupt-moderation-im"></a>(IM) de moderación de interrupciones

Mensajería instantánea almacena en búfer varios paquetes recibidos antes de interrumpir el sistema operativo. Cuando una NIC recibe un paquete, inicia un temporizador. Cuando el búfer esté lleno o caduque el temporizador, lo que ocurra primero, la NIC interrumpe el sistema operativo. 

Cuántas NIC admiten más de simplemente activado/desactivado para la moderación de interrupciones. La mayoría de las NIC admiten los conceptos de una baja, Media y alta velocidad para mensajería instantánea. Las tasas diferentes representan acortar o alargar los temporizadores y los ajustes de tamaño de búfer adecuadas para reducir la latencia (moderación de interrupciones baja) o reducir las interrupciones (moderación de interrupción elevado).

Hay un equilibrio verse afectadas entre lo que reduce las interrupciones y excesivamente retrasar la entrega de paquetes. Por lo general, procesamiento de paquetes es más eficaz con moderación de interrupción habilitados. Alto rendimiento o las aplicaciones de baja latencia debe evaluar el impacto de deshabilitar o reducir la moderación de interrupciones.

### <a name="jumbo-frames"></a>Tramas gigantes

Las tramas gigantes es una característica NIC y la red que permite que una aplicación enviar tramas mucho mayores que el valor predeterminado 1500 bytes. Normalmente el límite en las tramas gigantes es aproximadamente de 9000 bytes, pero puede ser menor.

No había ningún cambio en la compatibilidad con tramas gigantes en Windows Server 2012 R2.

En Windows Server 2016, que hay una nueva descarga: MTU_for_HNV. Esta descarga nueva funciona con la configuración del marco gigantes para asegurarse de que el tráfico encapsulado no requiere la segmentación entre el host y el conmutador adyacente. Esta nueva característica de la pila de SDN tiene la NIC se calcula automáticamente qué MTU para anunciar y qué MTU para usar en la conexión. Estos valores para MTU son diferentes si cualquier descarga HNV está en uso. (En la tabla de compatibilidad de características, la tabla 1, MTU_for_HNV tendría las mismas interacciones como descarga el HNVv2 descargas tienen, ya que está directamente relacionado con la HNVv2).

### <a name="large-send-offload-lso"></a>Descarga de envío grande (LSO)

LSO permite que una aplicación pasar de un bloque grande de datos a la NIC y los saltos de NIC de los datos en paquetes que caben dentro de la unidad de transmisión máxima (MTU) de la red.

### <a name="receive-segment-coalescing-rsc"></a>Receive Segment Coalescing (RSC)

Fusión de segmentos de recepción, también conocido como recibir descarga grande, es una característica NIC que toma los paquetes que forman parte de la misma secuencia que llega entre las interrupciones de red y los combina en un único paquete antes de enviarlos al sistema operativo. RSC no está disponible en la NIC que están enlazadas al conmutador Virtual de Hyper-V. Para obtener más información, consulte [Receive segmento Coalescing (RSC)](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).