---
ms.assetid: ''
title: Versión preliminar de Insider para características del servicio de hora de Windows en Windows Server 2019
description: Nuevas características del servicio de hora de Windows en Windows Server 2019
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: a0c4d01d095e8052a2192d6b6352732a6fe60919
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871792"
---
# <a name="insider-preview"></a>Versión preliminar de Insider 


## <a name="leap-second-support"></a>Compatibilidad con Leap Second


>Se aplica a: Windows Server 2019 y Windows 10, versión 1809

Un segundo bisiesto es un ajuste ocasional de 1 segundo a la hora UTC. A medida que la rotación de la tierra se ralentiza, la hora [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (una escala de tiempo atómica) difiere de la [hora solar media](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time) o de Astronomical.  Una vez que la hora UTC ha convergido en un máximo de 0,9 segundos, se inserta un [segundo bisiesto](https://en.wikipedia.org/wiki/Leap_second) para mantener la hora UTC sincronizada con el tiempo solar medio.

Los segundos bisiestos han vuelto a ser importantes para cumplir con los requisitos normativos de precisión y rastreabilidad tanto en el Estados Unidos como en la Unión Europea.

Para obtener más información, vea:

-  Nuestro [blog de anuncios](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guía de validación para los [desarrolladores](https://aka.ms/Dev-LeapSecond)

-  Guía de validación para [profesionales de ti](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>Protocolo de tiempo de precisión

>Se aplica a: Windows Server 2019 y Windows 10, versión 1809

Un nuevo proveedor de hora incluido en Windows Server 2019 y Windows 10 (versión 1809) le permite sincronizar el tiempo mediante el protocolo de tiempo de precisión (PTP). A medida que el tiempo se distribuye a través de una red, encuentra un retraso (latencia) que, si no se ha tenido en cuenta, o si no es simétrico, resulta cada vez más difícil comprender la marca de tiempo enviada desde el servidor horario. PTP permite que los dispositivos de red agreguen la latencia introducida por cada dispositivo de red en las mediciones de tiempo, con lo que se proporciona un ejemplo de tiempo mucho más preciso para el cliente de Windows.

Para obtener más información, vea:

-  Nuestro [blog de anuncios](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guía de validación para [profesionales de ti](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>Marca de tiempo de software

>Se aplica a: Windows Server 2019 y Windows 10, versión 1809

Al recibir un paquete de control de tiempo a través de la red desde un servidor horario, debe ser procesado por la pila de red del sistema operativo antes de usarse en el servicio de hora. Cada componente de la pila de red introduce una cantidad variable de latencia que afecta a la precisión de la medición del tiempo.

![marca de tiempo de software](../media/Windows-Time-Service/software-timestamping.png)

Para solucionar este problema, la marca de tiempo de software nos permite marcar paquetes de marca de tiempo antes y después de "componentes de red de Windows" mostrados anteriormente para tener en cuenta el retraso en el sistema operativo.

Para obtener más información, vea:

-  Nuestro [blog de anuncios](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guía de validación para [profesionales de ti](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---