---
ms.assetid: ''
title: Versión preliminar de Insider para las características del servicio de hora de Windows en Windows Server 2019
description: Nuevas características del servicio de hora de Windows en Windows Server 2019
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: ef0ff317f5957add5ecbe9f88ef83753b805ec41
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829026"
---
# <a name="insider-preview"></a>Versión preliminar de Insider 


## <a name="leap-second-support"></a>Soporte técnico de segundo intercalar


>Se aplica a: 2019 de Windows Server y Windows 10, versión 1809

Un segundo intercalar es un ajuste de 1 segundo ocasional a UTC. Como se ralentiza la rotación de la tierra, [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (una escala de tiempo atómica) difiere de [Greenwich solar](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time) u hora astronómico.  Una vez que se ha dividido UTC.9 segundos como máximo, un [segundo intercalar](https://en.wikipedia.org/wiki/Leap_second) se inserta para mantener UTC sincronizado con el tiempo medio de solar.

Se han vuelto importantes para cumplir los requisitos de las normas de rastreabilidad tanto en Estados Unidos y la Unión Europea y precisión los segundos intercalares.

Para obtener más información, vea:

-  Nuestro [blog del anuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guía de validación para el [a los desarrolladores](https://aka.ms/Dev-LeapSecond)

-  Guía de validación para el [IT Pro](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>Protocolo de tiempo de precisión

>Se aplica a: 2019 de Windows Server y Windows 10, versión 1809

Un nuevo proveedor de hora incluido en Windows Server 2019 y Windows 10 (versión 1809) permite sincronizar la hora con el protocolo de tiempo de precisión (PTP). Como el tiempo que se distribuye a través de una red, encuentra retraso (latencia), qué if no tienen en cuenta, o si no es simétrica, se vuelve cada vez más difícil de entender la marca de tiempo que se envían desde el servidor horario. PTP permite a los dispositivos de red agregar la latencia introducida por cada dispositivo de red en las medidas de tiempo, lo que proporciona un ejemplo de tiempo mucho más preciso al cliente de windows.

Para obtener más información, vea:

-  Nuestro [blog del anuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guía de validación para el [IT Pro](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>Marca de tiempo de software

>Se aplica a: 2019 de Windows Server y Windows 10, versión 1809

Al recibir un paquete de control de tiempo en la red desde un servidor horario, debe procesarse por la pila de red del sistema operativo antes de que se consume en el servicio de hora. Cada componente de la pila de red presenta una cantidad de latencia que afecta a la precisión de la medición de tiempo variable.

![marca de tiempo de software](../media/Windows-Time-Service/software-timestamping.png)

Para solucionar este problema, marca de tiempo de software nos permite a los paquetes de marca de tiempo antes y después de la "Windows componentes de red" mostrado anteriormente para tener en cuenta el retraso en el sistema operativo.

Para obtener más información, vea:

-  Nuestro [blog del anuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guía de validación para el [IT Pro](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---