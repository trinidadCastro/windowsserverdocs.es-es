---
title: Protocolo de puerta de enlace de borde (BGP)
description: Puede utilizar este tema para obtener una descripción de Border Gateway Protocol (BGP) en Windows Server 2016, incluidas las topologías de implementación admitidas por BGP y las características BGP y capacidades.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78cc2ce3-a48e-45db-b402-e480b493fab1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 655a7b02468db4246b85b495289806a3f9735a95
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282001"
---
# <a name="border-gateway-protocol-bgp"></a>Protocolo de puerta de enlace de borde (BGP)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener una descripción del Protocolo de puerta de enlace de borde (BGP), incluidas las topologías de implementación admitidas por BGP y las características y capacidades de BGP.  
  
> [!NOTE]  
> Además de este tema, está disponible la siguiente documentación de BGP.  
>   
> -   [Referencia de comandos de Windows PowerShell de BGP](../../remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
En este tema se incluyen las siguientes secciones.  
  
-   [BGP compatible con topologías de implementación](#bkmk_top)  
  
-   [Características de BGP](#bkmk_features)  
  
Cuando se configura en un servicio de acceso remoto de Windows Server 2016 \(RAS\) puerta de enlace en el modo multiempresa, Border Gateway Protocol (BGP) proporciona la capacidad para administrar el enrutamiento de tráfico de red entre redes de máquinas virtuales de los inquilinos y sus sitios remotos. También puede usar BGP para las implementaciones de puerta de enlace RAS de inquilino único, y al implementar acceso remoto como una red de área Local \(LAN\) enrutador.  
  
BGP reduce la necesidad de configuración de enrutamiento manual en los enrutadores, ya que es un protocolo de enrutamiento dinámico y aprende automáticamente las rutas entre sitios que están conectados mediante conexiones VPN de sitio a sitio.  
  
Para usar el enrutamiento de BGP, debe instalar la **servicio de acceso remoto \(RAS\)**  o **enrutamiento** servicio de rol del rol de servidor de acceso remoto en un equipo o máquina virtual \(VM\) -el tipo de sistema que use depende de si tiene una implementación multiempresa:  
  
-   Para una implementación de varios inquilinos, se recomienda que instale la puerta de enlace de RAS en una o más máquinas virtuales. Uso de varias máquinas virtuales proporciona alta disponibilidad. La puerta de enlace de RAS es capaz de controlar varias conexiones de varios inquilinos y consta de un host de Hyper-V y una máquina virtual que en realidad está configurada como puerta de enlace. Esta puerta de enlace se configura con conexiones VPN de sitio a sitio como un enrutador BGP multiinquilino para el inquilino de exchange y el proveedor de servicios de nube \(CSP\) las rutas de subred.  
  
-   Para una implementación de puerta de enlace de borde de inquilino único o una implementación de enrutador de LAN, puede instalar la puerta de enlace de RAS en un equipo físico o una máquina virtual.  
  
> [!IMPORTANT]  
> Al instalar una puerta de enlace de RAS, debe especificar si se habilita BGP para cada inquilino mediante el uso de la **Enable-RemoteAccessRoutingDomain** comando de Windows PowerShell con el **tipo** valor del parámetro de  **Todos los**. Para instalar acceso remoto como un enrutador LAN habilitada para BGP sin capacidades para varios inquilinos, puede usar el comando **Install-RemoteAccess - VpnType RoutingOnly**.  
>   
> Ejemplo de código siguiente muestra cómo instalar RAS en modo Multiinquilino con todas las características RAS (point-to-site VPN sitio a sitio VPN y BGP enrutamiento) habilitadas para dos inquilinos, Contoso y Fabrikam.  
  
```  
$Contoso_RoutingDomain = "ContosoTenant"  
$Fabrikam_RoutingDomain = "FabrikamTenant"  
  
Install-RemoteAccess -MultiTenancy  
  
Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru  
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru  
```  
  
## <a name="bkmk_top"></a>BGP compatible con topologías de implementación  
A continuación se enumeran las topologías de implementación compatibles, en las que los sitios de empresa se conectan a un centro de datos del proveedor de servicios de nube (CSP).  
  
En todos los escenarios, la puerta de enlace CSP es un enlace de RAS de Windows Server 2016 en el perímetro. La puerta de enlace de RAS, que es capaz de controlar varias conexiones de varios inquilinos, consta de un host de Hyper-V y una máquina virtual que en realidad está configurada como puerta de enlace. Esta puerta de enlace de perímetro se configura con conexiones VPN de sitio a sitio como un enrutador BGP multiinquilino para intercambiar rutas de subred CSP y de empresa.  
  
Los inquilinos se conectan a sus recursos en el centro de datos de CSP mediante una conexión VPN sitio a sitio (S2S). Además, se implementa el protocolo de enrutamiento de BGP para el intercambio de información de enrutamiento dinámico entre las puertas de enlace de empresa y CSP.  
  
Se admiten las siguientes topologías de implementación.  
  
-   [Puerta de enlace de RAS VPN sitio a sitio con BGP en el perímetro del sitio de empresa](#bkmk_top1)  
  
-   [Puerta de enlace de terceros con BGP en el perímetro del sitio de empresa](#bkmk_top2)  
  
-   [Varios sitios de empresa con puertas de enlace de terceros](#bkmk_top3)  
  
-   [Puntos de terminación independientes para BGP y VPN](#bkmk_top4)  
  
Las secciones siguientes contienen información adicional sobre cada topología BGP compatible.  
  
### <a name="bkmk_top1"></a>Puerta de enlace de RAS VPN sitio a sitio con BGP en el perímetro del sitio de empresa  
Esta topología muestra un sitio de empresa conectado a un CSP. La topología de enrutamiento Enterprise incluye un enrutador interno, configura una puerta de enlace de RAS de Windows Server 2016 para las conexiones de sitio a sitio VPN con el CSP y un dispositivo de firewall perimetral. La puerta de enlace de RAS termina las conexiones S2S VPN y BGP.  
  
![Puerta de enlace de RAS VPN sitio a sitio](../../media/Border-Gateway-Protocol-BGP/bgp_01.jpg)  
  
Ambos sitios están conectados mediante el protocolo de puerta de enlace de perímetro externo (eBGP), que puede transmitir información entre enrutadores compatibles con BGP en sistemas independientes autónomos (AS). Esto requiere que la empresa y CSP dispongan de distintos números de sistema autónomos (ASN), que es un parámetro que está integrado en el protocolo BGP.  
  
En este escenario, BGP funciona de la manera siguiente.  
  
-   El dispositivo perimetral de sitio de empresa aprende las rutas de subred virtualizadas (10.2.1.0/24) hospedadas en la nube mediante el uso de BGP. Este dispositivo también anuncia las rutas de subred locales (10.1.1.0/24) a la puerta de enlace Multiinquilino de CSP de RAS.  
  
-   El enrutador perimetral de cliente aprende las rutas internas locales a través de uno de los siguientes mecanismos:  
  
    -   El dispositivo perimetral ejecuta BGP con un enrutador interno y aprende rutas internas (en este ejemplo, 10.1.1.0/24). Mientras tanto, el enrutador interno aprende rutas externas (por ejemplo, 10.2.1.0/24) del dispositivo perimetral y el enrutador interno debe distribuir estas rutas a otros enrutadores locales mediante un protocolo de puerta de enlace interior (IGP) como Abrir primero la ruta de acceso más corta (OSPF) o Protocolo de información de enrutamiento (RIP).  
  
    -   El dispositivo perimetral puede configurarse con rutas o interfaces estáticas para seleccionar rutas para anunciarlas mediante BGP. El dispositivo perimetral también distribuye las rutas de acceso externas a otros enrutadores locales mediante un IGP.  
  
### <a name="bkmk_top2"></a>Puerta de enlace de terceros con BGP en el perímetro del sitio de empresa  
Esta topología describe un sitio de empresa mediante un enrutador perimetral de terceros para conectarse a un CSP. El enrutador perimetral también actúa como una puerta de enlace VPN de sitio a sitio.  
  
![Puerta de enlace de terceros con BGP en el perímetro del sitio de empresa](../../media/Border-Gateway-Protocol-BGP/bgp_02.jpg)  
  
El enrutador perimetral de empresa aprende las rutas internas locales a través de uno de los siguientes mecanismos:  
  
-   El dispositivo perimetral ejecuta BGP con un enrutador interno y aprende rutas internas (en este caso, 10.1.1.0/24).  
  
-   El dispositivo perimetral implementa un protocolo de puerta de enlace interior (IGP) y participa directamente en el enrutamiento interno.  
  
### <a name="bkmk_top3"></a>Centro de datos de varios sitios de empresa, conecta a CSP de nube  
Esta topología muestra varios sitios de empresa que usan las puertas de enlace de terceros para conectarse a un CSP. Los dispositivos perimetrales de terceros actúan como puertas de enlace VPN de sitio a sitio y como enrutadores BGP.  
  
![Centro de datos de varios sitios de empresa, conecta a CSP de nube](../../media/Border-Gateway-Protocol-BGP/bgp_03.jpg)  
  
Los enrutadores perimetrales de cliente aprenden las rutas de acceso internas locales a través de uno de los siguientes mecanismos:  
  
-   El dispositivo perimetral ejecuta BGP con un enrutador interno y aprende rutas internas (en este caso, 10.1.1.0/24).  
  
-   El dispositivo perimetral implementa un protocolo de puerta de enlace interior (IGP) y participa directamente en el enrutamiento interno.  
  
Cada sitio de empresa aprende las rutas del otro sitio sobre la conectividad eBGP directa.  
  
Cada sitio de empresa aprende las rutas de red hospedadas directamente y mediante el uso del otro sitio de empresa, pero selecciona la mejor ruta según el costo de la ruta.  
  
Si el enrutador BGP de sitio de empresa 1 no se puede conectar con el enrutador BGP de empresa sitio 2 porque se produjo un error de conectividad, el enrutador BGP de sitio 1 comienza dinámicamente obtener información sobre las rutas a la red empresarial sitio 2 desde el enrutador BGP de CSP y el tráfico es sin problemas redirigir desde el sitio 1 a 2 del sitio mediante el enrutador de BGP de Windows Server en el CSP.  
  
### <a name="bkmk_top4"></a>Puntos de terminación independientes para BGP y VPN  
Esta topología muestra una empresa que usa dos enrutadores diferentes como extremos BGP y VPN de sitio a sitio. VPN de sitio a sitio se termina en el enlace de RAS de Windows Server 2016, mientras se termina BGP en un enrutador interno. En el lado CSP de las conexiones, el CSP termina las conexiones VPN y BGP con la puerta de enlace de RAS. Con esta configuración, el hardware del enrutador interno de terceros debe admitir la redistribución de rutas IGP a BGP, así como la redistribución de rutas BGP a IGP.  
  
![Puntos de terminación independientes para BGP y VPN](../../media/Border-Gateway-Protocol-BGP/bgp_04.jpg)  
  
El enrutador interno aprende las rutas de empresa a través de uno de los siguientes mecanismos:  
  
-   BGP  
  
-   Un protocolo de puerta de enlace interior (IGP) como OSPF o RIP.  
  
-   Configuración de ruta estática  
  
Cuando se usa cualquier IGP en el sitio de la empresa, el enrutador interno debe redistribuir rutas IGP a BGP (así como redistribuir las rutas BGP a rutas IGP) para mantener la conectividad de subred entre redes virtuales CSP y subredes locales de empresa.  
  
Con esta implementación, la puerta de enlace de RAS de empresa tiene una conexión VPN de sitio a sitio con la puerta de enlace de RAS de CSP, que proporciona la puerta de enlace de RAS empresarial con las rutas a la puerta de enlace CSP. El enrutador interno de empresa, a continuación, aprende esta ruta a la puerta de enlace CSP mediante el uso de iBGP con la puerta de enlace de RAS de Enterprise. Por este motivo, el enrutador interno de empresa, a continuación, es capaz de establecer una sesión de emparejamiento con el enrutador de BGP de puerta de enlace de RAS de CSP.  
  
Desde este punto, el enrutador interno de empresa y la puerta de enlace de RAS de CSP intercambian información de enrutamiento. Y el enrutador BGP de RAS de empresa aprende las rutas CSP y las rutas de empresa para enrutar paquetes entre las redes que físicamente.  
  
## <a name="bkmk_features"></a>Características de BGP  
A continuación es las características del enrutador de BGP de puerta de enlace de RAS.  
  
**Enrutamiento de BGP como un servicio de rol de acceso remoto**. Ahora puede instalar el **enrutamiento** servicio de rol del rol de servidor de acceso remoto sin necesidad de instalar el **el servicio de acceso remoto (RAS)** servicio de rol cuando desea usar el acceso remoto como un enrutador LAN de BGP.  Esto reduce el consumo de memoria del enrutador BGP y sólo instala los componentes necesarios para el enrutamiento dinámico de BGP. El servicio de rol de enrutamiento es útil cuando solo se requiere una máquina virtual de enrutador de BGP y no requieren el uso de DirectAccess o VPN. Además, mediante el acceso remoto como un enrutador LAN con BGP le proporcionará las ventajas de enrutamientos dinámicas de BGP en su red interna.  
  
**Estadísticas de BGP (contadores de mensajes, contadores de ruta)** . El enrutador BGP permite mostrar las estadísticas de mensajes y ruta si es necesario mediante el uso del comando de Windows PowerShell **Get-BgpStatistics** .  
  
**Compatibilidad con enrutamiento de varias rutas de costo igual**. El enrutador BGP admite ECMP y puede tener más de una ruta de igual costo conectadas a la tabla y la pila de enrutamiento de BGP. La selección del enrutador BGP de la ruta para transmitir paquetes de datos es aleatoria con ECMP habilitado.  
  
**Configuración de HoldTime**. El enrutador BGP es compatible con la configuración del valor HoldTimer según sus requisitos de red. Este temporizador se puede cambiar dinámicamente para dar cabida a la interoperabilidad con dispositivos de terceros o mantener un tiempo máximo específico para el tiempo de espera de sesión de emparejamiento BGP.  
  
**Compatibilidad con BGP interno y externo**. El enrutador BGP es compatible con el emparejamiento BGP e iBGP. Para configurar ambos, debe asegurarse de que se hayan asignados los ASN adecuados a los enrutadores de BGP locales y remotos. Las cuatro topologías de implementación de BGP emplean el emparejamiento BGP, y la cuarta topología también usa el emparejamiento iBGP.  
  
**Interoperabilidad con soluciones de terceros**. El enrutador BGP se basa en la última especificación de la versión 4 de BGP y se ha probado su interoperabilidad con la mayoría de los dispositivos de enrutamiento de BGP principales de terceros. Para obtener más información, consulte Solicitudes de comentarios (RFC) 4271, [Protocolo de puerta de enlace perimetral 4 (BGP-4)](https://tools.ietf.org/html/rfc4271).  
  
**Compatibilidad de mismo nivel de transporte de IPv4 e IPv6**. El enrutador BGP es compatible con el emparejamiento IPv4 e IPv6. Sin embargo, debe configurar el identificador BGP como la dirección IPv4 del enrutador BGP. Para todas las topologías de implementación de enrutador BGP, se puede usar cualquiera de los dos tipos de emparejamiento (IPV4/IPv6).  
  
**Aprendizaje de rutas de unidifusión IPv4 e IPv6 y capacidad de anuncio (información de disponibilidad de capa de red multiprotocolo [NLRI])** . Independientemente del transporte que use, el enrutador BGP puede intercambiar rutas IPv4 e IPv6 si otros enrutadores BGP anuncian la capacidad adecuada al establecer la sesión. Para configurar el enrutamiento de IPv6, debe habilitarse el parámetro IPv6Routing y una dirección IPv6 global local a nivel del enrutador.  
  
**Emparejamiento de modo mixto y modo pasivo**. Puede configurar sesiones de emparejamiento BGP en modo mixto: donde el enrutador BGP actúa como iniciador y Respondedor - o modo pasivo, donde el enrutador BGP no inicia el emparejamiento, pero responder a las solicitudes entrantes. El modo mixto es el valor predeterminado y se recomienda para el emparejamiento BGP. Esto es así a menos que desee usar el modo pasivo para fines de depuración o de diagnóstico. Para todas las topologías de implementación de enrutador BGP, es necesario que el emparejamiento de modo mixto habilite reinicios automáticos en caso de que se produzcan eventos de error.  
  
**Capacidad de reescritura del atributo de ruta**. Puede agregar, modificar o eliminar los siguientes atributos de los anuncios de ruta de entrada y salida de enrutador BGP mediante las directivas de enrutamiento BGP de próximo salto, MED, Local-Pref y Community.  
  
**Filtrado de rutas**. El enrutador BGP admite el filtrado de anuncios de ruta de entrada o salida en función de varios atributos de ruta como Prefix, ASN-Range, Community y Próximo salto.  
  
**Cliente de ruta-reflector (RR) y RR**. El enrutador BGP puede actuar como ruta-Reflector y un cliente de RR. Esto es útil en topologías complejas donde RR puede simplificar la red mediante la formación de clústeres de RR.  
  
**Compatibilidad con Route-Refresh**. El enrutador BGP es compatible con Route-Refresh y anuncia esta capacidad en el emparejamiento de forma predeterminada. Es capaz de enviar un nuevo conjunto de actualizaciones de ruta cuando se solicita un elemento del mismo nivel a través de mensajes de la ruta de actualización, así como el envío de una actualización de ruta para actualizar su tabla de enrutamiento en los eventos, como los cambios de directiva de enrutamiento para un par. Esto habilita el escenario de cambiar o actualizar las directivas de enrutamiento de BGP en Windows Server 2016 sin necesidad de reiniciar el emparejamiento.  
  
**Compatibilidad con la configuración de ruta estática**. Puede configurar rutas estáticas o interfaces en el enrutador BGP mediante el comando de Windows PowerShell **Add-BgpCustomRoute**. Las rutas estáticas que configure pueden ser los prefijos o el nombre de las interfaces desde las que se deben elegir las rutas. Sin embargo, solamente se conectarán las rutas con próximos saltos que se puedan resolver en las tablas de enrutamiento de BGP y se anunciarán a los pares.  
  
**Compatibilidad con enrutamiento del tránsito**. El enrutador BGP admite el enrutamiento del tránsito de iBGP a las conexiones de iBGP, iBGP a eBGP y eBGP a eBGP.  
  
**Enrutar Flap amortiguamiento**. Amortiguamiento de Flap de ruta para el enrutamiento de BGP en Windows Server 2016 proporciona compatibilidad para amortiguar flap de ruta. Por ejemplo, cuando constantemente se publica una ruta y retiradas, hacer que la tabla de enrutamiento inestable, puede configurar el enrutador BGP para asignar un peso terminado a la ruta y supervisarlo para solapas - y en consecuencia suprimir o anular-suprimirla según sea necesario. Esto ayuda a mantener una tabla de enrutamiento estable y menos procesamiento del enrutador BGP.  
  
**Enrutar agregación**. Agregación de ruta para el enrutador BGP proporciona la capacidad para configurar las rutas agregadas y reemplazar los anuncios de rutas más granulares con las rutas de resumen o agregadas a elementos del mismo nivel. Esto da como resultado un menor número de mensajes de anuncio de ruta se transmite en la red.  
  
> [!NOTE]  
> En System Center, la puerta de enlace de RAS se denomina puerta de enlace de Windows Server.  
  

  

