---
title: Arquitectura de implementación de puerta de enlace RAS
description: Puedes usar este tema para obtener información sobre la implementación de proveedor de servicio de nube (CSP) de puerta de enlace de RAS en Windows Server 2016, incluidos los grupos de puerta de enlace de RAS, ruta catadióptricos e implementar varias puertas de enlace de inquilinos individuales.
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
ms.openlocfilehash: 21b101e10dba3d3b9578d6804b4fd92fbbcd2167
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-deployment-architecture"></a>Arquitectura de implementación de puerta de enlace RAS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre la implementación de proveedor de servicio de nube (CSP) de puerta de enlace de RAS, incluidos los grupos de puerta de enlace de RAS, ruta catadióptricos e implementar varias puertas de enlace de inquilinos individuales.  
  
Las secciones siguientes proporcionan breve introducción a algunas de las nuevas características de puerta de enlace de RAS para que puedas comprender cómo usar estas características de diseño de la implementación de puerta de enlace.  
  
Además, se proporciona un ejemplo de implementación, incluida la información sobre el proceso de agregar nuevas inquilinos, sincronización de la ruta y enrutamiento del plano de datos, puerta de enlace y ruta espejo conmutación por error y más.  
  
Este tema contiene las siguientes secciones.  
  
-   [Uso de nuevas características de puerta de enlace RAS para diseñar la implementación](#bkmk_new)  
  
-   [Ejemplo de implementación](#bkmk_example)  
  
-   [Agregar nuevos inquilinos y espacio de direcciones (CA) de cliente EBGP interconexión](#bkmk_tenant)  
  
-   [Sincronización de la ruta y datos plano enrutamiento](#bkmk_route)  
  
-   [Cómo responde el controlador de red a la puerta de enlace RAS y conmutación por error de ruta espejo](#bkmk_failover)  
  
-   [Ventajas de usar las nuevas características de puerta de enlace RAS](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>Uso de nuevas características de puerta de enlace RAS para diseñar la implementación  
Puerta de enlace de RAS incluye varias características nuevas que cambian y mejoran la forma de implementar la infraestructura de puerta de enlace en el centro de datos.  
  
### <a name="bgp-route-reflector"></a>Ruta de BGP espejo  
La funcionalidad de espejo de la ruta de protocolo de puerta de enlace de borde (BGP) ya está incluido con la puerta de enlace de RAS y ofrece una alternativa a topología de malla completa BGP que normalmente es necesaria para la sincronización de la ruta entre enrutadores. Con la sincronización de malla completa, todos los enrutadores BGP deben conectarse con todos los demás enrutadores de la topología de enrutamiento. Cuando usas el espejo de ruta, sin embargo, espejo ruta es el único que se conecta con todos los demás enrutadores, llamados a los clientes de espejo de ruta BGP, lo que simplifica la sincronización de la ruta y reduce el tráfico de red. Ruta espejo aprende todas las rutas, calcula rutas mejor y redistribuye las rutas mejor a sus clientes BGP.  
  
Para obtener más información, consulta [Novedades de puerta de enlace de RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).  
  
### <a name="bkmk_pools"></a>Grupos de puerta de enlace  
En Windows Server 2016, puedes crear varios grupos de puerta de enlace de diferentes tipos. Grupos de puerta de enlace contienen muchas instancias de puerta de enlace de RAS y enrutan el tráfico de red entre redes físicas y virtuales.  
  
Para obtener más información, consulta [Novedades de puerta de enlace de RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [RAS puerta de enlace de alta disponibilidad](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_gps"></a>Puerta de enlace escalabilidad de grupo  
Puede escalar fácilmente un grupo de puerta de enlace hacia arriba o hacia abajo, agregando o quitando máquinas virtuales de puerta de enlace en el grupo. Retirada o la adición de puertas de enlace no interrumpir los servicios proporcionados por un grupo. También puedes agregar y quitar todos grupos de puertas de enlace.  
  
Para obtener más información, consulta [Novedades de puerta de enlace de RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [RAS puerta de enlace de alta disponibilidad](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_m"></a>Redundancia del grupo de puerta de enlace M + N  
Cada grupo de puerta de enlace es M + N redundante. Esto significa que un estoy ' número de máquinas virtuales de puerta de enlace active copia un número de'n ' de máquinas virtuales en modo de espera de la puerta de enlace. M + N redundancia ofrece más flexibilidad para determinar el nivel de confiabilidad que requieren cuando implementas RAS puerta de enlace.  
  
Para obtener más información, consulta [Novedades de puerta de enlace de RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) y [RAS puerta de enlace de alta disponibilidad](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_example"></a>Ejemplo de implementación  
La siguiente ilustración, proporciona un ejemplo con eBGP interconexión a través de conexiones de VPN de sitio a sitio configuradas entre dos inquilinos, Contoso y ejemplo de Woodgrove y el centro de datos de Fabrikam CSP.  
  
![eBGP interconexión por sitio a sitio VPN](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
En este ejemplo, Contoso requiere puerta de enlace más ancho de banda, lo que la decisión de diseño de la infraestructura de puerta de enlace para terminar el sitio de Contoso Los Ángeles en GW3 en lugar de GW2. Por este motivo, las conexiones de VPN de Contoso desde distintos sitios finalización en el centro de datos CSP en dos diferentes puertas de enlace.  
  
Ambos estas puertas de enlace, GW2 y GW3, fueron las puertas de enlace RAS primera configurado por el controlador de red cuando el CSP agrega los inquilinos de Contoso y Woodgrove a su infraestructura. Por este motivo, estas dos puertas de enlace están configurados como reflectores de ruta para estos clientes correspondientes (o inquilinos). GW2 es Contoso ruta espejo y GW3 es Woodgrove ruta espejo - además de ser el punto de terminación de puerta de enlace de CSP RAS para la conexión VPN con el sitio de alta calidad de Los Ángeles de Contoso.  
  
> [!NOTE]  
> Una puerta de enlace de RAS pueden enrutar el tráfico de red físicas y virtuales de inquilinos de hasta cien diferentes, según los requisitos de ancho de banda de cada inquilino.  
  
Ruta catadióptricos, GW2 envía las rutas de Contoso CA espacio al controlador de red y GW3 envía las rutas de ejemplo de Woodgrove CA espacio al controlador de red.  
  
Controlador de red inserta las directivas de virtualización de Hyper-V en red al Contoso y Woodgrove redes virtuales, así como las directivas RAS a las puertas de enlace de RAS y directivas para multiplexores (MUXes) que están configurados como un grupo de equilibrio de carga de Software de equilibrio de carga.  
  
## <a name="bkmk_tenant"></a>Agregar nueva inquilinos y espacio de direcciones de cliente de certificación (CA) eBGP Peering  
Cuando inicias sesión a un cliente nuevo y agrega al cliente como un inquilino nuevo en el centro de datos, puedes usar el siguiente proceso, gran parte de las cuales se realiza automáticamente los enrutadores eBGP RAS puerta de enlace y del mando de la red.  
  
1.  Aprovisionar una nueva red virtual y cargas de trabajo según los requisitos de tu inquilino.  
  
2.  Si es necesario, configurar la conectividad remota entre el sitio de empresa de inquilino remoto y su red virtual en el centro de datos. Al implementar una conexión VPN de sitio a sitio del inquilino, controlador de red se selecciona una VM de puerta de enlace RAS disponible desde el grupo de puerta de enlace disponibles automáticamente y configura la conexión.  
  
3.  Al configurar la máquina virtual de puerta de enlace de RAS del inquilino de nuevo, el controlador de red también configura la puerta de enlace RAS como un enrutador BGP y designa como espejo ruta del inquilino. Esto es así incluso en casos donde la puerta de enlace RAS actúa como una puerta de enlace, o como una puerta de enlace y el espejo de ruta, para otros inquilinos.  
  
4.  Según si el enrutamiento de CA espacio está configurado para redes de uso configurado de forma estática o enrutamiento BGP dinámico, el controlador de red configura las rutas estáticas correspondientes, vecinos BGP o ambos en la máquina virtual de puerta de enlace de RAS y el espejo de la ruta.  
  
    > [!NOTE]  
    > -   Después de que el controlador de red ha configurado una puerta de enlace de RAS y espejo de la ruta del inquilino, siempre que el mismo inquilino requiere una nueva conexión VPN de sitio a sitio, comprueba si hay un controlador de red para la capacidad disponible en esta máquina virtual de puerta de enlace de RAS. Si la puerta de enlace original puede atender la capacidad requerida, la nueva conexión de red también se configura en la misma máquina virtual puerta de enlace de RAS. Si la puerta de enlace de RAS máquina virtual no se puede controlar la capacidad adicional, el controlador de red selecciona una nueva máquina virtual puerta de enlace de RAS disponibles y configura la conexión nuevo en ella. Esta nueva RAS puerta de enlace de máquina virtual asociado con el inquilino se convierte en el cliente de espejo de ruta del espejo de ruta de RAS puerta de enlace de inquilino original.  
    > -   Dado que son grupos de puerta de enlace de RAS detrás equilibradores de carga de Software (SLBs), sitio a sitio VPN de inquilinos direcciones cada uso una sola dirección IP pública, llamada a una dirección IP virtual (VIP), que se traduce la SLBs una dirección IP interna del centro de datos, llamada a una dirección IP dinámica (DIP), una puerta de enlace de RAS que redirige el tráfico del inquilino de empresa. Esta asignación de dirección IP pública a privada por SLB garantiza que los túneles VPN de sitio a sitio se establecen correctamente entre los sitios de empresa y el CSP RAS puertas de enlace y reflectores de ruta.  
    >   
    >     Para obtener más información acerca de SLB, VIPs y DIP, consulta [equilibrio de carga de Software & #40; SLB & #41; para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
5.  Después de túnel VPN de sitio a sitio entre el sitio de empresa y el centro de datos CSP que se establece la puerta de enlace de RAS del inquilino de nuevo, las rutas estáticas que se asocian con los túneles están aprovisionadas automáticamente en los lados de la empresa y el CSP del túnel.  
  
6.  Con el espacio de CA BGP también se establece la eBGP interconexión entre los sitios de empresa y espejo de ruta de CSP RAS puerta de enlace, el enrutamiento.  
  
## <a name="bkmk_route"></a>Sincronización de la ruta y datos plano enrutamiento  
Después de establece eBGP interconexión entre sitios de empresa y el espejo de ruta de puerta de enlace de CSP RAS, espejo ruta aprende todas las rutas de empresa mediante el uso de enrutamiento BGP dinámico. Espejo ruta sincroniza estas rutas entre todos los clientes de espejo de ruta de modo que están configurados con el mismo conjunto de rutas.  
  
Ruta espejo también actualiza estas rutas consolidadas, mediante la sincronización de la ruta, al controlador de red. Controlador de red, a continuación, las rutas traduce en las directivas de virtualización de la red de Hyper-V y configura la red Fabric para asegurarse de que se aprovisione el enrutamiento de End-to-End ruta de acceso de datos. Este proceso hace que el inquilino red virtual accesible desde el inquilino Enterprise sitios.  
  
Para el enrutamiento de plano de datos, los paquetes que llegue a las máquinas virtuales de puerta de enlace de RAS directamente se enrutan a red virtual del inquilino, porque las rutas necesarias ahora están disponibles con todas las máquinas virtuales puerta de enlace de participantes RAS.  
  
Del mismo modo, con las directivas de virtualización de Hyper-V en red en su lugar, la red virtual del inquilino enruta paquetes directamente a las máquinas virtuales de puerta de enlace de RAS (sin necesidad de conocer la ruta espejo) y, a continuación, a los sitios de empresa a través de los túneles VPN de sitio a sitio.  
  
Además. Tráfico devuelto de la red virtual de inquilino en el sitio de empresa de inquilino remoto omite la SLBs, un proceso denominado devolver servidor directa (DSR).  
  
## <a name="bkmk_failover"></a>Cómo responde el controlador de red a la puerta de enlace RAS y conmutación por error de ruta espejo  
Siguiente se muestran dos escenarios de conmutación por error, una para los clientes de espejo de ruta de puerta de enlace de RAS - y otro para RAS puerta de enlace ruta reflectores incluida la información sobre cómo el controlador de red controla la conmutación por error para máquinas virtuales en configuración.  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>Error de la máquina virtual de un cliente RAS de puerta de enlace BGP ruta espejo  
Controlador de red tiene las siguientes acciones cuando se produce un error en un cliente RAS puerta de enlace ruta espejo.  
  
> [!NOTE]  
> Cuando una puerta de enlace de RAS no es un espejo de ruta para la infraestructura de un inquilino BGP, es un cliente de espejo de ruta en la infraestructura BGP del inquilino.  
  
-   Controlador de red selecciona una VM en modo de espera de puerta de enlace RAS disponible y aprovisiona la nueva máquina virtual puerta de enlace RAS con la configuración de la máquina virtual puerta de enlace de RAS erróneos.  
  
-   Controlador de red actualiza la configuración de SLB correspondiente para garantizar que los túneles de VPN de sitio a sitio de sitios de inquilino a la puerta de enlace de RAS error se establecen correctamente con el nuevo Gateway RAS.  
  
-   Controlador de red configura al cliente BGP espejo de ruta en la puerta de enlace nuevo.  
  
-   Controlador de red configura al nuevo cliente RAS puerta de enlace BGP ruta espejo como activa. La puerta de enlace RAS inicia inmediatamente la interconexión con de espejo del inquilino de ruta para compartir información de enrutamiento y habilitar eBGP interconexión para el sitio de empresa correspondiente.  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>Error de máquina virtual en un espejo de ruta BGP RAS puerta de enlace  
Controlador de red tiene las siguientes acciones cuando se produce un error en un espejo de ruta RAS BGP de puerta de enlace.  
  
-   Controlador de red selecciona una VM en modo de espera de puerta de enlace RAS disponible y aprovisiona la nueva máquina virtual puerta de enlace RAS con la configuración de la máquina virtual puerta de enlace de RAS erróneos.  
  
-   Controlador de red configura espejo ruta en la nueva máquina virtual de puerta de enlace de RAS y asigna a la nueva máquina virtual la misma dirección IP que se usó en la máquina virtual erróneas, lo que proporciona la integridad de la ruta a pesar de que el error de la máquina virtual.  
  
-   Controlador de red actualiza la configuración de SLB correspondiente para garantizar que los túneles de VPN de sitio a sitio de sitios de inquilino a la puerta de enlace de RAS error se establecen correctamente con el nuevo Gateway RAS.  
  
-   Controlador de red configura la nueva RAS puerta de enlace BGP ruta espejo máquina virtual como activa.  
  
-   Ruta espejo estará activa de inmediato. Establecer el túnel VPN de sitio a sitio para la empresa y espejo ruta usa eBGP interconexión e intercambia rutas con los enrutadores del sitio de empresa.  
  
-   Después de la selección de la ruta BGP, las actualizaciones de RAS puerta de enlace BGP ruta espejo inquilinos clientes espejo de ruta en el centro de datos y las rutas se sincroniza con el controlador de red, hacer que la ruta de acceso de datos End-to-End disponible para el tráfico del inquilino.  
  
## <a name="bkmk_advantages"></a>Ventajas de usar las nuevas características de puerta de enlace RAS  
Siguientes son algunas de las ventajas de usar estas nuevas características de puerta de enlace de RAS al diseñar la implementación de puerta de enlace de RAS.  
  
**Escalabilidad de puerta de enlace de RAS**  
  
Dado que puedes agregar tantas RAS puerta de enlace de las máquinas virtuales que necesite para grupos de puerta de enlace de RAS, puede escalar fácilmente la implementación de puerta de enlace de RAS para optimizar el rendimiento y capacidad. Cuando agregas máquinas virtuales a un grupo, puedes configurar estas puertas de enlace RAS con conexiones de VPN de sitio a sitio de cualquier tipo (IKEv2, L3, GRE), eliminar los cuellos de botella de capacidad y no tienen ninguna tiempo de inactividad.  
  
**Administración de puerta de enlace de sitio de empresa simplificada**  
  
Cuando el inquilino tiene varios sitios de empresa, puede configurar el inquilino todos los sitios con una dirección IP de VPN de sitio a sitio remoto y una dirección IP remota vecinos - su centro de datos CSP RAS puerta de enlace BGP ruta espejo VIP ese inquilino. Esto simplifica la administración de puerta de enlace de tus inquilinos.  
  
**Corrección rápida de error de puerta de enlace**  
  
Para garantizar una respuesta rápida de conmutación por error, puedes configurar el tiempo de conexión persistente BGP parámetro entre las rutas de borde y el enrutador de control a un intervalo de tiempo breve, como menor o igual a diez segundos. Con este intervalo de conexión persistente de mantener cortos, si se produce un error en un enrutador de borde BGP de puerta de enlace de RAS, rápidamente se detecta el error y controlador de red sigue los pasos descritos en las secciones anteriores. Esta ventaja puede reducir la necesidad de un protocolo de detección de error independiente, como protocolo de detección de reenvío bidireccional (BFD).  
  
 
  


