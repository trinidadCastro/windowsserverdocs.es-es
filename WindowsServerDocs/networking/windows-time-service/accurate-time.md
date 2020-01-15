---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Hora precisa para Windows Server 2016
description: La precisión de la sincronización de la hora en Windows Server 2016 se ha mejorado considerablemente, al tiempo que se mantiene la compatibilidad con NTP totalmente atrás con versiones anteriores de Windows.
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 2e8e9e86f81596c85219c37c07d8fd2e95cc3a49
ms.sourcegitcommit: 10331ff4f74bac50e208ba8ec8a63d10cfa768cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "75953077"
---
# <a name="accurate-time-for-windows-server-2016"></a>Hora precisa para Windows Server 2016

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

El servicio de hora de Windows es un componente que utiliza un modelo de complemento para los proveedores de sincronización de tiempo de cliente y de servidor.  Hay dos proveedores de clientes integrados en Windows y hay complementos de terceros disponibles. Un proveedor usa [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) o [MS-NTP](https://msdn.microsoft.com/library/cc246877.aspx) para sincronizar la hora del sistema local con un servidor de referencia compatible con NTP o MS-NTP. El otro proveedor es para Hyper-V y sincroniza las máquinas virtuales (VM) con el host de Hyper-V.  Cuando existan varios proveedores, Windows elegirá el mejor proveedor mediante el nivel de capa primero, seguido del retraso raíz, la dispersión raíz y, por último, el desplazamiento de la hora.

> [!NOTE]
> Para obtener información general rápida del servicio Horario de Windows, eche un vistazo a este [vídeo de información general de alto nivel](https://aka.ms/WS2016TimeVideo).

En este tema, trataremos... estos temas se refieren a la habilitación del tiempo exacto: 

- Mejoras a la
- Medidas
- Procedimientos recomendados

> [!IMPORTANT]
> [Aquí](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)puede descargarse un anexo al que se hace referencia en el artículo de hora exacta de Windows 2016.  En este documento se proporcionan más detalles sobre nuestras metodologías de pruebas y medidas.

> [!NOTE] 
> El modelo de complemento del proveedor de hora de Windows se [documenta en TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

## <a name="domain-hierarchy"></a>Jerarquía de dominios
Las configuraciones de dominio y independientes funcionan de manera diferente.

- Los miembros del dominio usan un protocolo NTP seguro, que usa la autenticación de para garantizar la seguridad y la autenticidad de la referencia de hora.  Los miembros de dominio se sincronizan con un reloj maestro determinado por la jerarquía de dominios y un sistema de puntuación.  En un dominio, hay una capa jerárquica de transformaciones de tiempo, en la que cada DC apunta a un controlador de dominio principal con un estrato de tiempo más preciso.  La jerarquía se resuelve en el PDC o en un controlador de dominio del bosque raíz, o un controlador de dominio con la marca de dominio GTIMESERV, que indica un buen servidor de tiempo para el dominio.  Vea la sección [Especificación de un servicio de hora local confiable con GTIMESERV a continuación.

- Las máquinas independientes están configuradas para usar time.windows.com de forma predeterminada.  El servidor DNS resuelve este nombre, que debe apuntar a un recurso propiedad de Microsoft.  Al igual que todas las referencias de tiempo localizadas de forma remota, las interrupciones de red pueden impedir la sincronización.  Las cargas de tráfico de red y las rutas de acceso de red asimétricas pueden reducir la precisión de la sincronización de la hora.  Para una precisión de 1 MS, no puede depender de orígenes de hora remotos.

Como los invitados de Hyper-V van a tener al menos dos proveedores de hora de Windows entre los que elegir, la hora del host y el NTP, es posible que vea comportamientos diferentes con cualquiera de los dominios o de forma independiente cuando se ejecute como invitado.

> [!NOTE] 
> Para obtener más información acerca de la jerarquía de dominios y el sistema de puntuación, vea el [tema "¿Qué es el servicio de hora de Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) . .

> [!NOTE]
> Capa es un concepto que se usa en los proveedores de NTP y Hyper-V, y su valor indica la ubicación de los relojes en la jerarquía.  La capa 1 está reservada para el reloj de nivel superior y la capa 0 está reservada para el hardware que se supone que es preciso y tiene poco o ningún retraso asociado.  El estrato 2 se comunica con los servidores de estrato 1, de estrato 3 a estrato 2, etc.  Aunque una capa inferior suele indicar un reloj más preciso, es posible encontrar discrepancias.  Además, W32time solo acepta el tiempo del estrato 15 o inferior.  Para ver la capa de un cliente, use *W32tm/Query/status*.

## <a name="critical-factors-for-accurate-time"></a>Factores críticos para un tiempo preciso
En cada caso para un tiempo preciso, hay tres factores críticos:

1. **Reloj de origen sólido** : el reloj de origen del dominio debe ser estable y preciso. Esto suele significar la instalación de un dispositivo GPS o la señalización de un origen de estrato 1, que toma #3 en cuenta. La analogía se dirige, si tiene dos botes sobre el agua y está intentando medir la altitud de uno en comparación con el otro, su precisión es mejor si el bote de origen es muy estable y no se mueve. Lo mismo ocurre para el tiempo y, si el reloj de origen no es estable, toda la cadena de relojes sincronizados se ve afectada y se amplía en cada fase. También debe ser accesible porque las interrupciones en la conexión interfieren con la sincronización de la hora. Y, por último, debe ser seguro. Si la referencia de hora no se mantiene correctamente u opera mediante una entidad potencialmente malintencionada, podría exponer su dominio a ataques basados en tiempo.
2. **Reloj de cliente estable** : un reloj de cliente estable garantiza que se contenga el desplazamiento natural del oscilador.  NTP usa varios ejemplos de varios servidores NTP para condicionar la condición y la disciplina del reloj de los equipos locales.  No realiza el paso de los cambios de tiempo, sino que se ralentiza o acelera el reloj local para que se acerque el tiempo exacto rápidamente y se mantenga una precisión entre las solicitudes de NTP.  Sin embargo, si el oscilador del reloj del equipo cliente no es estable, se pueden producir más fluctuaciones entre los ajustes y los algoritmos que Windows usa para que el reloj no funcione correctamente.  En algunos casos, es posible que se necesiten actualizaciones de firmware para un tiempo preciso.
3. **Comunicación NTP simétrica** : es fundamental que la conexión para la comunicación NTP sea simétrica.  NTP usa cálculos para ajustar el tiempo que supone que la revisión de red es simétrica.  Si la ruta de acceso del paquete NTP al servidor tarda un tiempo diferente en devolverse, la precisión se verá afectada.  Por ejemplo, la ruta de acceso podría cambiar debido a cambios en la topología de red, o a que los paquetes se enruten a través de dispositivos que tienen diferentes velocidades de interfaz.

En el caso de los dispositivos con batería, tanto móviles como portátiles, debe tener en cuenta diferentes estrategias.  Según nuestra recomendación, mantener el tiempo preciso requiere que el reloj esté disciplinado una vez por segundo, lo que se correlaciona con la frecuencia de actualización del reloj. Esta configuración utilizará más energía de la batería de lo esperado y puede interferir con los modos de ahorro de energía disponibles en Windows para dichos dispositivos. Los dispositivos con batería también tienen determinados modos de energía que impedirán que se ejecuten todas las aplicaciones, lo que interfiere con la capacidad de W32time's para disciplinar el reloj y mantener un tiempo preciso. Además, es posible que los relojes de los dispositivos móviles no sean muy precisos para comenzar.  Las condiciones ambientales ambiente afectan a la precisión del reloj y un dispositivo móvil puede pasar de una condición ambiente a la siguiente, lo que puede interferir con la capacidad de mantener la precisión del tiempo.  Por lo tanto, Microsoft no recomienda que configure dispositivos portátiles con batería con una configuración de precisión alta. 

## <a name="why-is-time-important"></a>¿Por qué es importante el tiempo?  
Hay muchas razones diferentes que podría necesitar una hora precisa.  El caso típico de Windows es Kerberos, que requiere una precisión de 5 minutos entre el cliente y el servidor.  Sin embargo, hay muchas otras áreas que pueden verse afectadas por la precisión temporal, como:


- Regulaciones gubernamentales como:
    - 50 ms de precisión para FINRA en EE. UU.
    - 1 ms ESMA (MiFID II) en la Unión Europea.
- Algoritmos de criptografía
- Sistemas distribuidos como Cluster/SQL/Exchange y Document DBs
- Blockchain Framework para transacciones de Bitcoin
- Registros distribuidos y análisis de amenazas 
- Replicación de AD
- PCI (industria de tarjeta de pago), actualmente una precisión de 1 segundo



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
