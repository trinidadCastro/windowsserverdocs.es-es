---
title: Equilibrio de carga del servidor NPS Proxy
description: Puede utilizar este tema para obtener información sobre la funcionalidad y las características de Windows Server 2016 y VPN de Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 04f466f7646fc5109af7379cab1240d8cd6829ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874406"
---
# <a name="nps-proxy-server-load-balancing"></a>Equilibrio de carga del servidor NPS Proxy

Se aplica a: Windows Server 2016

Clientes remotos del servicio de autenticación telefónica de usuario (RADIUS), que son servidores de acceso de red, como servidores de red privada virtual (VPN) y puntos de acceso inalámbrico, crean solicitudes de conexión y los envían a servidores RADIUS como NPS. En algunos casos, un NPS podría recibir demasiadas solicitudes de conexión al mismo tiempo, lo que resulta en un rendimiento degradado o una sobrecarga. Cuando se sobrecarga un NPS, es una buena idea agregar NPSs más a la red y configurar el equilibrio de carga. Cuando se distribución uniformemente solicitudes de conexión entrantes entre varios NPSs para evitar la sobrecarga de NPSs uno o más, se denomina equilibrio de carga.

Es especialmente útil para equilibrio de carga:

- Las organizaciones que usan Extensible Authentication Protocol Transport Layer Security \(EAP-TLS\) o protocolo de autenticación Extensible protegido \(PEAP\)- TLS para la autenticación. Dado que estos métodos de autenticación usan certificados para la autenticación de servidor y de usuario o autenticación de equipos cliente, la carga en los servidores proxy RADIUS y servidores es más pesada que cuando se usan métodos de autenticación basado en contraseña.
- Organizaciones que necesitan para mantener la disponibilidad continua del servicio.
- Proveedores de servicios Internet \(ISP\) que externalizar acceso VPN para otras organizaciones. Los servicios de VPN externos pueden generar un gran volumen de tráfico de autenticación.

Hay dos métodos que puede usar para equilibrar la carga de solicitudes de conexión enviadas a sus NPSs:

- Configure los servidores de acceso de red para enviar las solicitudes de conexión a varios servidores RADIUS. Por ejemplo, si tiene 20 puntos de acceso inalámbrico y dos servidores RADIUS, debe configurar cada punto de acceso para enviar las solicitudes de conexión a ambos servidores RADIUS. Puede equilibrar la carga y proporcionar conmutación por error en cada servidor de acceso de red mediante la configuración del servidor de acceso para enviar las solicitudes de conexión a varios servidores RADIUS en un orden de prioridad especificado. Este método de equilibrio de carga normalmente es mejor para las organizaciones pequeñas que no implementan un gran número de clientes RADIUS.
- Use NPS configurado como proxy RADIUS para cargar solicitudes de conexión del equilibrio entre varios NPSs u otros servidores RADIUS. Por ejemplo, si tiene 100 puntos de acceso inalámbrico, un proxy NPS y tres servidores RADIUS, puede configurar los puntos de acceso para enviar todo el tráfico al proxy NPS. En el proxy NPS, configure el equilibrio de carga para que el proxy distribuye uniformemente las solicitudes de conexión entre los tres servidores RADIUS. Este método de equilibrio de carga es más adecuada para organizaciones medianas y grandes que tienen muchos clientes y servidores RADIUS.

En muchos casos, el mejor enfoque para el equilibrio de carga es configurar clientes RADIUS para enviar las solicitudes de conexión a dos servidores de proxy NPS y, a continuación, configure los proxies NPS para equilibrar entre los servidores RADIUS. Este enfoque proporciona conmutación por error y equilibrio de carga para los servidores proxy NPS y servidores RADIUS.

## <a name="radius-server-priority-and-weight"></a>Prioridad del servidor RADIUS y la importancia

Durante el proceso de configuración del proxy NPS, puede crear grupos de servidores RADIUS remotos y, a continuación, agregar servidores RADIUS a cada grupo. Para configurar el equilibrio de carga, debe tener más de un servidor RADIUS por grupo de servidores RADIUS remotos. Al agregar los miembros del grupo, o después de crear un servidor RADIUS como miembro del grupo, puede acceder el cuadro de diálogo Agregar RADIUS server para configurar los siguientes elementos en la pestaña de equilibrio de carga:

- **prioridad**. Prioridad especifica el orden de importancia del servidor RADIUS para el servidor proxy NPS. Nivel de prioridad debe asignarse un valor que es un entero, como 1, 2 o 3. Cuanto menor sea el número, la prioridad más alta proporciona el proxy NPS en el servidor RADIUS. Por ejemplo, si el servidor RADIUS se asigna la prioridad máxima de 1, el proxy NPS envía las solicitudes de conexión al servidor RADIUS en primer lugar; Si no están disponibles los servidores con prioridad 1, NPS envía las solicitudes de conexión a servidores RADIUS con prioridad 2 y así sucesivamente. Puede asignar la misma prioridad para varios servidores RADIUS y, a continuación, usar la configuración de peso para equilibrar la carga entre ellos.

- **Peso**. Usa NPS solicita esta configuración de peso para determinar cuántos de conexión para enviar a cada grupo de miembro cuando los miembros del grupo tienen el mismo nivel de prioridad. Configuración de peso debe asignarse un valor entre 1 y 100 y el valor representa un porcentaje del 100 por ciento. Por ejemplo, si el grupo de servidores RADIUS remotos contiene a dos miembros que tienen un nivel de prioridad 1 y una clasificación de peso de 50, el proxy NPS reenvía el 50 por ciento de las solicitudes de conexión para cada servidor RADIUS.

- **Configuración avanzada**. Esta configuración de conmutación por error proporciona una manera para que NPS pueda determinar si el servidor RADIUS remoto no está disponible. Si NPS determina que un servidor RADIUS no está disponible, puede empezar a enviar las solicitudes de conexión a otros miembros del grupo. Con esta configuración puede configurar el número de segundos que el proxy NPS espera una respuesta del servidor RADIUS antes de considerar que la solicitud quitada; el número máximo de solicitudes interrumpidas antes el proxy NPS identifica el servidor RADIUS como no disponible; y el número de segundos que puede transcurrir entre solicitudes antes el proxy NPS identifica el servidor RADIUS como no disponible.

## <a name="configure-nps-proxy-load-balancing"></a>Configurar Equilibrio de carga del proxy NPS

Antes de configurar el equilibrio de carga, cree un plan de implementación que incluya cuántos remotos grupos de servidores RADIUS que requiere, los servidores que son miembros de cada grupo en particular y la configuración de prioridad e importancia de cada servidor.

>[!NOTE]
>Los pasos siguientes se suponen que ya ha implementado y configurado servidores RADIUS.

Para configurar NPS para que actúe como un servidor proxy y reenviar solicitudes de conexión de clientes RADIUS a servidores remotos RADIUS, debe realizar las siguientes acciones:

1. Implementar los clientes RADIUS \(servidores VPN, servidores de acceso telefónico, servidores de puerta de enlace de Terminal Services, conmutadores de autenticación 802.1X y 802.1X inalámbrico puntos de acceso\) y configurarlas para enviar las solicitudes de conexión para el proxy NPS servidores.

2. En el proxy NPS, configure los servidores de acceso de red como clientes RADIUS. Para obtener más información, consulte [configurar clientes RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. En el proxy NPS, cree uno o varios grupos de servidores RADIUS remotos. Durante este proceso, agregar servidores RADIUS a los grupos de servidores RADIUS remotos. Para obtener más información, consulte [configurar grupos de servidores RADIUS remotos](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. En el proxy NPS, para cada servidor RADIUS que se agrega a un grupo de servidores RADIUS remotos, haga clic en el servidor RADIUS **equilibrio de carga** ficha y, a continuación, configure **prioridad**, **peso** , y **configuración avanzada**.

5. En el proxy NPS, configure las directivas de solicitud de conexión para reenviar solicitudes de autenticación y cuentas a grupos de servidores RADIUS remotos. Debe crear una directiva de solicitud de conexión por grupo de servidores RADIUS remotos. Para obtener más información, consulte [configurar directivas de solicitud de conexión](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


