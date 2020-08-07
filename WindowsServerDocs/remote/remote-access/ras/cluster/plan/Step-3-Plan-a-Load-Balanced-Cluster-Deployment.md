---
title: 'Paso 3: planear la implementación de un clúster con equilibrio de carga'
description: Este tema forma parte de la guía deploy Remote Access in a Cluster in Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 7540c17b-81de-47de-a04f-3247afa26f70
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6c590fd99715e49034592358f65c468c6a46dfb9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963891"
---
# <a name="step-3-plan-a-load-balanced-cluster-deployment"></a>Paso 3: planear la implementación de un clúster con equilibrio de carga

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El siguiente paso consiste en planear la configuración de equilibrio de carga y la implementación del clúster.

|Tarea|Descripción|
|----|--------|
|3,1 planear el equilibrio de carga|Decida si desea usar el equilibrio de carga de red (NLB) de Windows o un equilibrador de carga externo (ELB).|
|3,2 plan IP-HTTPS|Si no se usa un certificado autofirmado, el servidor de acceso remoto necesita un certificado SSL en cada servidor del clúster, con el fin de autenticar las conexiones IP-HTTPS.|
|3,3 plan para conexiones de cliente VPN|Tenga en cuenta los requisitos para las conexiones de cliente VPN.|
|3,4 planear el servidor de ubicación de red|Si el sitio web del servidor de ubicación de red se hospeda en el servidor de acceso remoto y no se usa un certificado autofirmado, asegúrese de que cada servidor del clúster tenga un certificado de servidor para autenticar la conexión al sitio Web.|

## <a name="31-plan-load-balancing"></a><a name="bkmk_2_1_Plan_LB"></a>3,1 planear el equilibrio de carga
El acceso remoto se puede implementar en un solo servidor o en un clúster de servidores de acceso remoto. Se puede equilibrar la carga del tráfico al clúster para proporcionar alta disponibilidad y escalabilidad a los clientes de DirectAccess. Hay dos opciones de equilibrio de carga:

-   **Windows NLB**: Windows NLB es una característica de Windows Server. Para usarlo, no necesita hardware adicional, ya que todos los servidores del clúster son responsables de administrar la carga de tráfico. Windows NLB admite un máximo de ocho servidores en un clúster de acceso remoto.

-   **Equilibrador de carga externo**: el uso de un equilibrador de carga externo requiere hardware externo para administrar la carga de tráfico entre los servidores del clúster de acceso remoto. Además, el uso de un equilibrador de carga externo admite un máximo de 32 servidores de acceso remoto en un clúster. Algunos aspectos que hay que tener en cuenta al configurar el equilibrio de carga externo son:

    -   El administrador debe asegurarse de que las direcciones IP virtuales configuradas mediante el Asistente para el equilibrio de carga de acceso remoto se usan en los equilibradores de carga externos (como F5 BIG-IP local Traffic Manager sistema). Cuando se habilita el equilibrio de carga externo, las direcciones IP de las interfaces externas e internas se promocionarán a direcciones IP virtuales y deben estar sondeadas en los equilibradores de carga. Esto se hace para que el administrador no tenga que cambiar la entrada DNS para el nombre público de la implementación del clúster. Además, los extremos del túnel IPsec se derivan de las direcciones IP del servidor. Si el administrador proporciona direcciones IP virtuales independientes, el cliente no podrá conectarse al servidor. Vea el ejemplo de configuración de DirectAccess con equilibrio de carga externo en 3.1.1 external Load Balancer Configuration example.

    -   Muchos equilibradores de carga externos (incluido F5) no admiten el equilibrio de carga de 6to4 e ISATAP. Si el servidor de acceso remoto es un enrutador ISATAP, la función ISATAP debe moverse a un equipo diferente. Además, cuando la función ISATAP está en un equipo diferente, los servidores de DirectAccess deben tener conectividad IPv6 nativa con el enrutador ISATAP. Tenga en cuenta que esta conectividad debe estar presente antes de configurar DirectAccess.

    -   En el caso del equilibrio de carga externo, si se debe usar Teredo, todos los servidores de acceso remoto deben tener dos direcciones IPv4 públicas consecutivas como direcciones IP dedicadas. Las direcciones IP virtuales del clúster también deben tener dos direcciones IPv4 públicas consecutivas. Esto no es cierto para Windows NLB, donde solo las direcciones IP virtuales del clúster deben tener dos direcciones IPv4 públicas consecutivas. Si no se utiliza Teredo, no se requieren dos direcciones IP consecutivas.

    -   El administrador puede cambiar de NLB de Windows a equilibrador de carga externo y viceversa. Tenga en cuenta que el administrador no puede cambiar de load balancer externo a NLB de Windows si tiene más de 8 servidores en la implementación del equilibrador de carga externo.

### <a name="311-external-load-balancer-configuration-example"></a><a name="ELBConfigEx"></a>3.1.1 ejemplo de configuración de Load Balancer externa
En esta sección se describen los pasos de configuración para habilitar un equilibrador de carga externo en una nueva implementación de acceso remoto. Cuando se usa un equilibrador de carga externo, el clúster de acceso remoto puede ser similar a la siguiente ilustración, donde los servidores de acceso remoto están conectados a la red corporativa a través de un equilibrador de carga en la red interna y a Internet a través de un equilibrador de carga conectado a la red externa:

![Ejemplo de configuración de Load Balancer externo](../../../../media/Step-3-Plan-a-Load-Balanced-Cluster-Deployment/ELBDiagram-URA_Enterprise_NLB-.png)

##### <a name="planning-information"></a>Información de planeación

1.  Los VIP externos (direcciones IP que el cliente usará para conectarse al acceso remoto) se decidieron 131.107.0.102, 131.107.0.103

2.  Equilibrador de carga en la red externa Self-IPs-131.107.0.245 (Internet), 131.107.1.245

    La red perimetral (también conocida como zona desmilitarizada y DMZ) se encuentra entre el equilibrador de carga de la red externa y el servidor de acceso remoto.

3.  Direcciones IP para el servidor de acceso remoto en la red perimetral-131.107.1.102, 131.107.1.103

4.  Direcciones IP para el servidor de acceso remoto en la red ELB (es decir, entre el servidor de acceso remoto y el equilibrador de carga de la red interna)-30.11.1.101, 2006:2005:11:1::101

5.  Equilibrador de carga en la red interna Self-IP-30.11.1.245 2006:2005:11:1::245 (ELB), 30.1.1.245 2006:2005:1:1::245 (CorpNet)

6.  Las VIP internas (direcciones IP usadas para el sondeo Web de cliente y para el servidor de ubicación de red, si están instaladas en los servidores de acceso remoto) se decidieron 30.1.1.10, 2006:2005:1:1::10

##### <a name="steps"></a>Pasos

1.  Configure el adaptador de red externo del servidor de acceso remoto (que está conectado a la red perimetral) con las direcciones 131.107.0.102, 131.107.0.103. Este paso es necesario para que la configuración de DirectAccess detecte los puntos de conexión de túnel IPsec correctos.

2.  Configure el adaptador de red interno del servidor de acceso remoto (que está conectado a la red ELB) con las direcciones IP del servidor de sondeo/red (30.1.1.10, 2006:2005:1:1::10). Este paso es necesario para permitir que los clientes tengan acceso a la dirección IP de sondeo Web, por lo que el Asistente para la conectividad de red indica correctamente el estado de la conexión a DirectAccess. Este paso también permite el acceso al servidor de ubicación de red, si está configurado en el servidor de DirectAccess.

    > [!NOTE]
    > Asegúrese de que el controlador de dominio es accesible desde el servidor de acceso remoto con esta configuración.

3.  Configure un único servidor de DirectAccess en el servidor de acceso remoto.

4.  Habilite el equilibrio de carga externo en la configuración de DirectAccess. Usar 131.107.1.102 como dirección IP dedicada (DIP) externa (131.107.1.103 se seleccionará automáticamente), use 30.11.1.101, 2006:2005:11:1::101 como DIP internas.

5.  Configure las direcciones IP virtuales externas (VIP) en el equilibrador de carga externo con las direcciones 131.107.0.102 y 131.107.0.103. Además, configure las VIP internas en el equilibrador de carga externo con las direcciones 30.1.1.10 y 2006:2005:1:1::10.

6.  El servidor de acceso remoto se configurará con las direcciones IP planeadas y las direcciones IP externas e internas del clúster se configurarán según las direcciones IP planeadas.

## <a name="32-plan-ip-https"></a><a name="bkmk_2_2_NLB"></a>3,2 plan IP-HTTPS

1.  **Requisitos de certificado**: durante la implementación del único servidor de acceso remoto, seleccionó usar un certificado IP-https emitido por una entidad de certificación (CA) pública o interna, o un certificado autofirmado. En la implementación de clústeres, debe usar un tipo de certificado idéntico en cada miembro del clúster de acceso remoto. Es decir, si usó un certificado emitido por una CA pública (recomendado), debe instalar un certificado emitido por una CA pública en cada miembro del clúster. El nombre de sujeto del certificado nuevo debe ser idéntico al nombre de sujeto del certificado IP-HTTPS que se usa actualmente en la implementación. Tenga en cuenta que si usa certificados autofirmados, estos se configurarán automáticamente en cada servidor durante la implementación del clúster.

2.  **Requisitos de prefijo**: el acceso remoto permite el equilibrio de carga entre el tráfico basado en SSL y el tráfico de DirectAccess. Para equilibrar la carga de todo el tráfico de DirectAccess basado en IPv6, el acceso remoto debe examinar el túnel IPv4 para todas las tecnologías de transición. Dado que el tráfico IP-HTTPS está cifrado, no es posible examinar el contenido del túnel IPv4. Para permitir que el tráfico IP-HTTPS tenga equilibrio de carga, debe asignar un prefijo IPv6 de ancho suficiente para que se pueda asignar un prefijo IPv6/64 diferente a todos los miembros del clúster. Puede configurar un máximo de 32 servidores en un clúster con equilibrio de carga; por lo tanto, debe especificar un prefijo/59. Este prefijo debe ser enrutable a la dirección IPv6 interna del clúster de acceso remoto y se configura en el Asistente para la instalación del servidor de acceso remoto.

    > [!NOTE]
    > Los requisitos de prefijo solo son relevantes en una red interna habilitada para IPv6 (solo IPv6 o IPV4 + IPv6). En una red corporativa solo IPv4, el prefijo de cliente se configura automáticamente y el administrador no puede cambiarlo.

## <a name="33-plan-for-vpn-client-connections"></a><a name="BKMK_3.3"></a>3,3 plan para conexiones de cliente VPN
Hay una serie de consideraciones para las conexiones de cliente VPN:

-   No se puede equilibrar la carga del tráfico de clientes VPN si se asignan direcciones de cliente VPN mediante DHCP. Se requiere un grupo de direcciones estáticas.

-   RRAS se puede habilitar en un clúster con equilibrio de carga que se ha implementado solo para DirectAccess, mediante **habilitar VPN** en el panel de tareas de la consola de administración de acceso remoto.

-   Cualquier cambio de VPN completado en la consola de administración de enrutamiento y acceso remoto (rrasmgmt. msc) tendrá que replicarse manualmente en todos los servidores de acceso remoto del clúster.

-   Para permitir que el tráfico de cliente IPv6 de VPN tenga equilibrio de carga, debe especificar un prefijo IPv6 de 59 bits.

## <a name="34-plan-the-network-location-server"></a><a name="BKMK_nls"></a>3,4 planear el servidor de ubicación de red
Si está ejecutando el sitio web del servidor de ubicación de red en el único servidor de acceso remoto, durante la implementación seleccionó para usar un certificado emitido por una entidad de certificación (CA) interna o un certificado autofirmado.  Tenga en cuenta lo siguiente:

1.  Cada miembro del clúster de acceso remoto debe tener un certificado para el servidor de ubicación de red correspondiente a la entrada DNS para el sitio web del servidor de ubicación de red.

2.  El certificado para cada servidor de clúster se debe emitir de la misma manera que el certificado del servidor de ubicación de red actual del servidor de acceso remoto. Por ejemplo, si usó un certificado emitido por una CA interna, debe instalar un certificado emitido por la entidad de certificación interna en cada miembro del clúster.

3.  Si ha usado un certificado autofirmado, se configurará automáticamente un certificado autofirmado para cada servidor durante la implementación del clúster.

4.  El nombre de sujeto del certificado no debe ser idéntico al nombre de ninguno de los servidores de la implementación de acceso remoto.
