---
title: Equilibrio de carga NPS Proxy Server
description: Puedes usar este tema para obtener información sobre las funcionalidades y características de Windows Server 2016 y VPN de Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9f00eb6575284a69358c58b36d08672cdd69dc18
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="nps-proxy-server-load-balancing"></a>Equilibrio de carga NPS Proxy Server

Se aplica a: Windows Server 2016

Clientes remotos del servicio de autenticación telefónica de usuario (RADIUS), que son servidores de acceso de red, como servidores de red privada virtual (VPN) y los puntos de acceso inalámbrico, crean solicitudes de conexión y envían a los servidores RADIUS, como NPS. En algunos casos, un servidor NPS recibir demasiadas solicitudes de conexión al mismo tiempo, lo que provocaría una sobrecarga o de disminución del rendimiento. Cuando un servidor NPS está sobrecargado, es una buena idea agregar más servidores NPS a la red y para configurar el equilibrio de carga. Cuando se distribuyen uniformemente solicitudes de conexión entrantes entre varios servidores NPS para evitar la sobrecarga de uno o varios servidores NPS, se denomina equilibrio de carga.

Equilibrio de carga es especialmente útil para:

- Las organizaciones que usan el protocolo de autenticación Extensible protegido o de seguridad de la capa de transporte de protocolo de autenticación Extensible \(EAP-TLS\) \ (PEAP\)-TLS para la autenticación. Dado que estos métodos de autenticación usan certificados para la autenticación de servidor y de usuario o la autenticación de equipo cliente, la carga en los servidores y servidores proxy RADIUS es mayor que cuando se usan los métodos de autenticación basada en contraseña.
- Organizaciones que necesitan para mantener la disponibilidad del servicio.
- Internet servicio proveedores \(ISPs\) que subcontrata el acceso VPN para otras organizaciones. Los servicios VPN externos pueden generar un gran volumen de tráfico de autenticación.

Hay dos métodos que puedes usar para equilibrar la carga de las solicitudes de conexión que se envían a los servidores NPS:

- Configurar los servidores de acceso de red para enviar solicitudes de conexión a varios servidores RADIUS. Por ejemplo, si tienes 20 puntos de acceso inalámbrico y dos servidores RADIUS, configurar cada punto de acceso para enviar solicitudes de conexión a los servidores RADIUS. Puede equilibrar la carga y proporcionar conmutación por error en cada servidor de acceso de red al configurar el servidor de acceso para enviar solicitudes de conexión a varios servidores RADIUS en un orden de prioridad especificado. Este método de equilibrio de carga normalmente es mejor para las organizaciones pequeñas que implementar un gran número de clientes de RADIUS.
- Usa NPS configurado como un proxy RADIUS para cargar equilibrar las solicitudes de conexión entre varios servidores NPS u otros servidores RADIUS. Por ejemplo, si tienes tres servidores RADIUS, un proxy NPS y 100 puntos de acceso inalámbrico, puede configurar los puntos de acceso para enviar todo el tráfico para el proxy NPS. En el servidor proxy NPS, configurar Equilibrio de carga para que el proxy distribuye uniformemente las solicitudes de conexión entre los servidores RADIUS tres. Este método de equilibrio de carga es mejor para organizaciones medianas y grandes que tienen muchos clientes y servidores RADIUS.

En muchos casos, el mejor enfoque para equilibrio de carga es configurar los clientes RADIUS para enviar solicitudes de conexión a dos servidores proxy NPS y, a continuación, configura los proxy NPS para cargar el saldo entre servidores RADIUS. Este enfoque logra conmutación por error y equilibrio de carga para los servidores RADIUS y proxy NPS.

## <a name="radius-server-priority-and-weight"></a>RADIUS server prioridad y peso

Durante el proceso de configuración de NPS proxy, puedes crear grupos de servidores RADIUS remotos y, a continuación, agregar servidores RADIUS a cada grupo. Para configurar el equilibrio de carga, debes tener más de un servidor de radio por grupo de servidores RADIUS remotos. Al agregar los miembros del grupo, o después de crear un servidor RADIUS como un miembro del grupo, puede obtener acceso al cuadro de diálogo Agregar RADIUS server para configurar los siguientes elementos en la pestaña de equilibrio de carga:

- **Prioridad**. Prioridad especifica el orden de importancia del servidor RADIUS con el servidor NPS. Nivel de prioridad debe asignarse un valor que sea un entero, como 1, 2 o 3. Cuanto menor sea el número, mayor prioridad proxy NPS te ofrece al servidor RADIUS. Por ejemplo, si el servidor de radio se asigna la prioridad más alta de 1, el proxy NPS envía solicitudes de conexión al servidor RADIUS primero; Si los servidores con prioridad 1 no están disponibles, NPS envía solicitudes de conexión a servidores RADIUS con prioridad 2 y así sucesivamente. Puedes asignar la misma prioridad a varios servidores RADIUS y, a continuación, usar la configuración de peso para cargar el equilibrio entre ellos.

- **Peso**. Usa NPS esta configuración de peso para determinar cuántos conexión las solicitudes para enviar a cada grupo de miembros cuando los miembros del grupo tienen el mismo nivel de prioridad. Configuración de peso debe tener asignado un valor entre 1 y 100, y el valor representa un porcentaje del 100%. Por ejemplo, si el grupo de servidores RADIUS remotos contiene a dos miembros que tienen un nivel de prioridad 1 y una clasificación de espesor de 50, el proxy NPS reenvía 50 por ciento de las solicitudes de conexión a cada servidor RADIUS.

- **Configuración avanzada**. Estas opciones de configuración de conmutación por error ofrecen una manera de NPS determinar si el servidor RADIUS remoto no está disponible. Si NPS determina que un servidor RADIUS no está disponible, puede iniciar el envío de solicitudes de conexión a otros miembros del grupo. Con esta configuración puede configurar el número de segundos que el proxy NPS espera la respuesta del servidor RADIUS antes de que tiene en cuenta la solicitud colocada; el número máximo de solicitudes interrumpidas antes de proxy NPS identifica el servidor RADIUS como no disponible; y el número de segundos que pueden pasar entre solicitudes antes de proxy NPS identifica el servidor RADIUS como no disponible.

## <a name="configure-nps-proxy-load-balancing"></a>Configurar Equilibrio de carga de proxy NPS

Antes de configurar el equilibrio de carga, crear un plan de implementación que incluye la cantidad remotos grupos de servidores RADIUS necesita, los servidores que son miembros de cada grupo en particular y la configuración de prioridad y peso para cada servidor.

>[!NOTE]
>Los pasos que siguen se suponen que ya han implementado y configurado de servidores RADIUS.

Para configurar NPS para que actúe como un servidor proxy y solicitudes de conexión directa de los clientes RADIUS a los servidores RADIUS remotos, se deben realizar las siguientes acciones:

1. Implementar los clientes RADIUS \ (802.1X, servidores de acceso telefónico, servidores de puerta de enlace de Terminal Services, conmutadores de autenticación 802.1X y servidores VPN inalámbricas acceso points\) y configurarlos para enviar solicitudes de conexión a los servidores proxy NPS.

2. En el servidor proxy NPS, configurar los servidores de acceso de red como los clientes RADIUS. Para obtener más información, consulta [configurar clientes de RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. En el servidor proxy NPS, crear uno o varios grupos de servidores RADIUS remotos. Durante este proceso, agregar servidores RADIUS a los grupos de servidores RADIUS remotos. Para obtener más información, consulta [configurar grupos de servidores RADIUS remotos](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. En el servidor proxy NPS, para cada servidor RADIUS que agregues a un grupo de servidores RADIUS remotos, haz clic en el servidor RADIUS **equilibrio de carga** pestaña y, a continuación, configura **prioridad**, **peso**, y **configuración avanzada**.

5. En el servidor proxy NPS, configurar las directivas de la solicitud de conexión para que reenvíe solicitudes de autenticación y administración de cuentas para los grupos de servidores RADIUS remotos. Debes crear una directiva de solicitud de conexión por grupo de servidores RADIUS remotos. Para obtener más información, consulta [configurar directivas de la solicitud de conexión](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


