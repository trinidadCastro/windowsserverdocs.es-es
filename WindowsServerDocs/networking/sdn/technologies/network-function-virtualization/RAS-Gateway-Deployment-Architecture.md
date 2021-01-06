---
title: Arquitectura de implementación de puerta de enlace de RAS
description: Puede usar este tema para obtener información acerca de la implementación del proveedor de servicios en la nube (CSP) de la puerta de enlace de RAS en Windows Server 2016, incluidos los grupos de puerta de enlace RAS, los Reflejadores de ruta e implementación de varias puertas de enlace para inquilinos individuales.
manager: grcusanz
ms.topic: how-to
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/07/2020
ms.openlocfilehash: abc36a7e26ab25f7f121538dcadf9761b111a6e2
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949371"
---
# <a name="ras-gateway-deployment-architecture"></a>Arquitectura de implementación de puerta de enlace de RAS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información acerca de la implementación del proveedor de servicios en la nube (CSP) de la puerta de enlace RAS, incluidos los grupos de puerta de enlace RAS, los reflectores de rutas y la implementación de varias puertas de enlace para inquilinos individuales.

En las secciones siguientes se proporciona información general sobre algunas de las nuevas características de puerta de enlace RAS para que pueda entender cómo usar estas características en el diseño de la implementación de la puerta de enlace.

Además, se proporciona una implementación de ejemplo, que incluye información sobre el proceso de agregar nuevos inquilinos, la sincronización de rutas y el enrutamiento del plano de datos, la conmutación por error de reflector de enrutamiento y puerta de enlace, etc.

En este tema se incluyen las siguientes secciones.

-   [Uso de las nuevas características de puerta de enlace RAS para diseñar la implementación](#bkmk_new)

-   [Ejemplo de implementación](#bkmk_example)

-   [Agregar nuevos inquilinos y el espacio de direcciones de cliente (CA) EBGP emparejamiento](#bkmk_tenant)

-   [Sincronización de rutas y enrutamiento de plano de datos](#bkmk_route)

-   [Cómo responde el controlador de red a la puerta de enlace RAS y la conmutación por error de reflector](#bkmk_failover)

-   [Ventajas del uso de las nuevas características de puerta de enlace RAS](#bkmk_advantages)

## <a name="using-ras-gateway-new-features-to-design-your--deployment"></a><a name="bkmk_new"></a>Uso de las nuevas características de puerta de enlace RAS para diseñar la implementación
La puerta de enlace RAS incluye varias características nuevas que cambian y mejoran la manera en que se implementa la infraestructura de puerta de enlace en el centro de recursos.

### <a name="bgp-route-reflector"></a>Reflector de rutas BGP
La funcionalidad de reflector de rutas de Protocolo de puerta de enlace de borde (BGP) ahora se incluye con la puerta de enlace de RAS y proporciona una alternativa a la topología de malla completa de BGP que normalmente se requiere para la sincronización de rutas entre los enrutadores. Con la sincronización de malla completa, todos los enrutadores de BGP deben conectarse con todos los demás enrutadores de la topología de enrutamiento. Sin embargo, cuando se usa el reflector de rutas, este es el único enrutador que se conecta con todos los demás enrutadores, denominados clientes del reflector de rutas de BGP, lo que simplifica la sincronización de rutas y reduce el tráfico de red. El reflector de rutas aprende todas las rutas, calcula las mejores rutas y redistribuye las mejores rutas a sus clientes de BGP.

Para obtener más información, consulte [novedades de puerta de enlace de Ras](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).

### <a name="gateway-pools"></a><a name="bkmk_pools"></a>Grupos de puerta de enlace
En Windows Server 2016, puede crear muchos grupos de puertas de enlace de distintos tipos. Los grupos de puertas de enlace contienen muchas instancias de puerta de enlace RAS y enrutan el tráfico de red entre las redes físicas y virtuales.

Para obtener más información, consulte [novedades de puerta de enlace ras](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [alta disponibilidad de puerta de enlace ras](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).

### <a name="gateway-pool-scalability"></a><a name="bkmk_gps"></a>Escalabilidad del grupo de puerta de enlace
Puede escalar o reducir verticalmente un grupo de puertas de enlace fácilmente agregando o quitando máquinas virtuales de puerta de enlace del grupo. La eliminación o adición de puertas de enlace no interrumpe los servicios que proporciona un grupo. También puede agregar y quitar grupos de puertas de enlace completos.

Para obtener más información, consulte [novedades de puerta de enlace ras](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [alta disponibilidad de puerta de enlace ras](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).

### <a name="mn-gateway-pool-redundancy"></a><a name="bkmk_m"></a>Redundancia de grupo de puerta de enlace M + N
Cada grupo de puerta de enlace es M + N redundante. Esto significa que se realiza una copia de seguridad de un número de máquinas virtuales de puerta de enlace activo con un número "N" de máquinas virtuales de puerta de enlace en espera. La redundancia M + N proporciona más flexibilidad a la hora de determinar el nivel de confiabilidad que necesita al implementar la puerta de enlace de RAS.

Para obtener más información, consulte [novedades de puerta de enlace ras](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [alta disponibilidad de puerta de enlace ras](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).

## <a name="example-deployment"></a><a name="bkmk_example"></a>Ejemplo de implementación
En la ilustración siguiente se proporciona un ejemplo con el emparejamiento de eBGP en las conexiones VPN de sitio a sitio configuradas entre dos inquilinos, contoso y Woodgrove, y el centro de información de Fabrikam CSP.

![emparejamiento de eBGP a través de VPN de sitio a sitio](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)

En este ejemplo, contoso requiere más ancho de banda de puerta de enlace, lo que conduce a la decisión de diseño de la infraestructura de puerta de enlace para finalizar el sitio de Contoso los Ángeles en GW3 en lugar de GW2. Por este motivo, las conexiones VPN de Contoso de sitios diferentes terminan en el centro de recursos de CSP en dos puertas de enlace diferentes.

Ambas puertas de enlace, GW2 y GW3, eran las primeras puertas de enlace de RAS configuradas por la controladora de red cuando el CSP agregaba los inquilinos contoso y Woodgrove a su infraestructura. Por este motivo, estas dos puertas de enlace se configuran como reflectores de ruta para estos clientes (o inquilinos) correspondientes. GW2 es el reflector de rutas de Contoso y GW3 es el reflector de rutas de Woodgrove, además de ser el punto de terminación de la puerta de enlace RAS de CSP para la conexión VPN con el sitio de Contoso los Ángeles HQ.

> [!NOTE]
> Una puerta de enlace RAS puede enrutar el tráfico de red virtual y físico para hasta 100 inquilinos diferentes, en función de los requisitos de ancho de banda de cada inquilino.

Como reflectores de ruta, GW2 envía rutas de espacio de CA de Contoso a la controladora de red y GW3 envía rutas de espacio de CA de Woodgrove a la controladora de red.

La controladora de red envía directivas de virtualización de red de Hyper-V a las redes virtuales contoso y Woodgrove, así como directivas de RAS a las puertas de enlace RAS y directivas de equilibrio de carga a los multiplexores (MUX) que se configuran como un grupo de equilibrio de carga de software.

## <a name="adding-new-tenants-and-customer-address-ca-space-ebgp-peering"></a><a name="bkmk_tenant"></a>Agregar nuevos inquilinos y el espacio de direcciones de cliente (CA) eBGP emparejamiento
Al firmar un cliente nuevo y agregar el cliente como un nuevo inquilino en su centro de recursos, puede usar el proceso siguiente, gran parte de lo que realizan automáticamente los enrutadores eBGP de la puerta de enlace de RAS y la controladora de red.

1.  Aprovisione una nueva red virtual y cargas de trabajo de acuerdo con los requisitos de su inquilino.

2.  Si es necesario, configure la conectividad remota entre el sitio de la empresa de inquilinos remotos y su red virtual en el centro de recursos. Cuando se implementa una conexión VPN de sitio a sitio para el inquilino, la controladora de red selecciona automáticamente una máquina virtual de puerta de enlace RAS disponible desde el grupo de puerta de enlace disponible y configura la conexión.

3.  Al configurar la máquina virtual de puerta de enlace RAS para el nuevo inquilino, la controladora de red también configura la puerta de enlace RAS como un enrutador BGP y la designa como el reflector de rutas para el inquilino. Esto es cierto incluso en las circunstancias en las que la puerta de enlace RAS actúa como puerta de enlace, o como una puerta de enlace y un reflector de rutas, para otros inquilinos.

4.  Dependiendo de si el enrutamiento de espacio de la entidad de certificación está configurado para usar redes configuradas estáticamente o el enrutamiento BGP dinámico, el controlador de red configura las rutas estáticas correspondientes, los vecinos BGP o ambos en la máquina virtual de puerta de enlace RAS y en el reflector de rutas.

    > [!NOTE]
    > -   Una vez que la controladora de red ha configurado una puerta de enlace de RAS y un reflector de enrutamiento para el inquilino, siempre que el mismo inquilino requiera una nueva conexión VPN de sitio a sitio, la controladora de red comprueba la capacidad disponible en esta máquina virtual de puerta de enlace de RAS. Si la puerta de enlace original puede atender la capacidad necesaria, la nueva conexión de red también se configura en la misma máquina virtual de puerta de enlace de RAS. Si la máquina virtual de puerta de enlace de RAS no puede controlar la capacidad adicional, el controlador de red selecciona una nueva máquina virtual de puerta de enlace RAS y configura la nueva conexión en ella. Esta nueva máquina virtual de puerta de enlace RAS asociada al inquilino se convierte en el cliente de reflector de rutas del reflector de enrutamiento de puerta de enlace RAS de inquilino original.
    > -   Dado que los grupos de puerta de enlace RAS están detrás de equilibradores de carga de software (SLBs), las direcciones VPN de sitio a sitio de los inquilinos usan una única dirección IP pública, denominada dirección IP virtual (VIP), que se traduce mediante el SLBs en una dirección IP interna del centro de bits, denominada dirección IP dinámica Esta asignación de dirección IP pública a privada por SLB garantiza que los túneles VPN de sitio a sitio se establezcan correctamente entre los sitios de la empresa y las puertas de enlace RAS de CSP y los Reflejadores de ruta.
    >
    >     Para más información sobre SLB, VIP y DIP, consulte [equilibrio de carga de Software &#40;slb&#41; para Sdn](./software-load-balancing-for-sdn.md).

5.  Después de establecer el túnel VPN de sitio a sitio entre el sitio de la empresa y la puerta de enlace RAS del centro de recursos de CSP para el nuevo inquilino, las rutas estáticas que están asociadas a los túneles se aprovisionan automáticamente en los lados de la empresa y del CSP del túnel.

6.  Con el enrutamiento BGP de espacio de CA, también se establece el emparejamiento de eBGP entre los sitios de empresa y el reflector de rutas de puerta de enlace RAS de CSP.

## <a name="route-synchronization-and-data-plane-routing"></a><a name="bkmk_route"></a>Sincronización de rutas y enrutamiento de plano de datos
Después de establecer el emparejamiento de eBGP entre los sitios de empresa y el reflector de enrutamiento de puerta de enlace RAS de CSP, el reflector de rutas aprende todas las rutas empresariales mediante el enrutamiento BGP dinámico. El reflector de rutas sincroniza estas rutas entre todos los clientes de reflector de rutas para que todos estén configurados con el mismo conjunto de rutas.

El reflector de rutas también actualiza estas rutas consolidadas, mediante la sincronización de rutas, a la controladora de red. A continuación, la controladora de red traduce las rutas a las directivas de virtualización de red de Hyper-V y configura la red de tejido para asegurarse de que se aprovisiona el enrutamiento de rutas de acceso de datos de un extremo a otro. Este proceso hace que la red virtual de inquilino sea accesible desde los sitios empresariales de inquilinos.

Para el enrutamiento del plano de datos, los paquetes que llegan a las máquinas virtuales de puerta de enlace RAS se enrutan directamente a la red virtual del inquilino, ya que las rutas necesarias ahora están disponibles con todas las máquinas virtuales de puerta de enlace RAS participantes.

Del mismo modo, con las directivas de virtualización de red de Hyper-V aplicadas, la red virtual de inquilinos enruta los paquetes directamente a las máquinas virtuales de puerta de enlace de RAS (sin necesidad de conocer el reflector de rutas) y, a continuación, a los sitios empresariales a través de los túneles VPN de sitio a sitio.

Permita los intervalos de direcciones IP para la región de Azure de su suscripción y para el oeste de EE. devolver el tráfico de la red virtual de inquilino al sitio de empresa de inquilino remoto omite SLBs, un proceso llamado Direct Server Return (DSR).

## <a name="how-network-controller-responds-to-ras-gateway-and-route-reflector-failover"></a><a name="bkmk_failover"></a>Cómo responde el controlador de red a la puerta de enlace RAS y la conmutación por error de reflector
A continuación se muestran dos escenarios posibles de conmutación por error: uno para los clientes de reflector de enrutamiento de puerta de enlace RAS y otro para los reflectores de ruta de puerta de enlace RAS, incluida información sobre cómo controla la conmutación por error las máquinas virtuales en cualquier configuración.

### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>Error de máquina virtual de un cliente reflector de enrutamiento BGP de puerta de enlace RAS
Controladora de red realiza las siguientes acciones cuando se produce un error en un cliente de reflector de enrutamiento de puerta de enlace RAS.

> [!NOTE]
> Cuando una puerta de enlace de RAS no es un reflector de rutas para la infraestructura BGP de un inquilino, es un cliente de reflector de rutas en la infraestructura BGP del inquilino.

-   Controladora de red selecciona una máquina virtual de puerta de enlace RAS en espera disponible y aprovisiona la nueva máquina virtual de puerta de enlace RAS con la configuración de la máquina virtual de puerta de enlace RAS errónea.

-   La controladora de red actualiza la configuración de SLB correspondiente para asegurarse de que los túneles de VPN de sitio a sitio de los sitios de inquilino a la puerta de enlace de RAS errónea se han establecido correctamente con la nueva puerta de enlace de RAS.

-   La controladora de red configura el cliente de reflector de enrutamiento BGP en la nueva puerta de enlace.

-   La controladora de red configura el nuevo cliente de reflector de ruta BGP de puerta de enlace RAS como activo. La puerta de enlace RAS inicia inmediatamente el emparejamiento con el reflector de rutas del inquilino para compartir información de enrutamiento y para habilitar el emparejamiento de eBGP para el sitio de la empresa correspondiente.

### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>Error de máquina virtual para un reflector de rutas BGP de puerta de enlace RAS
La controladora de red realiza las siguientes acciones cuando se produce un error en un reflector de rutas BGP de puerta de enlace RAS.

-   Controladora de red selecciona una máquina virtual de puerta de enlace RAS en espera disponible y aprovisiona la nueva máquina virtual de puerta de enlace RAS con la configuración de la máquina virtual de puerta de enlace RAS errónea.

-   La controladora de red configura el reflector de rutas en la nueva máquina virtual de puerta de enlace RAS y asigna a la nueva máquina virtual la misma dirección IP usada por la máquina virtual con errores, lo que proporciona la integridad de la ruta a pesar del error de la máquina virtual.

-   La controladora de red actualiza la configuración de SLB correspondiente para asegurarse de que los túneles de VPN de sitio a sitio de los sitios de inquilino a la puerta de enlace de RAS errónea se han establecido correctamente con la nueva puerta de enlace de RAS.

-   La controladora de red configura la nueva máquina virtual de reflector de enrutamiento BGP de puerta de enlace RAS como activa.

-   El reflector de rutas se activa inmediatamente. Se establece el túnel VPN de sitio a sitio a la empresa, y el reflector de rutas utiliza las rutas de intercambio y el emparejamiento de eBGP con los enrutadores de sitios de empresa.

-   Después de la selección de la ruta BGP, el reflector de enrutamiento BGP de puerta de enlace RAS actualiza los clientes de reflector de rutas de inquilino en el centro de datos y sincroniza las rutas con el controlador de red, lo que permite que la ruta de acceso a los datos de un extremo a otro esté disponible para el tráfico

## <a name="advantages-of-using-new-ras-gateway-features"></a><a name="bkmk_advantages"></a>Ventajas del uso de las nuevas características de puerta de enlace RAS
A continuación se enumeran algunas de las ventajas de usar estas nuevas características de puerta de enlace RAS al diseñar la implementación de puerta de enlace RAS.

**Escalabilidad de puerta de enlace RAS**

Dado que puede Agregar tantas máquinas virtuales de puerta de enlace RAS como necesite para los grupos de puerta de enlace ras, puede escalar fácilmente la implementación de puerta de enlace RAS para optimizar el rendimiento y la capacidad. Al agregar máquinas virtuales a un grupo, puede configurar estas puertas de enlace RAS con conexiones VPN de sitio a sitio de cualquier tipo (IKEv2, L3, GRE), lo que elimina los cuellos de botella de capacidad sin tiempo de inactividad.

**Administración simplificada de puerta de enlace de sitios empresariales**

Cuando el inquilino tiene varios sitios empresariales, el inquilino puede configurar todos los sitios con una dirección IP de VPN de sitio a sitio remota y una única dirección IP de vecino remoto: la VIP de reflector de la puerta de enlace de RAS del centro de recursos de CSP para ese inquilino. Esto simplifica la administración de la puerta de enlace para los inquilinos.

**Corrección rápida de errores de puerta de enlace**

Para garantizar una respuesta de conmutación por error rápida, puede configurar el tiempo del parámetro keepalive de BGP entre las rutas perimetrales y el enrutador de control en un intervalo de tiempo corto, como menor o igual que diez segundos. Con este breve intervalo de mantenimiento de conexión, si se produce un error en un enrutador BGP Edge de puerta de enlace RAS, el error se detecta rápidamente y la controladora de red sigue los pasos proporcionados en las secciones anteriores. Esta ventaja podría reducir la necesidad de un protocolo de detección de errores independiente, como el protocolo de detección de reenvío bidireccional (BFD).