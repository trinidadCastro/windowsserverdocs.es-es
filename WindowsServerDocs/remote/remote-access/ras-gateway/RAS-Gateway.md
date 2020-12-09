---
title: Puerta de enlace RAS
description: En este tema, destinado a los profesionales de tecnologías de la información (TI), se proporciona información general sobre la puerta de enlace RAS, incluidos los modos y características de implementación de puerta de enlace RAS en Windows Server 2016.
manager: dougkim
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: lizross
author: eross-msft
ms.date: 05/23/2018
ms.openlocfilehash: 9e8dfd54fc919fa857ad360eb86baf47f8c3e274
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865214"
---
# <a name="ras-gateway"></a>Puerta de enlace RAS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

La puerta de enlace RAS es un enrutador de software y una puerta de enlace que puede usar en el modo de un solo inquilino o en el modo multiempresa.

- El modo de un **solo inquilino** permite a las organizaciones de cualquier tamaño implementar la puerta de enlace como un servidor de DirectAccess o una red privada virtual (VPN) perimetral con conexión a Internet. En el modo de un solo inquilino, puede implementar la puerta de enlace RAS en un servidor físico o una máquina virtual (VM) que ejecute Windows Server 2016.

- El **modo multiempresa** permite a los proveedores de servicios en la nube (CSP) y a las empresas usar la puerta de enlace ras para habilitar el enrutamiento del tráfico de red del centro de recursos y la nube entre redes físicas y virtuales, incluido Internet. En el modo multiempresa, se recomienda implementar la puerta de enlace RAS en las máquinas virtuales que ejecutan Windows Server 2016.

> [!NOTE]
> La puerta de enlace RAS es compatible con IPv4 e IPv6, incluyendo el reenvío de IPv4 e IPv6. Al configurar la puerta de enlace de RAS con la traducción de direcciones de red (NAT), solo se admite únicamente NAT44.

## <a name="who-will-be-interested-in-ras-gateway"></a>¿A quién le interesará la puerta de enlace RAS?

Si es administrador del sistema, arquitecto de redes u otro profesional de ti, la puerta de enlace RAS podría interesarle en una o varias de las siguientes circunstancias:

-   Diseña o proporciona soporte técnico a la infraestructura de TI de una organización que está utilizando o planeando la utilización de Hyper-V para implementar máquinas virtuales (VM) en redes virtuales.

-   Diseña o proporciona soporte técnico a la infraestructura de red de una organización que ha implementado o planea implementar tecnologías de nube.

-   Desea proporcionar conectividad de red completa entre redes físicas y redes virtuales.

-   Desea proporcionar a los clientes de su organización acceso a sus redes virtuales a través de Internet.

-   Desea proporcionar a los empleados de la organización acceso remoto a la red de la organización.

-   Quiere conectar oficinas en diferentes ubicaciones físicas a través de Internet.

En este tema, destinado a los profesionales de tecnologías de la información (TI), se proporciona información general sobre la puerta de enlace RAS, incluidos los modos y características de implementación de puerta de enlace RAS.

En este tema se incluyen las siguientes secciones.


-   [Modos de implementación de puerta de enlace RAS](#bkmk_modes)

-   [Agrupación en clústeres de puerta de enlace RAS para alta disponibilidad](#bkmk_clustering)

-   [Características de puerta de enlace RAS](#bkmk_features)

-   [Escenarios de implementación de puerta de enlace RAS](#bkmk_deploy)

-   [Herramientas de administración de puerta de enlace RAS](#bkmk_manage)




## <a name="ras-gateway-deployment-modes"></a><a name="bkmk_modes"></a>Modos de implementación de puerta de enlace RAS
La puerta de enlace RAS incluye los siguientes modos de implementación.

### <a name="single-tenant-mode"></a>Modo de un solo inquilino
Para la mayoría de las organizaciones, el uso de la puerta de enlace RAS en modo de un solo inquilino es la configuración típica. En el modo de un solo inquilino, puede implementar la puerta de enlace RAS como un servidor VPN perimetral, un servidor perimetral de DirectAccess o ambos simultáneamente. En esta configuración, la puerta de enlace RAS proporciona a los empleados remotos la conectividad a la red mediante conexiones VPN o DirectAccess. Además, el modo de un solo inquilino le permite conectar oficinas en diferentes ubicaciones físicas a través de Internet.

### <a name="multitenant-mode"></a>Modo multiempresa
Si su organización es un CSP o una empresa con varios inquilinos, puede implementar la puerta de enlace RAS en el modo multiempresa para proporcionar el enrutamiento del tráfico de red hacia y desde redes físicas y virtuales.

Multiinquilino es la capacidad de una infraestructura de nube para admitir las cargas de trabajo de máquinas virtuales de varios inquilinos, pero aislarlas entre sí, mientras que todas las cargas de trabajo se ejecutan en la misma infraestructura. Las distintas cargas de trabajo de un inquilino individual pueden interconectarse y administrarse de manera remota, pero estos sistemas no se interconectan con las cargas de trabajo de los demás inquilinos, ni tampoco pueden los demás inquilinos administrarlas de manera remota.

Por ejemplo, una empresa puede tener muchas subredes virtuales distintas, cada una de ellas dedicada a prestar servicio a un departamento específico, como Investigación y desarrollo o Contabilidad. En otro ejemplo, un CSP tiene muchos inquilinos con subredes virtuales aisladas en el mismo centro de datos físico. En ambos casos, la puerta de enlace RAS puede enrutar el tráfico hacia y desde cada inquilino manteniendo el aislamiento diseñado de cada inquilino. Esta capacidad hace que la puerta de enlace RAS sea compatible con multiinquilino.

Las redes virtuales se crean con virtualización de red de Hyper-V, que es una tecnología introducida en Windows Server 2012 y se ha mejorado en Windows Server 2016. La puerta de enlace RAS se integra con la virtualización de red de Hyper-V y es capaz de enrutar el tráfico de red de forma eficaz en circunstancias en las que hay muchos clientes (o inquilinos) que tienen redes virtuales aisladas en el mismo centro de recursos.

La virtualización de red de Hyper-V proporciona la capacidad de implementar una red de máquina virtual (VM) que es independiente de la red física subyacente. Con las redes de VM, que se componen de una o más subredes virtuales, la ubicación física exacta de una subred IP está desacoplada de la topología de red virtual. Como resultado, puede trasladar fácilmente sus subredes locales a la nube, a la vez que conserva las direcciones IP y la topología existentes en la nube. Esta capacidad de preservar la infraestructura permite a los servicios existentes seguir trabajando, sin tener en cuenta la ubicación física de las subredes. Es decir, Virtualización de red de Hyper-V hace posible una nube híbrida perfecta.

> [!NOTE]
> La virtualización de red de Hyper-V es una tecnología de superposición de red que usa la encapsulación de enrutamiento genérico ([NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00)) de virtualización de red, que permite a los inquilinos traer su propio espacio de direcciones y permite a los CSP una mejor escalabilidad que la posible mediante el uso de redes VLAN para el aislamiento de inquilinos.

En Windows Server 2016, la puerta de enlace RAS enruta el tráfico de red entre la red física y los recursos de red de VM, independientemente de dónde se encuentren los recursos. Puede usar la puerta de enlace RAS para enrutar el tráfico de red entre las redes físicas y virtuales en la misma ubicación física o en muchas ubicaciones físicas distintas.

Por ejemplo, si tiene una red física y una red virtual en la misma ubicación física, puede implementar un equipo que ejecute Hyper-V configurado con una máquina virtual de puerta de enlace RAS para que actúe como puerta de enlace de reenvío y Enrute el tráfico entre las redes virtuales y físicas.

En otro ejemplo, si las redes virtuales existen en la nube, el CSP puede implementar una puerta de enlace RAS para que pueda crear una conexión de sitio a sitio de red privada virtual (VPN) entre el servidor VPN y la puerta de enlace RAS del CSP. Cuando se establece este vínculo, puede conectarse a los recursos virtuales en la nube a través de la conexión VPN.

Para más información, consulte [Alta disponibilidad de la puerta de enlace de RAS](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).

## <a name="clustering-ras-gateway-for-high-availability"></a><a name="bkmk_clustering"></a>Agrupación en clústeres de puerta de enlace RAS para alta disponibilidad
La puerta de enlace RAS se implementa en un equipo dedicado que ejecuta Hyper-V y que está configurado con una máquina virtual. Después, la máquina virtual se configura como puerta de enlace de RAS.

Para lograr una alta disponibilidad de los recursos de red, puede implementar la puerta de enlace RAS con conmutación por error mediante dos servidores host físicos que ejecutan Hyper-V y cada uno de ellos ejecuta también una máquina virtual (VM) configurada como puerta de enlace. Las VM de la puerta de enlace están configuradas, por lo tanto, como clúster para proporcionar protección de conmutación por error frente a las interrupciones de red y los errores de hardware.

Por ejemplo, si su organización es una empresa con una implementación de nube privada, es posible que solo necesite dos máquinas virtuales de puerta de enlace RAS, cada una de las cuales se instala en un equipo diferente que ejecuta Hyper-V. En este escenario, las máquinas virtuales de puerta de enlace RAS se agregan a un clúster para proporcionar alta disponibilidad.

En otro ejemplo, si su organización es un proveedor de servicios en la nube (CSP) con 200 inquilinos en su centro de recursos, puede usar ocho máquinas virtuales de puerta de enlace RAS, con cada par de máquinas virtuales de puerta de enlace RAS en clúster que proporcionan servicios de enrutamiento para los inquilinos de 50. En este escenario, dos equipos que ejecutan Hyper-V tienen cuatro máquinas virtuales configuradas como puertas de enlace de RAS. Después, se configuran cuatro clústeres de máquina virtual de puerta de enlace RAS, cada uno de los cuales contiene una máquina virtual de cada equipo que ejecuta Hyper-V.

Cuando se implementa la puerta de enlace de RAS, los servidores de host que ejecutan Hyper-V y las máquinas virtuales que se configuran como puertas de enlace deben ejecutar Windows Server 2012 R2 o Windows Server 2016.

## <a name="ras-gateway-features"></a><a name="bkmk_features"></a>Características de puerta de enlace RAS
La puerta de enlace RAS incluye las siguientes capacidades.

-   **VPN de sitio a sitio**. Esta característica de puerta de enlace RAS le permite conectar dos redes en diferentes ubicaciones físicas a través de Internet mediante una conexión VPN de sitio a sitio. Si tiene una oficina principal y varias sucursales, puede implementar una puerta de enlace RAS perimetral en cada ubicación y crear conexiones de sitio a sitio para proporcionar el flujo de tráfico de red entre las ubicaciones. En el caso de los CSP que hospedan muchos inquilinos en su centro de recursos, la puerta de enlace de RAS proporciona una solución de puerta de enlace multiinquilino que permite a los inquilinos tener acceso a sus recursos y administrarlos a través de conexiones VPN de sitio a sitio desde sitios remotos y que permite el flujo de tráfico de red entre los recursos virtuales del centro de bits y su red

-   **VPN de punto a sitio**. Esta característica de puerta de enlace RAS permite que los empleados o administradores de la organización se conecten a la red de su organización desde ubicaciones remotas. Para implementaciones de un solo inquilino de puerta de enlace RAS, los empleados remotos pueden conectarse a la red de la organización mediante una conexión VPN. Esta conexión les permite usar recursos de la red interna, como sitios web de la intranet y servidores de archivos. En el caso de las implementaciones multiinquilino, los administradores de red de inquilinos pueden usar conexiones VPN de punto a sitio para tener acceso a los recursos de red virtual en el centro de recursos de CSP.

-   **Enrutamiento dinámico con protocolo de puerta de enlace de borde (BGP)**. BGP reduce la necesidad de configuración de enrutamiento manual en los enrutadores, ya que es un protocolo de enrutamiento dinámico y aprende automáticamente las rutas entre sitios que están conectados mediante conexiones VPN de sitio a sitio. Si su organización tiene varios sitios que están conectados mediante enrutadores habilitados para BGP, como la puerta de enlace RAS, BGP permite a los enrutadores calcular y usar rutas válidas entre sí automáticamente en caso de que se produzcan interrupciones o errores en la red. Para obtener más información, consulte [RFC 4271](https://tools.ietf.org/html/rfc4271).

-   **Traducción de direcciones de red (NAT)**. La traducción de direcciones de red (NAT) permite compartir una conexión a Internet pública a través de una única interfaz con una sola dirección IP pública. Los equipos de la red privada utilizan direcciones privadas no enrutables. NAT asigna las direcciones privadas a la dirección pública. Esta característica de puerta de enlace RAS permite que los empleados de la organización con implementaciones de un solo inquilino tengan acceso a recursos de Internet desde detrás de la puerta de enlace. En el caso de los CSP, esta característica permite que las aplicaciones que se ejecutan en máquinas virtuales de inquilino tengan acceso a Internet. Por ejemplo, una máquina virtual de inquilino que está configurada como un servidor web puede ponerse en contacto con los recursos financieros externos para procesar las transacciones de tarjetas de crédito.


## <a name="ras-gateway-deployment-scenarios"></a><a name="bkmk_deploy"></a>Escenarios de implementación de puerta de enlace RAS
A continuación se indican los escenarios de implementación recomendados para la puerta de enlace RAS.

-   **Perimetral empresarial: implementación de un solo inquilino**. Con la implementación empresarial de un solo inquilino, puede conectar una física a varias ubicaciones físicas a través de Internet mediante el uso de la característica VPN de sitio a sitio, y Protocolo de puerta de enlace de borde (BGP) le permite usar el enrutamiento dinámico. También puede proporcionar a los empleados remotos acceso a la red de la organización con conexiones VPN de punto a sitio y conexiones de DirectAccess. (Las conexiones de DirectAccess están siempre activadas y también ofrecen la ventaja de que puede administrar fácilmente los equipos que están conectados mediante DirectAccess, ya que están conectados siempre que están conectados a Internet). También puede configurar puertas de enlace RAS de empresa de un solo inquilino con NAT, de modo que los equipos de la intranet puedan comunicarse fácilmente con Internet.

-   **Perimetral del proveedor de servicios en la nube: implementación multiinquilino**. La implementación multiinquilino de puerta de enlace RAS para CSP le permite ofrecer a sus inquilinos todas las características que están disponibles con la implementación de un solo inquilino de Enterprise Edge. Las conexiones VPN de sitio a sitio entre las redes virtuales de inquilino de su centro de recursos y las ubicaciones de red de inquilinos a través de Internet significan que los inquilinos tienen acceso sin problemas a sus recursos en la nube todo el tiempo. El acceso a VPN de punto a sitio para inquilinos significa que los administradores de inquilinos siempre pueden conectarse a sus redes virtuales en el centro de recursos para administrar sus recursos. BGP proporciona enrutamiento dinámico y mantiene los inquilinos conectados a sus activos incluso cuando se producen problemas de red en Internet o en otro lugar. Y NAT permite que las máquinas virtuales de inquilino se conecten a los recursos de Internet, como los recursos de procesamiento de tarjetas de crédito.

## <a name="ras-gateway-management-tools"></a><a name="bkmk_manage"></a>Herramientas de administración de puerta de enlace RAS
A continuación se muestran las herramientas de administración para la puerta de enlace RAS.

-   En Windows Server 2016, para implementar un enrutador de puerta de enlace RAS, debe usar los comandos de Windows PowerShell. Para obtener más información, consulte  [cmdlets de acceso remoto](/powershell/module/remoteaccess) para windows Server 2016 y Windows 10.

-   En System Center 2012 R2 Virtual Machine Manager (VMM), la puerta de enlace RAS se denomina puerta de enlace de Windows Server. Hay disponible un conjunto limitado de opciones de configuración de Protocolo de puerta de enlace de borde (BGP) en la interfaz de software de VMM, incluidas la **dirección IP BGP local** y **los números de sistema autónomo (ASN)**, la **lista de direcciones IP del par BGP** y **los valores de ASN**. Puede, no obstante, utilizar los comandos de Windows PowerShell BGP de acceso remoto para configurar el resto de características de la puerta de enlace de Windows Server. Para obtener más información, consulte  [Virtual Machine Manager (VMM)](/system-center/vmm/overview) y [cmdlets de acceso remoto](/system-center/vmm/overview) para Windows Server 2016 y Windows 10.

## <a name="related-topics"></a>Temas relacionados
- [Alta disponibilidad de puerta de enlace RAS](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)
- [Tunelización de GRE en Windows Server 2016](gre-tunneling-windows-server.md)
- [Rendimiento de túnel GRE de puerta de enlace de RAS](RAS-Gateway-GRE-Perf.md)
