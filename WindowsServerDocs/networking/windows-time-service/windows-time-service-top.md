---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Servicio de hora de Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3233434403594ef9e2555c0329c4791d1fb99709
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469589"
---
# <a name="windows-time-service-w32time"></a>Servicio de hora de Windows (W32Time)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

El servicio de hora de Windows (W32Time) sincroniza la fecha y hora para todos los equipos que se ejecutan en servicios de dominio de Active Directory (AD DS). Sincronización de hora es fundamental para el correcto funcionamiento de muchos servicios de Windows y aplicaciones de línea de negocio (LOB). El servicio de hora de Windows usa el protocolo de tiempo de red (NTP) para sincronizar los relojes de los equipos de la red. NTP garantiza que un valor de reloj exacto o marca de tiempo, puede asignarse a recursos y la validación de solicitudes de acceso de red.

En el tema de servicio de hora de Windows (W32Time), está disponible el siguiente contenido:
- **[Hora exacta de Windows Server 2016](accurate-time.md).** Precisión de la sincronización de hora de Windows Server 2016 se ha mejorado notablemente, manteniendo la completa hacia atrás NTP compatibilidad con versiones anteriores de Windows. En condiciones de funcionamiento razonables puede mantener un 1 ms de precisión con respecto a UTC o superior para los miembros de dominio de Windows Server 2016 y Windows 10 Anniversary Update.
- **[Límite de soporte para entornos de alta precisión](support-boundary.md).** En este artículo se describe los límites de compatibilidad para el servicio de hora de Windows (W32Time) en entornos que requieren la hora del sistema estable y de alta precisión.
- **[Configuración de sistemas de alta precisión](configuring-systems-for-high-accuracy.md).** Sincronización de hora de Windows 10 y Windows Server 2016 se ha mejorado considerablemente.  En condiciones de funcionamiento razonables, se pueden configurar los sistemas para mantener 1 ms (milisegundos) precisión o superior (con respecto a UTC).
- **[Hora de Windows para la rastreabilidad](windows-time-for-traceability.md).** Las leyes en muchos sectores requieren sistemas para ser conforme a UTC.  Esto significa que puede recibir atestación desplazamiento de un sistema con respecto a UTC.  Para habilitar escenarios de cumplimiento normativo, Windows 10 y Server 2016 proporciona nuevos registros de eventos para proporcionar una imagen desde la perspectiva del sistema operativo para formar una comprensión de las acciones realizadas en el reloj del sistema.  Estos registros de eventos se generan continuamente para el servicio de hora de Windows y puede examinar o archivados para su análisis posterior.
- **[Referencia técnica del servicio de hora de Windows](windows-time-service-tech-ref.md).** El servicio W32Time proporciona sincronización de relojes de red para equipos sin necesidad de numerosas tareas de configuración. El servicio W32Time es esencial para el correcto funcionamiento de la autenticación Kerberos V5 y, por lo tanto, para autenticación AD DS.
    - **[Cómo funciona el servicio de hora de Windows](How-the-Windows-Time-Service-Works.md).** Aunque el servicio de hora de Windows no es una implementación exacta de protocolo de tiempo de red (NTP), que usa el conjunto de algoritmos complejo que se define en las especificaciones de NTP para asegurarse de que los relojes de los equipos a lo largo de una red son tan precisos como sea posible.
    - **[Configuración y herramientas de servicio de hora de Windows](Windows-Time-Service-Tools-and-Settings.md).** La mayoría de los equipos de miembros de dominio tienen un tipo de cliente de la hora de NT5DS, lo que significa que sincronicen la hora de la jerarquía de dominios. La excepción a esto sólo típica es el controlador de dominio que funciona como el maestro de operaciones de emulador PDC (controlador) de dominio principal del dominio de raíz del bosque, que normalmente está configurado para sincronizar la hora con un origen de hora externo.


## <a name="related-topics"></a>Temas relacionados
Para obtener más información acerca de la jerarquía de dominio y el sistema de puntuación, vea el ["¿Qué es el servicio de hora de Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) .

El modelo de complemento de proveedor de hora de windows es [documentada en TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Se puede descargar un anexo al que hace referencia el artículo de Windows 2016 preciso momento [aquí](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Para una introducción rápida de servicio de hora de Windows, eche un vistazo a este [vídeo de introducción de alto nivel](https://aka.ms/WS2016TimeVideo).
