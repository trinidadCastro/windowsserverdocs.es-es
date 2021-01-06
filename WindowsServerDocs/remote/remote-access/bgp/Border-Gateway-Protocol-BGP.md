---
title: Protocolo de puerta de enlace de borde (BGP)
description: Puede usar este tema para obtener una descripción de Protocolo de puerta de enlace de borde (BGP) en Windows Server 2016, incluidas las topologías de implementación compatibles con BGP y las características y capacidades de BGP.
manager: brianlic
ms.topic: article
ms.assetid: 78cc2ce3-a48e-45db-b402-e480b493fab1
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: b6cbf1780e35b1181648761990a344093aaa8f58
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950081"
---
# <a name="border-gateway-protocol-bgp"></a>Protocolo de puerta de enlace de borde (BGP)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener una descripción del Protocolo de puerta de enlace de borde (BGP), incluidas las topologías de implementación admitidas por BGP y las características y capacidades de BGP.

> [!NOTE]
> Además de este tema, está disponible la siguiente documentación de BGP.
>
> -   [Referencia de comandos de Windows PowerShell de BGP](../../remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)

En este tema se incluyen las siguientes secciones.

-   [Topologías de implementación compatibles con BGP](#bkmk_top)

-   [Características de BGP](#bkmk_features)

Cuando se configura en una puerta de enlace ras del servicio de acceso remoto de Windows Server 2016 \( \) en el modo multiempresa, protocolo de puerta de enlace de borde (BGP) le proporciona la capacidad de administrar el enrutamiento del tráfico de red entre las redes de VM de los inquilinos y sus sitios remotos. También puede usar BGP para implementaciones de puerta de enlace RAS de un solo inquilino y al implementar el acceso remoto como un \( enrutador LAN de red de área local \) .

BGP reduce la necesidad de configuración de enrutamiento manual en los enrutadores, ya que es un protocolo de enrutamiento dinámico y aprende automáticamente las rutas entre sitios que están conectados mediante conexiones VPN de sitio a sitio.

Para usar el enrutamiento de BGP, debe instalar el servicio de **acceso remoto \( ras \)** y/o el servicio de rol de **enrutamiento** del rol de servidor de acceso remoto en un equipo o máquina virtual \( \) (VM). el tipo de sistema que use dependerá de si tiene una implementación multiinquilino:

-   Para una implementación multiinquilino, se recomienda instalar la puerta de enlace RAS en una o más máquinas virtuales. El uso de varias máquinas virtuales proporciona alta disponibilidad. La puerta de enlace RAS es capaz de controlar varias conexiones de varios inquilinos y consta de un host de Hyper-V y una máquina virtual que realmente está configurada como puerta de enlace. Esta puerta de enlace se configura con conexiones VPN de sitio a sitio como un enrutador BGP multiinquilino para intercambiar rutas de subred de CSP de proveedor de servicios en la nube y inquilino de Exchange \( \) .

-   En el caso de una implementación de puerta de enlace perimetral de un solo inquilino o una implementación de enrutador LAN, puede instalar la puerta de enlace RAS en un equipo físico o en una máquina virtual.

> [!IMPORTANT]
> Cuando se instala una puerta de enlace RAS, debe especificar si se habilita BGP para cada inquilino mediante el comando **enable-RemoteAccessRoutingDomain** de Windows PowerShell con el valor del parámetro de **tipo** **All**. Para instalar el acceso remoto como un enrutador de LAN con BGP habilitado sin funcionalidades multiinquilino, puede usar el comando **install-RemoteAccess-VpnType RoutingOnly**.
>
> En el ejemplo de código siguiente se muestra cómo instalar RAS en modo multiinquilino con todas las características de RAS (VPN de punto a sitio, VPN de sitio a sitio y enrutamiento BGP) habilitadas para dos inquilinos, contoso y fabrikam.

```
$Contoso_RoutingDomain = "ContosoTenant"
$Fabrikam_RoutingDomain = "FabrikamTenant"

Install-RemoteAccess -MultiTenancy

Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru
```

## <a name="bgp-supported-deployment-topologies"></a><a name="bkmk_top"></a>Topologías de implementación compatibles con BGP
A continuación se enumeran las topologías de implementación compatibles, en las que los sitios de empresa se conectan a un centro de datos del proveedor de servicios de nube (CSP).

En todos los escenarios, la puerta de enlace de CSP es una puerta de enlace RAS de Windows Server 2016 en el perímetro. La puerta de enlace RAS, que es capaz de controlar varias conexiones de varios inquilinos, consta de un host de Hyper-V y una máquina virtual que realmente está configurada como puerta de enlace. Esta puerta de enlace de perímetro se configura con conexiones VPN de sitio a sitio como un enrutador BGP multiinquilino para intercambiar rutas de subred CSP y de empresa.

Los inquilinos se conectan a sus recursos en el centro de datos de CSP mediante una conexión VPN sitio a sitio (S2S). Además, se implementa el protocolo de enrutamiento de BGP para el intercambio de información de enrutamiento dinámico entre las puertas de enlace de empresa y CSP.

Se admiten las siguientes topologías de implementación.

-   [Puerta de enlace de sitio a sitio de VPN RAS con BGP en el perímetro del sitio de empresa](#bkmk_top1)

-   [Puerta de enlace de terceros con BGP en el perímetro del sitio de empresa](#bkmk_top2)

-   [Varios sitios de empresa con puertas de enlace de terceros](#bkmk_top3)

-   [Puntos de terminación independientes para BGP y VPN](#bkmk_top4)

Las secciones siguientes contienen información adicional sobre cada topología BGP compatible.

### <a name="ras-vpn-site-to-site-gateway-with-bgp-at-enterprise-site-edge"></a><a name="bkmk_top1"></a>Puerta de enlace de sitio a sitio de VPN RAS con BGP en el perímetro del sitio de empresa
Esta topología muestra un sitio de empresa conectado a un CSP. La topología de enrutamiento de empresa incluye un enrutador interno, una puerta de enlace RAS de Windows Server 2016 configurada para las conexiones VPN de sitio a sitio con el CSP y un dispositivo de firewall perimetral. La puerta de enlace RAS finaliza las conexiones VPN S2S y BGP.

![Puerta de enlace de sitio a sitio de VPN RAS](../../media/Border-Gateway-Protocol-BGP/bgp_01.jpg)

Ambos sitios están conectados mediante el protocolo de puerta de enlace de perímetro externo (eBGP), que puede transmitir información entre enrutadores compatibles con BGP en sistemas independientes autónomos (AS). Esto requiere que la empresa y CSP dispongan de distintos números de sistema autónomos (ASN), que es un parámetro que está integrado en el protocolo BGP.

En este escenario, BGP funciona de la manera siguiente.

-   El dispositivo perimetral de sitio de empresa aprende las rutas de subred virtualizadas (10.2.1.0/24) hospedadas en la nube mediante el uso de BGP. Este dispositivo también anuncia las rutas de subred locales (10.1.1.0/24) a la puerta de enlace multiinquilino de CSP RAS.

-   El enrutador perimetral de cliente aprende las rutas internas locales a través de uno de los siguientes mecanismos:

    -   El dispositivo perimetral ejecuta BGP con un enrutador interno y aprende rutas internas (en este ejemplo, 10.1.1.0/24). Mientras tanto, el enrutador interno aprende rutas externas (por ejemplo, 10.2.1.0/24) del dispositivo perimetral y el enrutador interno debe distribuir estas rutas a otros enrutadores locales mediante un protocolo de puerta de enlace interior (IGP) como Abrir primero la ruta de acceso más corta (OSPF) o Protocolo de información de enrutamiento (RIP).

    -   El dispositivo perimetral puede configurarse con rutas o interfaces estáticas para seleccionar rutas para anunciarlas mediante BGP. El dispositivo perimetral también distribuye las rutas de acceso externas a otros enrutadores locales mediante un IGP.

### <a name="third-party-gateway-with-bgp-at-enterprise-site-edge"></a><a name="bkmk_top2"></a>Puerta de enlace de terceros con BGP en el perímetro del sitio de empresa
Esta topología describe un sitio de empresa mediante un enrutador perimetral de terceros para conectarse a un CSP. El enrutador perimetral también actúa como una puerta de enlace VPN de sitio a sitio.

![Puerta de enlace de terceros con BGP en el perímetro del sitio de empresa](../../media/Border-Gateway-Protocol-BGP/bgp_02.jpg)

El enrutador perimetral de empresa aprende las rutas internas locales a través de uno de los siguientes mecanismos:

-   El dispositivo perimetral ejecuta BGP con un enrutador interno y aprende rutas internas (en este caso, 10.1.1.0/24).

-   El dispositivo perimetral implementa un protocolo de puerta de enlace interior (IGP) y participa directamente en el enrutamiento interno.

### <a name="multiple-enterprise-sites-connecting-to-csp-cloud-datacenter"></a><a name="bkmk_top3"></a>Varios sitios de empresa que se conectan al centro de recursos de nube de CSP
Esta topología muestra varios sitios de empresa que usan las puertas de enlace de terceros para conectarse a un CSP. Los dispositivos perimetrales de terceros actúan como puertas de enlace VPN de sitio a sitio y como enrutadores BGP.

![Varios sitios de empresa que se conectan al centro de recursos de nube de CSP](../../media/Border-Gateway-Protocol-BGP/bgp_03.jpg)

Los enrutadores perimetrales de cliente aprenden las rutas de acceso internas locales a través de uno de los siguientes mecanismos:

-   El dispositivo perimetral ejecuta BGP con un enrutador interno y aprende rutas internas (en este caso, 10.1.1.0/24).

-   El dispositivo perimetral implementa un protocolo de puerta de enlace interior (IGP) y participa directamente en el enrutamiento interno.

Cada sitio de empresa aprende las rutas del otro sitio sobre la conectividad eBGP directa.

Cada sitio de empresa aprende las rutas de red hospedadas directamente y mediante el uso del otro sitio de empresa, pero selecciona la mejor ruta según el costo de la ruta.

Si el enrutador BGP del sitio de empresa 1 no se puede conectar con el enrutador BGP del sitio de empresa 2 porque se ha producido un error de conectividad, el enrutador BGP de sitio 1 comienza de forma dinámica a conocer las rutas a la red del sitio 2 de la empresa desde el enrutador BGP de Windows Server en el CSP.

### <a name="separate-termination-points-for-bgp-and-vpn"></a><a name="bkmk_top4"></a>Puntos de terminación independientes para BGP y VPN
Esta topología muestra una empresa que usa dos enrutadores diferentes como extremos BGP y VPN de sitio a sitio. La VPN de sitio a sitio se termina en la puerta de enlace RAS de Windows Server 2016, mientras que BGP se termina en un enrutador interno. En el lado de CSP de las conexiones, el CSP termina las conexiones VPN y BGP con la puerta de enlace de RAS. Con esta configuración, el hardware del enrutador interno de terceros debe admitir la redistribución de rutas IGP a BGP, así como la redistribución de rutas BGP a IGP.

![Puntos de terminación independientes para BGP y VPN](../../media/Border-Gateway-Protocol-BGP/bgp_04.jpg)

El enrutador interno aprende las rutas de empresa a través de uno de los siguientes mecanismos:

-   BGP

-   Un protocolo de puerta de enlace interior (IGP) como OSPF o RIP.

-   Configuración de ruta estática

Cuando se usa cualquier IGP en el sitio de la empresa, el enrutador interno debe redistribuir rutas IGP a BGP (así como redistribuir las rutas BGP a rutas IGP) para mantener la conectividad de subred entre redes virtuales CSP y subredes locales de empresa.

Con esta implementación, la puerta de enlace RAS de la empresa tiene una conexión VPN de sitio a sitio con la puerta de enlace RAS de CSP, que proporciona la puerta de enlace RAS de la empresa con las rutas a la puerta de enlace CSP. A continuación, el enrutador interno de la empresa aprende esta ruta a la puerta de enlace de CSP mediante iBGP con la puerta de enlace RAS de la empresa. Por este motivo, el enrutador interno de la empresa puede establecer una sesión de emparejamiento con el enrutador BGP de puerta de enlace RAS de CSP.

A partir de este punto, el enrutador interno de empresa y la puerta de enlace de CSP RAS intercambian información de enrutamiento. Y el enrutador BGP de RAS de la empresa aprende las rutas de CSP y las rutas de empresa para enrutar paquetes entre las redes físicamente.

## <a name="bgp-features"></a><a name="bkmk_features"></a>Características de BGP
A continuación se muestran las características del enrutador BGP de puerta de enlace de RAS.

**Enrutamiento de BGP como un servicio de rol de acceso remoto**. Ahora puede instalar el servicio de rol **enrutamiento** del rol de servidor acceso remoto sin necesidad de instalar el servicio de rol **servicio de acceso remoto (RAS)** cuando quiera usar el acceso remoto como un enrutador LAN BGP.  Esto reduce la superficie de memoria del enrutador BGP e instala solo los componentes necesarios para el enrutamiento BGP dinámico. El servicio de rol enrutamiento es útil cuando solo se requiere una máquina virtual de enrutador BGP y no se requiere el uso de DirectAccess o VPN. Además, el uso de acceso remoto como un enrutador LAN con BGP proporciona las ventajas de enrutamiento dinámico de BGP en la red interna.

**Estadísticas de BGP (contadores de mensajes, contadores de ruta)**. El enrutador BGP permite mostrar las estadísticas de mensajes y ruta si es necesario mediante el uso del comando de Windows PowerShell **Get-BgpStatistics**.

**Compatibilidad con enrutamiento de varias rutas de costo igual**. El enrutador BGP admite ECMP y puede tener más de una ruta de igual costo conectadas a la tabla y la pila de enrutamiento de BGP. La selección del enrutador BGP de la ruta para transmitir paquetes de datos es aleatoria con ECMP habilitado.

**Configuración de HoldTime**. El enrutador BGP es compatible con la configuración del valor HoldTimer según sus requisitos de red. Este temporizador se puede cambiar dinámicamente para dar cabida a la interoperabilidad con dispositivos de terceros o mantener un tiempo máximo específico para el tiempo de espera de sesión de emparejamiento BGP.

**Compatibilidad con BGP interno y externo**. El enrutador BGP es compatible con el emparejamiento BGP e iBGP. Para configurar ambos, debe asegurarse de que se hayan asignados los ASN adecuados a los enrutadores de BGP locales y remotos. Las cuatro topologías de implementación de BGP emplean el emparejamiento BGP, y la cuarta topología también usa el emparejamiento iBGP.

**Interoperabilidad con soluciones de terceros**. El enrutador BGP se basa en la última especificación de la versión 4 de BGP y se ha probado su interoperabilidad con la mayoría de los dispositivos de enrutamiento de BGP principales de terceros. Para obtener más información, consulte Solicitudes de comentarios (RFC) 4271, [Protocolo de puerta de enlace perimetral 4 (BGP-4)](https://tools.ietf.org/html/rfc4271).

**Compatibilidad de mismo nivel de transporte de IPv4 e IPv6**. El enrutador BGP es compatible con el emparejamiento IPv4 e IPv6. Sin embargo, debe configurar el identificador BGP como la dirección IPv4 del enrutador BGP. Para todas las topologías de implementación de enrutador BGP, se puede usar cualquiera de los dos tipos de emparejamiento (IPV4/IPv6).

**Aprendizaje de rutas de unidifusión IPv4 e IPv6 y capacidad de anuncio (información de disponibilidad de capa de red multiprotocolo [NLRI])**. Independientemente del transporte que use, el enrutador BGP puede intercambiar rutas IPv4 e IPv6 si otros enrutadores BGP anuncian la capacidad adecuada al establecer la sesión. Para configurar el enrutamiento de IPv6, debe habilitarse el parámetro IPv6Routing y una dirección IPv6 global local a nivel del enrutador.

**Emparejamiento de modo mixto y modo pasivo**. Puede configurar sesiones de emparejamiento BGP en modo mixto, en el que el enrutador BGP actúa como el modo de iniciador y de respuesta o pasivo, en el que el enrutador BGP no inicia el emparejamiento, pero responde a las solicitudes entrantes. El modo mixto es el valor predeterminado y se recomienda para el emparejamiento BGP. Esto es así a menos que desee usar el modo pasivo para fines de depuración o de diagnóstico. Para todas las topologías de implementación de enrutador BGP, es necesario que el emparejamiento de modo mixto habilite reinicios automáticos en caso de que se produzcan eventos de error.

**Capacidad de reescritura del atributo de ruta**. Puede agregar, modificar o eliminar los siguientes atributos de los anuncios de ruta de entrada y salida de enrutador BGP mediante las directivas de enrutamiento BGP de próximo salto, MED, Local-Pref y Community.

**Filtrado de rutas**. El enrutador BGP admite el filtrado de anuncios de ruta de entrada o salida en función de varios atributos de ruta como Prefix, ASN-Range, Community y Próximo salto.

**El cliente de ruta-reflector (RR) y RR**. El enrutador BGP puede actuar como un Route-Reflector y un cliente de RR. Esto resulta útil en topologías complejas en las que RR puede simplificar la red mediante la formación de clústeres de RR.

**Compatibilidad con Route-Refresh**. El enrutador BGP es compatible con Route-Refresh y anuncia esta capacidad en el emparejamiento de forma predeterminada. Es capaz de enviar un nuevo conjunto de actualizaciones de ruta cuando lo solicite un elemento del mismo nivel a través del mensaje de ruta de actualización, así como de enviar un Route-Refresh para actualizar su tabla de enrutamiento en los eventos, como los cambios de la Directiva de enrutamiento de un elemento del mismo nivel. Esto permite el escenario de cambiar o actualizar las directivas de enrutamiento BGP en Windows Server 2016 sin necesidad de reiniciar el emparejamiento.

**Compatibilidad con la configuración de ruta estática**. Puede configurar rutas estáticas o interfaces en el enrutador BGP mediante el comando de Windows PowerShell **Add-BgpCustomRoute**. Las rutas estáticas que configure pueden ser los prefijos o el nombre de las interfaces desde las que se deben elegir las rutas. Sin embargo, solamente se conectarán las rutas con próximos saltos que se puedan resolver en las tablas de enrutamiento de BGP y se anunciarán a los pares.

**Compatibilidad con enrutamiento del tránsito**. El enrutador BGP admite el enrutamiento de tránsito para conexiones de iBGP a iBGP, iBGP a eBGP conexiones y eBGP a eBGP conexiones.

**Estabilización ligera de rutas**. La estabilización ligera de rutas al enrutamiento BGP en Windows Server 2016 proporciona compatibilidad para la estabilización ligera de rutas. Por ejemplo, cuando una ruta se está anunciando y retirando constantemente, lo que hace que la tabla de enrutamiento sea inestable, puede configurar el enrutador BGP para asignar un peso de amortiguación a la ruta y supervisarla para las aletas, y suprimir o anular la supresión según sea necesario. Esto ayuda a mantener una tabla de enrutamiento estable y menos procesamiento por parte del enrutador BGP.

**Agregación de rutas**. La agregación de rutas al enrutador BGP le proporciona la capacidad de configurar rutas de agregado y reemplazar los anuncios de ruta más granulares con rutas de resumen o agregadas a los pares. Esto da como resultado un número menor de mensajes de anuncios de ruta transmitidos en la red.

> [!NOTE]
> En System Center, la puerta de enlace RAS se denomina puerta de enlace de Windows Server.




