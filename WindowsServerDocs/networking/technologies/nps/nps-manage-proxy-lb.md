---
title: Equilibrio de carga del servidor proxy NPS
description: Puede usar este tema para obtener información sobre las características y la funcionalidad de VPN de Windows Server 2016 y Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2d138b9891fb3cfa8e15060be312ff945942c660
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405425"
---
# <a name="nps-proxy-server-load-balancing"></a>Equilibrio de carga del servidor proxy NPS

Se aplica a: Windows Server 2016

Los clientes Servicio de autenticación remota telefónica de usuario (RADIUS), que son servidores de acceso a la red, como los servidores de red privada virtual (VPN) y los puntos de acceso inalámbrico, crean solicitudes de conexión y las envían a servidores RADIUS como NPS. En algunos casos, un NPS podría recibir demasiadas solicitudes de conexión al mismo tiempo, lo que produce un rendimiento degradado o una sobrecarga. Cuando un NPS está sobrecargado, es una buena idea agregar más NPSs a la red y configurar el equilibrio de carga. Cuando se distribuyen uniformemente las solicitudes de conexión entrantes entre varios NPSs para evitar la sobrecarga de una o varias NPSs, se denomina equilibrio de carga.

El equilibrio de carga es especialmente útil para:

- Las organizaciones que utilizan el protocolo de autenticación extensible: seguridad de la capa de transporte \(EAP-TLS\) o el protocolo de autenticación extensible protegido \(PEAP\)-TLS para la autenticación. Dado que estos métodos de autenticación usan certificados para la autenticación de servidor y para la autenticación de usuario o de equipo cliente, la carga en servidores proxy RADIUS y servidores es más pesada que cuando se usan métodos de autenticación basados en contraseña.
- Organizaciones que necesitan mantener la disponibilidad continua del servicio.
- Los proveedores de servicios de Internet \(ISP\) que externalizan el acceso VPN para otras organizaciones. Los servicios VPN de origen pueden generar un gran volumen de tráfico de autenticación.

Existen dos métodos que puede usar para equilibrar la carga de las solicitudes de conexión enviadas a su NPSs:

- Configure los servidores de acceso a la red para enviar solicitudes de conexión a varios servidores RADIUS. Por ejemplo, si tiene 20 puntos de acceso inalámbrico y dos servidores RADIUS, configure cada punto de acceso para enviar solicitudes de conexión a ambos servidores RADIUS. Puede equilibrar la carga y proporcionar conmutación por error en cada servidor de acceso a la red mediante la configuración del servidor de acceso para enviar solicitudes de conexión a varios servidores RADIUS en un orden de prioridad especificado. Este método de equilibrio de carga suele ser mejor para las pequeñas organizaciones que no implementan un gran número de clientes RADIUS.
- Use NPS configurado como proxy RADIUS para equilibrar la carga de las solicitudes de conexión entre varios NPSs u otros servidores RADIUS. Por ejemplo, si tiene 100 puntos de acceso inalámbrico, un proxy NPS y tres servidores RADIUS, puede configurar los puntos de acceso para enviar todo el tráfico al proxy NPS. En el proxy NPS, configure el equilibrio de carga para que el proxy distribuya uniformemente las solicitudes de conexión entre los tres servidores RADIUS. Este método de equilibrio de carga es el mejor para las organizaciones medianas y grandes que tienen muchos clientes y servidores RADIUS.

En muchos casos, el mejor enfoque para el equilibrio de carga es configurar los clientes RADIUS para que envíen solicitudes de conexión a dos servidores proxy NPS y, a continuación, configurar los proxies NPS para equilibrar la carga entre los servidores RADIUS. Este enfoque proporciona conmutación por error y equilibrio de carga para los proxies NPS y los servidores RADIUS.

## <a name="radius-server-priority-and-weight"></a>Prioridad y peso del servidor RADIUS

Durante el proceso de configuración del proxy NPS, puede crear grupos de servidores RADIUS remotos y, a continuación, agregar servidores RADIUS a cada grupo. Para configurar el equilibrio de carga, debe tener más de un servidor RADIUS por cada grupo de servidores RADIUS remotos. Al agregar miembros del grupo o después de crear un servidor RADIUS como miembro del grupo, puede tener acceso al cuadro de diálogo Agregar servidor RADIUS para configurar los siguientes elementos en la pestaña equilibrio de carga:

- **Prioridad**. Prioridad especifica el orden de importancia del servidor RADIUS en el servidor proxy NPS. El nivel de prioridad debe tener asignado un valor que sea un entero, como 1, 2 o 3. Cuanto menor sea el número, mayor prioridad proporcionará el proxy NPS al servidor RADIUS. Por ejemplo, si al servidor RADIUS se le asigna la prioridad más alta de 1, el proxy NPS envía primero las solicitudes de conexión al servidor RADIUS; Si los servidores con prioridad 1 no están disponibles, NPS envía las solicitudes de conexión a los servidores RADIUS con prioridad 2, y así sucesivamente. Puede asignar la misma prioridad a varios servidores RADIUS y, a continuación, usar la configuración de peso para equilibrar la carga entre ellos.

- **Peso**. NPS usa este valor de peso para determinar cuántas solicitudes de conexión enviar a cada miembro del grupo cuando los miembros del grupo tienen el mismo nivel de prioridad. La configuración del peso debe tener asignado un valor entre 1 y 100, y el valor representa un porcentaje del 100 por ciento. Por ejemplo, si el grupo de servidores RADIUS remotos contiene dos miembros que tienen un nivel de prioridad de 1 y una clasificación de peso de 50, el proxy NPS reenvía el 50 por ciento de las solicitudes de conexión a cada servidor RADIUS.

- **Configuración avanzada**. Esta configuración de conmutación por error proporciona una manera de que NPS determine si el servidor RADIUS remoto no está disponible. Si NPS determina que un servidor RADIUS no está disponible, puede empezar a enviar solicitudes de conexión a otros miembros del grupo. Con estas opciones, puede configurar el número de segundos que el proxy NPS espera una respuesta del servidor RADIUS antes de considerar que se ha quitado la solicitud. el número máximo de solicitudes quitadas antes de que el proxy NPS identifique el servidor RADIUS como no disponible. y el número de segundos que pueden transcurrir entre solicitudes antes de que el proxy NPS identifique el servidor RADIUS como no disponible.

## <a name="configure-nps-proxy-load-balancing"></a>Configuración del equilibrio de carga del proxy NPS

Antes de configurar el equilibrio de carga, cree un plan de implementación que incluya el número de grupos de servidores RADIUS remotos que necesita, los servidores que son miembros de cada grupo en particular y la configuración de prioridad y peso para cada servidor.

>[!NOTE]
>En los pasos siguientes se supone que ya ha implementado y configurado los servidores RADIUS.

Para configurar NPS para que actúe como servidor proxy y Reenvíe solicitudes de conexión de clientes RADIUS a servidores RADIUS remotos, debe realizar las siguientes acciones:

1. Implemente los clientes RADIUS \(servidores VPN, los servidores de acceso telefónico, los servidores de puerta de enlace de Terminal Services, los conmutadores de autenticación de 802.1 X y los puntos de acceso inalámbricos 802.1 X\) y configurarlos para enviar solicitudes de conexión a los servidores proxy NPS.

2. En el proxy NPS, configure los servidores de acceso a la red como clientes RADIUS. Para obtener más información, consulte [Configure RADIUS clients](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. En el proxy NPS, cree uno o varios grupos de servidores RADIUS remotos. Durante este proceso, agregue servidores RADIUS a los grupos de servidores RADIUS remotos. Para obtener más información, vea [configurar grupos de servidores RADIUS remotos](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. En el proxy NPS, para cada servidor RADIUS que agregue a un grupo de servidores RADIUS remotos, haga clic en la pestaña **equilibrio de carga** del servidor RADIUS y, a continuación, configure la **prioridad**, el **peso**y la **Configuración avanzada**.

5. En el proxy NPS, configure las directivas de solicitud de conexión para reenviar las solicitudes de autenticación y cuentas a los grupos de servidores RADIUS remotos. Debe crear una directiva de solicitud de conexión por cada grupo de servidores RADIUS remotos. Para obtener más información, consulte [configurar directivas de solicitud de conexión](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


