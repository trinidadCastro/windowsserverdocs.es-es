---
description: 'Más información sobre: Servicio de hora de Windows (W32time)'
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Servicio de hora de Windows (W32time)
author: dahavey
ms.author: dahavey
ms.date: 05/08/2018
ms.topic: article
ms.openlocfilehash: b8b4459b57f66efb2587b1370a5e3018d9c8d359
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047853"
---
# <a name="windows-time-service-w32time"></a>Servicio de hora de Windows (W32time)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

El servicio de hora de Windows (W32Time) sincroniza la fecha y la hora de todos los equipos que se ejecutan en Active Directory Domain Services (AD DS). La sincronización de la hora es fundamental para el funcionamiento correcto de muchos servicios de Windows y aplicaciones de línea de negocio (LOB). El servicio de hora de Windows usa el Protocolo de tiempo de redes (NTP) para sincronizar los relojes de los equipos de una red. NTP garantiza que se pueda asignar un valor de reloj, o una marca de tiempo, preciso a las solicitudes de validación de red y de acceso a recursos.

En el tema del servicio de hora de Windows (W32Time), está disponible el siguiente contenido:
- **[Hora precisa para Windows Server 2016](accurate-time.md).** La precisión de la sincronización de la hora en Windows Server 2016 ha mejorado considerablemente, a la vez que se mantiene totalmente la compatibilidad de NTP con versiones anteriores de Windows. En condiciones de funcionamiento razonables, puedes mantener una precisión de 1 ms, o menos, con respecto a la hora UTC para los miembros de dominio de Windows Server 2016 y la Actualización de aniversario de Windows 10.
- **[Límite de compatibilidad para entornos de alta precisión](support-boundary.md).** En este artículo se describen los límites de compatibilidad del servicio de hora de Windows (W32Time) en entornos que requieren una hora del sistema muy precisa y estable.
- **[Configuración de sistemas de alta precisión](configuring-systems-for-high-accuracy.md).** La sincronización de la hora en Windows 10 y Windows Server 2016 se ha mejorado considerablemente.  En condiciones de funcionamiento razonables, los sistemas se pueden configurar para mantener una precisión de 1 ms (milisegundos) o mejor (con respecto a la hora UTC).
- **[Hora de Windows para rastreabilidad](windows-time-for-traceability.md).** Las normas de muchos sectores exigen que los sistemas puedan rastrearse según la hora UTC.  Esto significa que se pueda dar fe de la diferencia horaria de un sistema con respecto a UTC.  Para habilitar los escenarios de cumplimiento normativo, Windows 10 y Server 2016 proporcionan nuevos registros de eventos para dar una imagen desde la perspectiva del sistema operativo con el objetivo de comprender las medidas adoptadas en el reloj del sistema.  Estos registros de eventos se generan continuamente para el servicio de hora de Windows y se pueden examinar o archivar para su posterior análisis.
- **[Referencia técnica del servicio de hora de Windows](windows-time-service-tech-ref.md).** El servicio W32Time proporciona sincronización del reloj de red para los equipos sin necesidad de una extensa configuración. El servicio W32Time es esencial para el correcto funcionamiento de la autenticación Kerberos, versión 5, y, por tanto, para la autenticación basada en AD DS.
    - **[Funcionamiento del servicio de hora de Windows](How-the-Windows-Time-Service-Works.md).** Aunque el servicio de hora de Windows no es una implementación exacta del Protocolo de tiempo de redes (NTP), usa el conjunto de algoritmos complejo que se define en las especificaciones de NTP para asegurarse de que los relojes de los equipos de una red sean lo más precisos posible.
    - **[Configuración y herramientas del servicio de hora de Windows](Windows-Time-Service-Tools-and-Settings.md).** La mayoría de los equipos miembros de dominio tienen un tipo de cliente de hora de NT5DS, lo que significa que sincronizan la hora desde la jerarquía de dominios. La única excepción habitual a esto es el controlador de dominio que funciona como maestro de operaciones del emulador del controlador de dominio principal (PDC) del dominio raíz del bosque, que normalmente se configura para sincronizar la hora con un origen de la hora externo.


## <a name="related-topics"></a>Temas relacionados
Para más información sobre la jerarquía de dominios y el sistema de puntuación, consulta la entrada de blog ["¿Qué es el servicio de hora de Windows?"](/archive/blogs/w32time/what-is-windows-time-service) .

El modelo de complementos del proveedor de hora de Windows está [documentado en TechNet](/windows/win32/sysinfo/time-provider).

[Aquí](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf) se puede descargar un anexo al que se hace referencia en el artículo Hora exacta para Windows 2016.

Para obtener una introducción rápida al servicio de hora de Windows, echa un vistazo a este [vídeo de información general](https://aka.ms/WS2016TimeVideo).
