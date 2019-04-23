---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Hora exacta para Windows Server 2016
description: Precisión de la sincronización de hora de Windows Server 2016 se ha mejorado notablemente, manteniendo la completa hacia atrás NTP compatibilidad con versiones anteriores de Windows.
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: ea4d957ee68f14f4568d3cefe664736585e50cce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864016"
---
# <a name="accurate-time-for-windows-server-2016"></a>Hora exacta para Windows Server 2016

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 o posterior

El servicio de hora de Windows es un componente que utiliza un modelo de complementos para proveedores de sincronización de hora de cliente y servidor.  Existen dos proveedores de cliente integrado en Windows, y hay complementos de terceros. Usa un proveedor [NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305) o [MS NTP](https://msdn.microsoft.com/library/cc246877.aspx) para sincronizar la hora del sistema local a un servidor de referencia compatibles NTP o MS-NTP. El otro proveedor es de Hyper-V y máquinas virtuales (VM) en el host de Hyper-V sincroniza.  Cuando existen varios proveedores, Windows creará elegir el mejor proveedor mediante el nivel de los satélites en primer lugar, seguida de retraso de raíz, la dispersión de raíz y la hora de desplazamiento.

>[!NOTE]
>Para una introducción rápida de servicio de hora de Windows, eche un vistazo a este [vídeo de introducción de alto nivel](https://aka.ms/WS2016TimeVideo).

<!-- Not sure what to do with the following -->
En este tema, analizamos... estos temas ya que afectan a la hora exacta de habilitación: 

- Mejoras
- Medidas
- Procedimientos recomendados

>[!IMPORTANT]
>Se puede descargar un anexo al que hace referencia el artículo de Windows 2016 preciso momento [aquí](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf).  Este documento proporciona información detallada acerca de nuestras metodologías de pruebas y mediciones.



>[!NOTE] 
>El modelo de complemento de proveedor de hora de windows es [documentada en TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

## <a name="domain-hierarchy"></a>Jerarquía de dominios
Configuraciones de dominio e independiente funcionan de manera diferente.

- Los miembros del dominio use un protocolo seguro de NTP, que usa la autenticación para garantizar la seguridad y la autenticidad de la referencia de tiempo.  Los miembros del dominio se sincronización con un reloj principal determinado por la jerarquía de dominios y un sistema de puntuación.  En un dominio, hay una capa jerárquica de stratums de tiempo, mediante el cual cada controlador de dominio apunta a un controlador de dominio principal con un estrato más precisa del tiempo.  La jerarquía se resuelve en un controlador de dominio con la marca de dominio GTIMESERV, que denota un buen servidor horario para el dominio o un controlador de dominio del bosque de raíz o el PDC.  Consulte la [especificar un Local confiable tiempo servicio utilizando GTIMESERV](#GTIMESERV) sección más adelante.

- Las máquinas independientes están configuradas para usar time.windows.com de forma predeterminada.  Este nombre se resuelve el servidor DNS, que debe apuntar a una propiedad de recursos de Microsoft.  Al igual que todas las referencias de tiempo remoto específico, interrupciones de red, puede evitar la sincronización.  Carga de tráfico de red y rutas de acceso de red asimétrica pueden reducir la precisión de la sincronización de hora.  Para comprobar su exactitud de 1 ms, no puede depender de orígenes de hora remoto.

Puesto que los invitados de Hyper-V tendrá al menos dos proveedores de hora de Windows para elegir, la hora de host y NTP, puede que vea distintos comportamientos con el dominio o independiente cuando se ejecuta como un invitado.

> [!NOTE] 
> Para obtener más información acerca de la jerarquía de dominio y el sistema de puntuación, vea el ["¿Qué es el servicio de hora de Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) .

> [!NOTE]
> Los satélites es un concepto que se usa en Hyper-V y NTP proveedores, y su valor indica la ubicación de los relojes de la jerarquía.  Los satélites 1 está reservado para el reloj del nivel más alto y los satélites 0 se reserva para el hardware que se supone que sea precisa y tiene poco o ningún retraso asociados con él.  Estrato 2 se comuniquen con servidores de estrato 1, los satélites 3 para los satélites 2 y así sucesivamente.  Mientras que una capa inferior indica a menudo un reloj más preciso, es posible encontrar discrepancias.  Además, W32time solo acepta tiempo desde los satélites 15 o por debajo.  Para ver el estrato de un cliente, use *w32tm /query /status*.

## <a name="critical-factors-for-accurate-time"></a>Factores críticos para la hora exacta
En todos los casos la hora exacta, hay tres factores críticos:

1. **Sólido de origen de reloj** -en sus necesidades de dominio sea estable y preciso, el reloj de origen. Normalmente, esto significa instalar un dispositivo GPS o que apunta a un origen de los satélites 1, habida cuenta de #3. La analogía va, si tiene dos barcos en el agua y está intentando medir la altitud de una comparación con la otra, la precisión es mejor si el bote de origen es muy estable y no móvil. Lo mismo sucede con el tiempo y, si el reloj de origen no es estable, a continuación, toda la cadena de relojes sincronizados es afectada y ampliar en cada fase. También debe ser accesible porque las interrupciones en la conexión interferirá con la sincronización de hora. Y por último, debe ser seguro. Si mantiene el tiempo de referencia no está correctamente o controlado por un tercero potencialmente malintencionado, podría exponer su dominio a los ataques de basado en el tiempo.
2. **El reloj de cliente estable** -relojes del cliente estable garantiza que el desfase del oscilador de natural es containable.  NTP utiliza varias muestras de potencialmente varios servidores NTP para la condición y disciplina el reloj de los equipos locales.  Que no avanza los cambios de tiempo, pero en su lugar se ralentiza o acelera el reloj local para que enfocar el tiempo exacto rápidamente y mantenerse precisas entre las solicitudes NTP.  Sin embargo, si oscilador del reloj del equipo cliente no es estable, pueden producirse más de las fluctuaciones entre los ajustes y los algoritmos de que Windows se usa para el reloj de la condición no funcionan con precisión.  En algunos casos, las actualizaciones de firmware pueden ser necesaria la hora exacta.
3. **Comunicación simétrico de NTP** -es fundamental que la conexión para la comunicación de NTP es simétrica.  NTP utiliza cálculos para ajustar el tiempo que supone que la revisión de seguridad de red es simétrica.  Si la ruta de acceso del paquete NTP toma ir al servidor tarda una cantidad de tiempo en devolver diferente, se ve afectada la precisión.  Por ejemplo, puede cambiar la ruta de acceso debido a cambios en la topología de red o los paquetes se enrutan a través de los dispositivos que tengan velocidades de interfaz diferente.


Para los dispositivos con tecnología de la batería, móviles y portátil, debe considerar estrategias diferentes.  Según nuestra recomendación, mantener la hora exacta requiere el reloj se sea disciplinado una vez por segundo, que se correlaciona con la frecuencia de actualización de reloj. Esta configuración consume más energía de batería de la esperada y puede interferir con ahorro de energía modos disponibles en Windows para estos dispositivos. Dispositivos con tecnología de la batería también tienen ciertos modos de energía que detención todas las aplicaciones que no se ejecuten, que interfiere con la capacidad de W32time disciplina el reloj y mantener la hora exacta. Además, los relojes de los dispositivos móviles pueden no ser muy precisos para empezar.  Condiciones ambientales ambientales afectan a la precisión del reloj y un dispositivo móvil puede mover de una condición de ambiente a lo que puede interferir con su capacidad para mantener el tiempo con precisión.  Por lo tanto, Microsoft no recomienda que configure dispositivos portátiles alimentado por batería con la configuración de alta precisión. 

## <a name="why-is-time-important"></a>¿Por qué es importante el tiempo?  
Hay muchas razones diferentes, que podría necesitar tiempo precisa.  El caso típico de Windows es Kerberos, lo que requiere de 5 minutos de precisión entre el cliente y servidor.  Sin embargo, hay muchas otras áreas que pueden verse afectados mediante la inclusión de precisión de tiempo:


- Regulaciones gubernamentales, como:
    - precisión de 50 ms para FINRA en Estados Unidos
    - 1 ESMA de ms (MiFID II) en la Unión Europea.
- Algoritmos de criptografía
- Sistemas distribuidos como clúster/Exchange/SQL y bases de datos de documento
- Marco de la cadena de bloques para las transacciones de bitcoin
- Registros distribuidos y análisis de amenazas 
- Replicación de AD
- PCI (Payment Card Industry), precisión segunda actualmente 1



[!INCLUDE [windows-server-2016-improvements](windows-server-2016-improvements.md)]
