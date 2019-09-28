---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Servicio de hora de Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: e3dbaa188426ac81073e706db3adc6ab0a655c01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405183"
---
# <a name="windows-time-service-w32time"></a>Servicio de hora de Windows (W32Time)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

El servicio de hora de Windows (W32Time) sincroniza la fecha y la hora de todos los equipos que se ejecutan en Active Directory Domain Services (AD DS). La sincronización de la hora es fundamental para el funcionamiento correcto de muchos servicios de Windows y aplicaciones de línea de negocio (LOB). El servicio de hora de Windows usa el protocolo de tiempo de red (NTP) para sincronizar los relojes de los equipos de la red. NTP garantiza que se puede asignar un valor de reloj exacto, o una marca de tiempo, a las solicitudes de validación de red y de acceso a recursos.

En el tema servicio de hora de Windows (W32Time), está disponible el siguiente contenido:
- **[Windows Server 2016 hora exacta](accurate-time.md).** La precisión de la sincronización de la hora en Windows Server 2016 se ha mejorado considerablemente, al tiempo que se mantiene la compatibilidad con NTP totalmente atrás con versiones anteriores de Windows. En condiciones de funcionamiento razonables, puede mantener una precisión de 1 MS con respecto a la hora UTC o superior para los miembros de dominio de actualización de aniversario de Windows Server 2016 y Windows 10.
- **[Límite de compatibilidad para entornos de alta precisión](support-boundary.md).** En este artículo se describen los límites de compatibilidad del servicio de hora de Windows (W32Time) en entornos que requieren una hora de sistema muy precisa y estable.
- **[Configuración de sistemas para una precisión alta](configuring-systems-for-high-accuracy.md).** La sincronización de la hora en Windows 10 y Windows Server 2016 se ha mejorado considerablemente.  En condiciones de funcionamiento razonables, los sistemas se pueden configurar para mantener una precisión de 1 ms (milisegundos) o superior (con respecto a la hora UTC).
- **[Hora de Windows para la rastreabilidad](windows-time-for-traceability.md).** Las regulaciones en muchos sectores requieren que se pueda realizar un seguimiento de los sistemas a UTC.  Esto significa que se puede atestiguar el desplazamiento de un sistema con respecto a la hora UTC.  Para habilitar escenarios de cumplimiento normativo, Windows 10 y Server 2016 proporcionan nuevos registros de eventos para proporcionar una imagen desde la perspectiva del sistema operativo para comprender las acciones realizadas en el reloj del sistema.  Estos registros de eventos se generan continuamente para el servicio de hora de Windows y se pueden examinar o archivar para su posterior análisis.
- **[Referencia técnica del servicio de hora de Windows](windows-time-service-tech-ref.md).** El servicio W32Time proporciona sincronización de reloj de red para los equipos sin necesidad de una configuración extensa. El servicio W32Time es esencial para el correcto funcionamiento de la autenticación Kerberos V5 y, por lo tanto, en la autenticación basada en AD DS.
    - **[Cómo funciona el servicio de hora de Windows](How-the-Windows-Time-Service-Works.md).** Aunque el servicio de hora de Windows no es una implementación exacta del Protocolo de tiempo de red (NTP), utiliza el conjunto de algoritmos complejo que se define en las especificaciones de NTP para asegurarse de que los relojes de los equipos de una red sean lo más precisos posible.
    - **[Herramientas y configuración del servicio de hora de Windows](Windows-Time-Service-Tools-and-Settings.md).** La mayoría de los equipos miembros de dominio tienen un tipo de cliente de hora de NT5DS, lo que significa que sincronizan la hora desde la jerarquía de dominios. La única excepción típica es el controlador de dominio que funciona como maestro de operaciones del emulador del controlador de dominio principal (PDC) del dominio raíz del bosque, que normalmente se configura para sincronizar la hora con un origen de hora externo.


## <a name="related-topics"></a>Temas relacionados
Para obtener más información acerca de la jerarquía de dominios y el sistema de puntuación, vea el [tema "¿Qué es el servicio de hora de Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) . .

El modelo de complemento del proveedor de hora de Windows se [documenta en TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

[Aquí](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf) puede descargarse un anexo al que se hace referencia en el artículo de hora exacta de Windows 2016

Para obtener una introducción rápida al servicio de hora de Windows, eche un vistazo a este [vídeo de información general de alto nivel](https://aka.ms/WS2016TimeVideo).
