---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Hora precisa para Windows Server 2016
description: La precisión de la sincronización de la hora en Windows Server 2016 ha mejorado considerablemente, a la vez que se mantiene totalmente la compatibilidad de NTP con versiones anteriores de Windows.
author: dcuomo
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.openlocfilehash: 26e8183414da80009ecf28829b833ca3c22058a5
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992652"
---
# <a name="accurate-time-for-windows-server-2016"></a>Hora precisa para Windows Server 2016

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

El servicio Hora de Windows es un componente que usa un modelo de complemento para los proveedores de sincronización de la hora de cliente y servidor.  Hay dos proveedores de clientes integrados en Windows y hay complementos de terceros disponibles. Un proveedor usa [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) o [MS-NTP](/openspecs/windows_protocols/ms-sntp/8106cb73-ab3a-4542-8bc8-784dd32031cc) para sincronizar la hora del sistema local con un servidor de referencia compatible con NTP o MS-NTP. El otro proveedor es para Hyper-V y sincroniza las máquinas virtuales (VM) con el host de Hyper-V.  Cuando existan varios proveedores, Windows elegirá el mejor mediante el nivel de estrato en primer lugar, seguido de la demora de raíz, la dispersión de raíz y, por último, la diferencia horaria.

> [!NOTE]
> Para obtener una introducción rápida al servicio de hora de Windows, echa un vistazo a este [vídeo de información general](https://aka.ms/WS2016TimeVideo).

En este tema, hablaremos de... estos temas se refieren a la habilitación de la hora precisa:

- Mejoras
- Medidas
- Procedimientos recomendados

> [!IMPORTANT]
> [Aquí](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf) puedes descargar el anexo al que se hace referencia en el artículo Hora precisa para Windows 2016.  En este documento se proporcionan más detalles sobre nuestras metodologías para las pruebas y medidas.

> [!NOTE]
> El modelo de complementos del proveedor de hora de Windows está [documentado en TechNet](/windows/win32/sysinfo/time-provider).

## <a name="domain-hierarchy"></a>Jerarquía de dominios
Las configuraciones Dominio e Independiente funcionan de manera diferente.

- Los miembros del dominio usan un protocolo NTP seguro, que usa la autenticación para garantizar la seguridad y la autenticidad de la referencia de la hora.  Los miembros del dominio se sincronizan con un reloj maestro determinado por la jerarquía de dominios y un sistema de puntuación.  En un dominio, hay una capa jerárquica de estratos de tiempo, en la que cada controlador de dominio apunta a un controlador de dominio principal con un estrato de tiempo más preciso.  La jerarquía se resuelve en el controlador de dominio principal o un controlador de dominio del bosque raíz, o en un controlador de dominio con la marca de dominio GTIMESERV, que indica un servidor horario adecuado para el dominio.  Consulta la sección Especificación de un servicio de hora local confiable mediante GTIMESERV a continuación.

- Las máquinas independientes están configuradas para usar time.windows.com de forma predeterminada.  El servidor DNS resuelve este nombre, que debe apuntar a un recurso propiedad de Microsoft.  Igual que en todas las referencias de hora ubicadas remotamente, las interrupciones de red pueden impedir la sincronización.  Las cargas de tráfico de red y las rutas de acceso de red asimétricas pueden reducir la precisión de la sincronización de la hora.  Para una precisión de 1 ms, no puedes depender de orígenes de hora remotos.

Como los invitados de Hyper-V van a tener al menos dos proveedores de hora de Windows entre los que elegir, la hora del host y NTP, es posible que observes comportamientos diferentes con la configuración Domino o Independiente cuando ejecutes como invitado.

> [!NOTE]
> Para más información sobre la jerarquía de dominios y el sistema de puntuación, consulta la entrada de blog ["¿Qué es el servicio de hora de Windows?"](/archive/blogs/w32time/what-is-windows-time-service) .

> [!NOTE]
> El estrato es un concepto que se usa en los proveedores de NTP e Hyper-V, y su valor indica la ubicación de los relojes en la jerarquía.  El estrato 1 está reservado para el reloj de nivel superior y el estrato 0 está reservado para el hardware que se supone preciso y que tiene poco o ningún retraso asociado.  El estrato 2 se comunica con los servidores del estrato 1, el estrato 3 con el estrato 2, etc.  Aunque un estrato inferior suele indicar un reloj más preciso, es posible encontrar discrepancias.  Además, W32time solo acepta la hora del estrato 15 o inferior.  Para ver el estrato de un cliente, usa *w32tm /query /status*.

## <a name="critical-factors-for-accurate-time"></a>Factores críticos para una hora precisa
En cada caso de hora precisa, hay tres factores críticos:

1. **Reloj de origen sólido**: el reloj de origen del dominio debe ser estable y preciso. Suele significar que se debe instalar un dispositivo GPS o apuntar a un origen de estrato 1, que tenga en cuenta el número 3. Una analogía: si tienes dos barcos en el agua e intentas medir la altitud de uno en comparación con el otro, la precisión es mejor si el barco de origen es muy estable y no se mueve. Lo mismo ocurre en la hora y, si el reloj de origen no es estable, toda la cadena de relojes sincronizados se ve afectada y aumenta en cada fase. También debe ser accesible porque las interrupciones en la conexión interferirán en la sincronización de la hora. Y, por último, debe ser seguro. Si la referencia de la hora no se mantiene correctamente u la gestiona una entidad potencialmente malintencionada, podrías exponer tu dominio a ataques basados en la hora.
2. **Reloj de cliente estable**: los relojes de cliente estables garantizan que se pueda contener el desplazamiento natural del oscilador.  NTP usa varias muestras de distintos servidores NTP para condicionar y controlar el reloj de los equipos locales.  No aplica cambios de hora, sino que ralentiza o acelera el reloj local para que se aproxime a la hora precisa rápidamente y mantenga la precisión entre las solicitudes NTP.  Sin embargo, si el oscilador del reloj del equipo cliente no es estable, se pueden producir más fluctuaciones entre los ajustes y que los algoritmos que Windows usa para acondicionar el reloj no funcionen correctamente.  En algunos casos, es posible que se necesiten actualizaciones del firmware para conseguir la hora precisa.
3. **Comunicación simétrica de NTP**: es fundamental que la conexión para la comunicación NTP sea simétrica.  NTP usa cálculos para ajustar la hora en los que se supone que la ruta de acceso de red es simétrica.  Si la ruta de acceso del paquete NTP que va al servidor tarda un tiempo diferente en devolverse, la precisión se verá afectada.  Por ejemplo, la ruta de acceso podría cambiar debido a cambios en la topología de red, o a que los paquetes se enruten a través de dispositivos que tienen diferentes velocidades de interfaz.

En el caso de dispositivos que funcionan con batería, tanto móviles como portátiles, debes tener en cuenta diferentes estrategias.  Según nuestra recomendación, mantener la hora precisa requiere que el reloj se controle una vez por segundo, que se correlaciona con la frecuencia de actualización del reloj. Esta configuración consumirá más energía de la batería de lo esperado y puede interferir con los modos de ahorro de energía disponibles en Windows para dichos dispositivos. Los dispositivos que funcionan con batería también tienen algunos modos de energía que detienen la ejecución de todas las aplicaciones, lo que interfiere con la capacidad de W32time para controlar el reloj y mantener una hora precisa. Además, es posible que los relojes de los dispositivos móviles no sean muy precisos para comenzar.  Las condiciones ambientales afectan a la precisión del reloj y un dispositivo móvil puede pasar de una condición ambiental a otra, lo que puede interferir con la capacidad de mantener la precisión de la hora.  Por lo tanto, Microsoft no recomienda que configures los dispositivos portátiles que funcionan con batería con una configuración de precisión alta.

## <a name="why-is-time-important"></a>¿Por qué es importante la hora?
Hay muchos motivos diferentes por los que podrías necesitar una hora precisa.  El caso típico de Windows es Kerberos, que requiere una precisión de 5 minutos entre el cliente y el servidor.  Sin embargo, hay muchas otras áreas que pueden verse afectadas por la precisión de la hora, por ejemplo, las siguientes:


- Regulaciones gubernamentales como:
    - 50 ms de precisión para FINRA en EE. UU.
    - 1 ms para ESMA (MiFID II) en la Unión Europea.
- Algoritmos de criptografía
- Sistemas distribuidos como Cluster/SQL/Exchange y Document DB
- Marco de cadena de bloques para las transacciones de bitcoins
- Registros distribuidos y análisis de amenazas
- Replicación de AD
- PCI (Payment Card Industry), actualmente una precisión de 1 segundo

## <a name="additional-references"></a>Referencias adicionales

- [Mejoras en la precisión temporal para Windows Server 2016](windows-server-2016-improvements.md)