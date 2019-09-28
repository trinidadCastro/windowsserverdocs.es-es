---
title: Tunelización de GRE en Windows Server 2016
description: Puede usar este tema para conocer las actualizaciones de la funcionalidad de túnel de encapsulación de enrutamiento genérico (GRE) para la puerta de enlace de RAS en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: be57bc0ce1b509c49f269618765c79f380fd3b12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404674"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Tunelización de GRE en Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Windows Server 2016 proporciona actualizaciones a la encapsulación de enrutamiento genérico \(GRE @ no__t-1 para la puerta de enlace de RAS.  
  
GRE es un protocolo de túnel ligero que puede encapsular una amplia variedad de protocolos de capa de red dentro de los vínculos de punto a punto virtuales en una conexión entre redes de protocolo de Internet. La implementación de Microsoft GRE puede encapsular IPv4 e IPv6.  
  
Los túneles GRE son útiles en muchos escenarios porque:  
  
-   Son compatibles con el estándar RFC 2890, lo que permite interoperar con varios dispositivos del proveedor.  
  
-   Puede usar Protocolo de puerta de enlace de borde \(BGP @ no__t-1 para el enrutamiento dinámico  
  
-   Puede configurar puertas de enlace RAS multiempresa GRE para su uso con redes definidas por software \(SDN @ no__t-1
  
-   Puede usar System Center Virtual Machine Manager para administrar las puertas de enlace de RAS de GRE @ no__t-0based.
  
-   Puede conseguir un rendimiento de hasta 2,0 Gbps en una máquina virtual de 6 núcleos configurada como una puerta de enlace RAS GRE
  
-   Una sola puerta de enlace admite varios modos de conexión  
  
Los túneles basados en GRE habilitan la conectividad entre redes virtuales de inquilinos y redes externas. Dado que el protocolo GRE es ligero y la compatibilidad con GRE está disponible en la mayoría de los dispositivos de red, se convierte en una opción ideal para el túnel en el que no es necesario el cifrado de datos. 

La compatibilidad con GRE en túneles de sitio a sitio (S2S) soluciona el problema de reenvío entre redes virtuales de inquilinos y redes externas de inquilinos mediante una puerta de enlace de varios inquilinos, como se describe más adelante en este tema.  
  
La característica de túnel GRE está diseñada para cumplir los siguientes requisitos:  
  
-   Un proveedor de hospedaje debe ser capaz de crear redes virtuales para el reenvío sin modificar la configuración del conmutador físico.  
  
-   Un proveedor de hospedaje debe ser capaz de agregar subredes a sus redes externas sin modificar la configuración de los conmutadores físicos dentro de su infraestructura.  
La característica de túnel GRE habilita o mejora varios escenarios clave para hospedar proveedores de servicios mediante tecnologías de Microsoft para implementar redes definidas por software en sus ofertas de servicio.  
  
Estos son algunos escenarios de ejemplo:  
  
-   [Acceso desde redes virtuales de inquilino a redes físicas de inquilino](#BKMK_Access)  
  
-   [Conectividad de alta velocidad](#BKMK_Speed)  
  
-   [Integración con aislamiento basado en VLAN](#BKMK_Integration)  
  
-   [Acceder a recursos compartidos](#BKMK_Shared)  
  
-   [Servicios de dispositivos de terceros a los inquilinos](#BKMK_thirdparty)  
  
## <a name="key-scenarios"></a>Escenarios clave

Los siguientes son escenarios clave a los que se dirige la característica de túnel GRE.  
  
### <a name="BKMK_Access"></a>Acceso desde redes virtuales de inquilino a redes físicas de inquilino

Este escenario permite una manera escalable de proporcionar acceso desde las redes virtuales de inquilino a las redes físicas de inquilino ubicadas en las instalaciones del proveedor del servicio de hospedaje. Se establece un punto de conexión de túnel GRE en la puerta de enlace para varios inquilinos, el otro extremo del túnel GRE se establece en un dispositivo de otro fabricante en la red física. El tráfico de nivel 3 se enruta entre las máquinas virtuales de la red virtual y el dispositivo de terceros en la red física.  
  
![Túnel GRE que conecta la red física y la red virtual de inquilino](../../media/gre-tunneling-in-windows-server/GRE_.png)  
  
### <a name="BKMK_Speed"></a>Conectividad de alta velocidad

Este escenario permite una manera escalable de proporcionar conectividad de alta velocidad desde la red local del inquilino a su red virtual ubicada en la red del proveedor de servicios de hosting. Un inquilino se conecta a la red del proveedor de servicios a través de conmutación de etiquetas multiprotocolo (MPLS), donde se establece un túnel GRE entre el enrutador perimetral del proveedor de servicios de hospedaje y la puerta de enlace multiinquilino a la red virtual del inquilino.  
  
![Túnel GRE conectando la red de inquilinos de empresa MPLS y la red virtual de inquilino](../../media/gre-tunneling-in-windows-server/GRE-.png)  
  
### <a name="BKMK_Integration"></a>Integración con aislamiento basado en VLAN

Este escenario le permite integrar el aislamiento basado en VLAN con virtualización de red de Hyper-V. Una red física de la red del proveedor de hospedaje contiene un equilibrador de carga que usa el aislamiento basado en VLAN. Una puerta de enlace para varios inquilinos establece túneles GRE entre el equilibrador de carga de la red física y la puerta de enlace multiinquilino en la red virtual.  
  
Se pueden establecer varios túneles entre el origen y el destino, y la clave GRE se usa para discriminar entre los túneles.  
  
![Varios túneles GRE que conectan redes virtuales de inquilinos](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)  
  
### <a name="BKMK_Shared"></a>Acceder a recursos compartidos

Este escenario permite tener acceso a recursos compartidos en una red física ubicada en la red del proveedor de hospedaje.  
  
Es posible que tenga un servicio compartido ubicado en un servidor de una red física ubicada en la red del proveedor de hospedaje que desea compartir con varias redes virtuales de inquilinos.  
  
Las redes de inquilinos con subredes no superpuestas acceden a la red común a través de un túnel GRE. Una puerta de enlace de un solo inquilino enruta entre los túneles GRE, por lo que enruta los paquetes a las redes de inquilinos adecuadas.  
  
En este escenario, la puerta de enlace de un solo inquilino se puede reemplazar por dispositivos de hardware de terceros.  
  
![Una puerta de enlace de un solo inquilino que usa varios túneles para conectar varias redes virtuales](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)  
  
### <a name="BKMK_thirdparty"></a>Servicios de dispositivos de terceros a los inquilinos

Este escenario se puede usar para integrar dispositivos de terceros (por ejemplo, equilibradores de carga de hardware) en el flujo de tráfico de la red virtual del inquilino. Por ejemplo, el tráfico procedente de un sitio de empresa pasa a través de un túnel S2S a la puerta de enlace multiinquilino. El tráfico se enruta al equilibrador de carga a través de un túnel GRE. El equilibrador de carga enruta el tráfico a varias máquinas virtuales de la red virtual de la empresa. Lo mismo ocurre para otro inquilino con direcciones IP potencialmente superpuestas en las redes virtuales. El tráfico de red está aislado en el equilibrador de carga mediante las VLAN y es aplicable a todos los dispositivos de nivel 3 compatibles con VLAN.  
  
![Varios túneles GRE que conectan redes virtuales a dispositivos de terceros](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)  
  
## <a name="configuration-and-deployment"></a>Configuración e implementación

Un túnel GRE se expone como un protocolo adicional dentro de una interfaz S2S. Se implementa de manera similar a un túnel S2S de IPSec descrito en el siguiente blog de redes: [VPN Gateway de sitio a sitio (S2S) multiempresa con Windows Server 2012 R2](https://blogs.technet.com/b/networking/archive/2013/09/29/multi-tenant-site-to-site-s2s-vpn-gateway-with-windows-server-2012-r2.aspx)  
  
Vea el tema siguiente para obtener un ejemplo en el que se implementan puertas de enlace, incluidas las puertas de enlace de túnel GRE:  
  
[Implementar una infraestructura de red definida por software mediante scripts](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
  
## <a name="more-information"></a>Más información

Para obtener más información sobre la implementación de puertas de enlace de S2S, vea los temas siguientes:  
  
-   [Puerta de enlace RAS](RAS-Gateway.md)  
  
-   [Protocolo de puerta de enlace de borde &#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)  
  
-   @no__t 0New! Guía de implementación de puerta de enlace multiinquilino de RAS de Windows Server 2012 R2 @ no__t-0  
  
-   [Implementación de Protocolo de puerta de enlace de borde (BGP) con la puerta de enlace multiinquilino de RAS](https://blogs.technet.com/b/wsnetdoc/archive/2014/04/03/deploy-border-gateway-protocol-bgp-with-the-RAS-multitenant-gateway.aspx)  
  


