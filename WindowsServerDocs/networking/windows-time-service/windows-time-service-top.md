---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Servicio hora de Windows
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b997d1f26e8da82e0d595b1ce13763e0a87d6d03
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="windows-time-service-w32time"></a>Servicio hora de Windows (W32Time)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El servicio hora de Windows (W32Time) sincroniza la fecha y hora para todos los equipos en los servicios de dominio de Active Directory (AD DS). Sincronización de hora es fundamental para el correcto funcionamiento de muchos de los servicios de Windows y aplicaciones de línea de negocio (LOB). El servicio hora de Windows usa el protocolo de tiempo de red (NTP) para sincronizar los relojes de los equipos de la red. NTP garantiza que se puede asignar un valor de reloj exacto o marca de tiempo, las solicitudes de acceso de validación y recursos de red.

En el tema de servicio hora de Windows (W32Time), el siguiente contenido está disponible:
- **[Windows 2016 precisa tiempo ](accurate-time.md).** Precisión de la sincronización de hora en Windows Server 2016 se ha mejorado considerablemente, mientras mantiene completa la NTP compatibilidad con versiones anteriores de Windows.  En condiciones operativas razonables puede mantener un 1 ms precisión con respecto a la hora UTC o superior para los miembros del dominio de Windows Server 2016 y actualización de aniversario de Windows 10.
- **[Referencia técnica del servicio hora de Windows ](windows-time-service-tech-ref.md).** El servicio de W32Time proporciona sincronización de reloj de red para los equipos sin necesidad de configuración amplia. El servicio W32Time es fundamental para el funcionamiento correcto de autenticación Kerberos V5 y, por lo tanto, para la autenticación de AD DS.
    - **[Cómo funciona el servicio hora de Windows ](How-the-Windows-Time-Service-Works.md).** Aunque el servicio hora de Windows no es una implementación exacta de protocolo de tiempo de red (NTP), usa el conjunto de algoritmos complejo que se define en las especificaciones de NTP para garantizar que los relojes de los equipos de una red sean lo más precisos posible.
    - **[Herramientas de servicio hora de Windows y configuración ](Windows-Time-Service-Tools-and-Settings.md).** La mayoría de los equipos de miembro de dominio tienen un tipo de cliente de tiempo de NT5DS, lo que significa que sincronicen la hora de la jerarquía de dominios. La excepción a esto solo típica es el controlador de dominio que funciona como el maestro de operaciones de emulador de dominio principal (PDC) del controlador de dominio de raíz del bosque, que generalmente está configurado para sincronizar la hora con una fuente externa.

## <a name="related-topics"></a>Temas relacionados
Para obtener más información sobre la jerarquía de dominio y sistema de puntuación, consulta el ["¿Qué es servicio hora de Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) Entrada de blog.

El modelo de complemento de proveedor de tiempo de windows es [documentado en TechNet](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Un apéndice al que hace referencia el artículo de tiempo preciso de 2016 de Windows puede descargarse [aquí](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Para ver una introducción rápida de servicio hora de Windows, echa un vistazo a este [descripción general de vídeo ](https://aka.ms/WS2016TimeVideo).

<!-- In this guide
In this guide:
Windows Accurate Time
High Accuracy
Support Boundary
Configuration for High Accuracy
Traceability for Compliance
Best Practices
Technical Reference
How the Windows Time Service Works
Windows Time Service Tools and Settings
-->

