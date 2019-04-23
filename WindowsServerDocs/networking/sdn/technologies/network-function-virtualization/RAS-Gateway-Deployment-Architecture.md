---
title: Arquitectura de implementación de puerta de enlace de RAS
description: Puede utilizar este tema para obtener información acerca de la implementación del proveedor de servicios en la nube (CSP) de la puerta de enlace de RAS en Windows Server 2016, incluidos los grupos de puerta de enlace RAS, reflectores de ruta e implementar varias puertas de enlace para los inquilinos individuales.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3895e25a2af0437deb9eebe4ad89b110cfc9f2b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870276"
---
# <a name="ras-gateway-deployment-architecture"></a>Arquitectura de implementación de puerta de enlace de RAS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información acerca de la implementación del proveedor de servicios en la nube (CSP) de la puerta de enlace de RAS, incluidos los grupos de puerta de enlace RAS, reflectores de ruta e implementar varias puertas de enlace para los inquilinos individuales.  
  
Las siguientes secciones ofrecen información general sobre algunas de las nuevas características de puerta de enlace RAS para que puedan comprender cómo usar estas características en el diseño de la implementación de puerta de enlace.  
  
Además, se proporciona un ejemplo de implementación, incluida la información sobre el proceso de adición de nuevos inquilinos, sincronización de ruta y enrutamiento de plano de datos, puerta de enlace y conmutación por error de Reflector de ruta y más.  
  
En este tema se incluyen las siguientes secciones.  
  
-   [Uso de nuevas características de puerta de enlace RAS para diseñar la implementación](#bkmk_new)  
  
-   [Ejemplo de implementación](#bkmk_example)  
  
-   [Adición de nuevos inquilinos y el espacio de direcciones (CA) de cliente EBGP emparejamiento](#bkmk_tenant)  
  
-   [Enrutamiento de plano de datos y sincronización de ruta](#bkmk_route)  
  
-   [Cómo responde la controladora de red para la puerta de enlace RAS y conmutación por error de ruta Reflector](#bkmk_failover)  
  
-   [Ventajas de usar nuevas características de la puerta de enlace RAS](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>Uso de nuevas características de puerta de enlace RAS para diseñar la implementación  
Puerta de enlace RAS incluye varias características nuevas que cambiar y mejoran la manera en que implementa la infraestructura de la puerta de enlace en su centro de datos.  
  
### <a name="bgp-route-reflector"></a>Ruta BGP Reflector  
La capacidad de Reflector de rutas de Border Gateway Protocol (BGP) ya está incluido con la puerta de enlace RAS y proporciona una alternativa a la topología de malla completa de BGP que normalmente se requiere para la sincronización de la ruta entre enrutadores. Con la sincronización de malla completa, deben conectar todos los enrutadores BGP con todos los demás enrutadores de la topología de enrutamiento. Cuando se usa la ruta Reflector, sin embargo, el Reflector de ruta es el único que se conecta con todos los demás enrutadores, llama a los clientes de Reflector de rutas BGP, con lo que se simplifica la sincronización de la ruta y reduce el tráfico de red. El Reflector de ruta aprende todas las rutas, calcula mejores rutas y redistribuye las rutas mejor a sus clientes BGP.  
  
Para obtener más información, consulte [Novedades de la puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).  
  
### <a name="bkmk_pools"></a>Grupos de puerta de enlace  
En Windows Server 2016, puede crear muchos grupos de puerta de enlace de tipos diferentes. Los grupos de puerta de enlace contienen muchas instancias de puerta de enlace de RAS y enrutar el tráfico de red entre redes físicas y virtuales.  
  
Para obtener más información, consulte [Novedades de la puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [alta disponibilidad de puerta de enlace de RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_gps"></a>Escalabilidad del grupo de servidores de puerta de enlace  
Puede escalar fácilmente un grupo de servidores de puerta de enlace hacia arriba o hacia abajo agregando o quitando máquinas virtuales de puerta de enlace en el grupo. Eliminación o adición de puertas de enlace no interrumpe los servicios proporcionados por un grupo. También puede agregar y quitar grupos completos de las puertas de enlace.  
  
Para obtener más información, consulte [Novedades de la puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [alta disponibilidad de puerta de enlace de RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_m"></a>Redundancia de grupo de servidores de puerta de enlace M+N  
Cada grupo de servidores de puerta de enlace es con redundancia M+N. Esto significa que un ' m ' número de máquinas virtuales de puerta de enlace activo estén respaldado por un número de "n" de las máquinas virtuales de puerta de enlace en espera. M + N redundancia proporciona más flexibilidad para determinar el nivel de confiabilidad que requieren al implementar la puerta de enlace RAS.  
  
Para obtener más información, consulte [Novedades de la puerta de enlace RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [alta disponibilidad de puerta de enlace de RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_example"></a>Ejemplo de implementación  
La ilustración siguiente proporciona un ejemplo con eBGP emparejamiento a través de conexiones de VPN de sitio a sitio configuradas entre dos inquilinos, Contoso y Woodgrove y el centro de datos de Fabrikam CSP.  
  
![eBGP emparejamiento a través de VPN de sitio a sitio](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
En este ejemplo, Contoso requiere ancho de banda de la puerta de enlace adicional, dando lugar a la decisión de diseño de infraestructura de puerta de enlace para terminar el sitio de Contoso Los Ángeles en GW3 en lugar de GW2. Por este motivo, las conexiones VPN de Contoso desde sitios distintos finalización en el centro de datos CSP en dos puertas de enlace diferentes.  
  
Ambas de estas puertas de enlace, GW2 y GW3, eran las puertas de enlace de RAS primera configurado por controladora de red cuando el CSP agrega los inquilinos Contoso y Woodgrove a su infraestructura. Por este motivo, estos dos puertas de enlace se configuran como los reflectores de ruta para estos clientes correspondientes (o inquilinos). GW2 es el Reflector de ruta de Contoso y GW3 es el Reflector de ruta de Woodgrove - además de ser el punto de terminación de la puerta de enlace de RAS de CSP para la conexión VPN con el sitio de la sede central de Contoso Los Ángeles.  
  
> [!NOTE]  
> Una puerta de enlace de RAS puede enrutar el tráfico de red virtual y física hasta cien de inquilinos diferentes, según los requisitos de ancho de banda de cada inquilino.  
  
Como los reflectores de ruta, GW2 envía las rutas de espacio de CA de Contoso a la controladora de red y GW3 envía las rutas de espacio de la entidad de certificación de Woodgrove controladora de red.  
  
Controladora de red inserta las directivas de virtualización de red de Hyper-V en las redes virtuales de Contoso y Woodgrove, así como directivas RAS para las puertas de enlace de RAS y directivas para multiplexores (MUX) que están configurados como un equilibrio de carga de Software de equilibrio de carga grupo.  
  
## <a name="bkmk_tenant"></a>Adición de nuevos inquilinos y el espacio de direcciones de cliente (CA) eBGP emparejamiento  
Cuando se inicie a un nuevo cliente y agregar al cliente como un nuevo inquilino en su centro de datos, puede usar el siguiente proceso, gran parte de los cuales se realiza automáticamente por controladora de red y puerta de enlace RAS enrutadores BGP.  
  
1.  Aprovisionar una nueva red virtual y las cargas de trabajo según los requisitos de su inquilino.  
  
2.  Si es necesario, configure la conectividad remota entre el sitio de empresa de inquilinos remotos y su red virtual en su centro de datos. Al implementar una conexión VPN de sitio a sitio para el inquilino, controladora de red selecciona automáticamente una VM de puerta de enlace de RAS disponible del grupo disponible de la puerta de enlace y configura la conexión.  
  
3.  Al configurar la máquina virtual de puerta de enlace de RAS para el nuevo inquilino, controladora de red también configura la puerta de enlace de RAS como un enrutador de BGP y designa que el Reflector de ruta para el inquilino. Esto es cierto incluso en circunstancias donde la puerta de enlace de RAS actúa como una puerta de enlace o como una puerta de enlace y la ruta Reflector, para otros inquilinos.  
  
4.  Dependiendo de si el enrutamiento de espacio CA está configurado para redes de uso configurado estáticamente o enrutamiento dinámico de BGP, controladora de red configura las rutas estáticas correspondientes, vecinos de BGP o en la máquina virtual de puerta de enlace de RAS y Reflector de ruta.  
  
    > [!NOTE]  
    > -   Después de controladora de red haya configurado una puerta de enlace RAS y Reflector de ruta para el inquilino, siempre que sea el mismo inquilino requiere una nueva conexión VPN de sitio a sitio, comprobaciones de controladora de red para la capacidad disponible en esta máquina virtual de puerta de enlace de RAS. Si la puerta de enlace original puede atender la capacidad requerida, la nueva conexión de red también se configura en la misma máquina virtual de puerta de enlace de RAS. Si la máquina virtual de puerta de enlace de RAS no puede controlar la capacidad adicional, controladora de red selecciona una máquina virtual nueva puerta de enlace de RAS disponibles y configura la nueva conexión en él. Esta nueva VM de puerta de enlace de RAS asociado con el inquilino se convierte en el cliente de Reflector de ruta del inquilino original Reflector de ruta de puerta de enlace de RAS.  
    > -   Dado que son grupos de puerta de enlace RAS detrás de equilibradores de carga de Software (SLBs), cada uno utiliza una única dirección IP pública, llama a una dirección IP virtual (VIP), que se traduce el SLBs en una dirección IP interna del centro de datos, las direcciones VPN de sitio a sitio de los inquilinos se llama a un dirección IP dinámica (DIP), una puerta de enlace de RAS que enruta el tráfico para el inquilino de empresa. Esta asignación de dirección IP pública a privada por SLB garantiza que los túneles VPN de sitio a sitio se establecen correctamente entre los sitios de empresa y las puertas de enlace de RAS de CSP y reflectores de ruta.  
    >   
    >     Para obtener más información acerca de SLB, las direcciones VIP y DIP, consulte [equilibrio de carga de Software &#40;SLB&#41; para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
5.  Después de túnel VPN de sitio a sitio entre el sitio de empresa y el centro de datos CSP que puerta de enlace de RAS se establece para el nuevo inquilino, las rutas estáticas que están asociadas con los túneles se aprovisionan automáticamente en la empresa y CSP lados del túnel .  
  
6.  Con BGP del espacio de CA se establece también el enrutamiento, el emparejamiento entre los sitios de empresa y el Reflector de ruta de puerta de enlace de RAS CSP BGP.  
  
## <a name="bkmk_route"></a>Enrutamiento de plano de datos y sincronización de ruta  
Después de establecer el emparejamiento BGP entre sitios de empresa y el Reflector de ruta de puerta de enlace de RAS CSP, el Reflector de ruta se entera de todas las rutas de la empresa mediante el enrutamiento dinámico de BGP. El Reflector de ruta sincroniza estas rutas entre todos los clientes de Reflector de ruta para que se configuran con el mismo conjunto de rutas.  
  
Reflector de ruta también actualiza estas rutas consolidadas, mediante la sincronización de la ruta, controladora de red. Controladora de red, a continuación, traduce las rutas en las directivas de virtualización de red de Hyper-V y configura la red del tejido para asegurarse de que se aprovisiona el enrutamiento de la ruta de acceso de datos de extremo a otro. Este proceso hace que el inquilino de red virtual accesible desde el inquilino Enterprise sitios.  
  
Para el enrutamiento de plano de datos, los paquetes que llegan a las máquinas virtuales de puerta de enlace de RAS se enrutan directamente a la red virtual del inquilino porque las rutas necesarias ya están disponibles con todas las máquinas virtuales que participan puerta de enlace de RAS.  
  
De forma similar, con las directivas de virtualización de red de Hyper-V en su lugar, la red virtual de inquilino enruta paquetes directamente a las máquinas virtuales de puerta de enlace de RAS (sin necesidad de conocer el Reflector de ruta) y, a continuación, en los sitios de empresa a través de los túneles VPN de sitio a sitio .  
  
Además. tráfico de retorno de la red virtual de inquilino para el sitio de inquilinos remotos Enterprise omite el SLBs, un proceso denominado Direct Server Return (DSR).  
  
## <a name="bkmk_failover"></a>Cómo responde la controladora de red para la puerta de enlace RAS y conmutación por error de ruta Reflector  
Estos son dos escenarios de conmutación por error posibles: una para los clientes de Reflector de enrutamiento de puerta de enlace de RAS - y otra para reflectores de ruta de puerta de enlace de RAS incluida información acerca de cómo controladora de red controla la conmutación por error para las máquinas virtuales en cualquiera de las configuraciones.  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>Error de la máquina virtual de un cliente de Reflector de ruta BGP de puerta de enlace RAS  
Controladora de red realiza las siguientes acciones cuando se produce un error en un cliente de Reflector de enrutamiento de puerta de enlace de RAS.  
  
> [!NOTE]  
> Cuando una puerta de enlace de RAS no es una ruta NET Reflector de infraestructura BGP del inquilino, es un cliente de Reflector de ruta en la infraestructura BGP del inquilino.  
  
-   Controladora de red selecciona una máquina virtual disponible puerta de enlace de RAS en espera y aprovisiona la máquina virtual nueva puerta de enlace de RAS con la configuración de la VM de puerta de enlace de RAS con errores.  
  
-   Controladora de red actualiza la configuración de SLB correspondiente para asegurarse de que los túneles VPN de sitio a sitio desde sitios de inquilino a la puerta de enlace de RAS con errores se establecen correctamente con la nueva puerta de enlace de RAS.  
  
-   Controladora de red configura al cliente de Reflector de rutas BGP en la nueva puerta de enlace.  
  
-   Controladora de red configura al nuevo cliente de Reflector de enrutamiento de BGP de puerta de enlace de RAS como activa. La puerta de enlace de RAS iniciará inmediatamente el emparejamiento de Reflector del inquilino de ruta para compartir información de enrutamiento y habilitar el emparejamiento BGP para el sitio de empresa correspondiente.  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>Error de la máquina virtual un reflector ruta BGP de puerta de enlace RAS  
Controladora de red realiza las siguientes acciones cuando se produce un error en un Reflector de ruta de BGP de puerta de enlace de RAS.  
  
-   Controladora de red selecciona una máquina virtual disponible puerta de enlace de RAS en espera y aprovisiona la máquina virtual nueva puerta de enlace de RAS con la configuración de la VM de puerta de enlace de RAS con errores.  
  
-   Controladora de red configura el Reflector de ruta en la nueva máquina virtual de puerta de enlace de RAS y asigna la nueva máquina virtual en la misma dirección IP que se usó en la máquina virtual con errores, lo que proporciona integridad de la ruta a pesar del error de la máquina virtual.  
  
-   Controladora de red actualiza la configuración de SLB correspondiente para asegurarse de que los túneles VPN de sitio a sitio desde sitios de inquilino a la puerta de enlace de RAS con errores se establecen correctamente con la nueva puerta de enlace de RAS.  
  
-   Controladora de red configura la nueva VM de Reflector de RAS de puerta de enlace de BGP ruta como activa.  
  
-   El Reflector de ruta estará activo de inmediato. Se establece el túnel VPN de sitio a sitio para la empresa, y el Reflector de ruta usa el emparejamiento BGP e intercambia rutas con los enrutadores de sitio de empresa.  
  
-   Después de la selección de la ruta BGP, las actualizaciones de Reflector de enrutamiento de BGP de puerta de enlace de RAS los clientes de Reflector de ruta en el centro de datos de inquilino y las rutas se sincroniza con la controladora de red, la disposición de la ruta de acceso de datos-to-End para tráfico de inquilinos.  
  
## <a name="bkmk_advantages"></a>Ventajas de usar nuevas características de la puerta de enlace RAS  
Siguientes son algunas de las ventajas de usar estas nuevas características de la puerta de enlace RAS al diseñar la implementación de puerta de enlace RAS.  
  
**Escalabilidad de la puerta de enlace RAS**  
  
Dado que puede agregar tantas máquinas de virtuales de puerta de enlace de RAS como necesite para grupos de puerta de enlace RAS, puede escalar fácilmente la implementación de puerta de enlace RAS para optimizar el rendimiento y capacidad. Al agregar máquinas virtuales a un grupo, puede configurar estas puertas de enlace de RAS con conexiones de VPN de sitio a sitio de cualquier tipo (IKEv2, L3, GRE), eliminar los cuellos de botella de capacidad sin tiempo de inactividad.  
  
**Administración de puerta de enlace de sitio empresarial simplificada**  
  
Cuando el inquilino tiene varios sitios de empresa, puede configurar el inquilino de todos los sitios con una dirección IP de VPN de sitio a sitio remoto y una dirección IP remota vecinos - su centro de datos CSP RAS puerta de enlace de BGP Route Reflector VIP para dicho inquilino. Esto simplifica la administración de puerta de enlace para los inquilinos.  
  
**Corrección rápida de errores de puerta de enlace**  
  
Para garantizar una respuesta rápida conmutación por error, puede configurar el tiempo de parámetro Keepalive BGP entre las rutas de borde y el enrutador de control para un intervalo de hora corta, como menor o igual a diez segundos. Con este intervalo corto keep alive, si se produce un error en un enrutador perimetral de BGP de puerta de enlace de RAS, rápidamente se detecta el error y controladora de red sigue los pasos proporcionados en las secciones anteriores. Esta ventaja puede reducir la necesidad de un protocolo de detección de error independientes, como el protocolo de detección de reenvío bidireccional (BFD).  
  
 
  


