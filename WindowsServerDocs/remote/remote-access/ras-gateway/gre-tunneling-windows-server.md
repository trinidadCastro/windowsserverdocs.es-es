---
title: Tunelización de GRE en Windows Server 2016
description: Puede utilizar este tema para obtener una descripción de las actualizaciones a la posibilidad de túnel de encapsulación de enrutamiento genérico (GRE) para la puerta de enlace de RAS en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0ec077ad5e97edd3db7d1dc4e662bb191f7885b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887866"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Tunelización de GRE en Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Windows Server 2016 ofrece actualizaciones para la encapsulación de enrutamiento genérico \(GRE\) capacidad de túnel para la puerta de enlace de RAS.  
  
GRE es un protocolo de túnel ligero que puede encapsular una amplia variedad de protocolos de capa de red dentro de los vínculos de punto a punto virtuales en una conexión entre redes de protocolo de Internet. Puede encapsular la implementación de Microsoft GRE IPv4 e IPv6.  
  
Túneles GRE son útiles en muchos escenarios porque:  
  
-   Son ligeras y 2890 de RFC compatibles, lo que puede interoperar con diversos dispositivos de proveedor  
  
-   Puede usar el protocolo de puerta de enlace de borde \(BGP\) para enrutamiento dinámico  
  
-   Puede configurar multiempresa GRE puertas de enlace RAS para su uso con redes definidas por Software \(SDN\)
  
-   Puede usar System Center Virtual Machine Manager para administrar GRE\-en función de las puertas de enlace de RAS
  
-   Puede lograr un rendimiento Gbps hasta 2.0 en una máquina virtual de 6 núcleos que se configura como una puerta de enlace de RAS de GRE
  
-   Una sola puerta de enlace admite varios modos de conexión  
  
Los túneles basados en GRE habilitan la conectividad entre redes virtuales de inquilinos y redes externas. Puesto que el protocolo GRE es ligero y compatibilidad con GRE se encuentra disponible en la mayoría de los dispositivos de red se convierte en una opción ideal para el túnel donde no se requiere el cifrado de datos. 

La compatibilidad con GRE en túneles de sitio a sitio (S2S) soluciona el problema de reenvío entre redes virtuales de inquilinos y redes externas de inquilinos mediante una puerta de enlace de varios inquilinos, como se describe más adelante en este tema.  
  
La característica de túnel GRE está diseñada para cumplir los siguientes requisitos:  
  
-   Un proveedor de hospedaje debe ser capaz de crear redes virtuales para el reenvío sin modificar la configuración del conmutador físico.  
  
-   Un proveedor de hospedaje debe poder agregar subredes a sus redes con orientación externa sin modificar la configuración de los conmutadores físicos dentro de su infraestructura.  
La característica de túnel GRE permite o mejora varios escenarios clave para hospedar los proveedores de servicios mediante las tecnologías de Microsoft para implementar redes definidas por Software en sus ofertas de servicio.  
  
Estos son algunos escenarios de ejemplo:  
  
-   [Acceso desde redes virtuales de inquilinos a las redes físicas de inquilinos](#BKMK_Access)  
  
-   [Conectividad de alta velocidad](#BKMK_Speed)  
  
-   [Integración con aislamiento de VLAN en función](#BKMK_Integration)  
  
-   [Recursos de acceso compartido](#BKMK_Shared)  
  
-   [Servicios de dispositivos de terceros a los inquilinos](#BKMK_thirdparty)  
  
## <a name="key-scenarios"></a>Escenarios clave

Los siguientes son escenarios clave que el GRE tunelizar las direcciones de la característica.  
  
### <a name="BKMK_Access"></a>Acceso desde redes virtuales de inquilinos a las redes físicas de inquilinos

Este escenario permite una manera escalable proporcionar acceso desde redes virtuales de inquilinos a las redes físicas ubicadas en el hospedaje instalaciones del proveedor de servicio del inquilino. Un extremo del túnel GRE se establece en la puerta de enlace para varios inquilinos, el otro extremo del túnel GRE se establece en un dispositivo de terceros en la red física. Se enruta el tráfico de nivel 3 entre las máquinas virtuales en la red virtual y el dispositivo de terceros en la red física.  
  
![Túnel GRE conexión de red física del proveedor de hospedaje y red virtual de inquilino](../../media/gre-tunneling-in-windows-server/GRE_.png)  
  
### <a name="BKMK_Speed"></a>Conectividad de alta velocidad

Este escenario permite una manera escalable proporcionar conectividad de alta velocidad de la red de inquilino en el entorno local a su red virtual ubicada en la red de proveedor de hospedaje de servicio. Un inquilino se conecta a la red del proveedor de servicio a través de conmutación (MPLS), donde se establece un túnel GRE entre el enrutador de borde del proveedor de servicio hospedaje y la puerta de enlace para varios inquilinos a red virtual del inquilino de etiquetas multiprotocolo.  
  
![Túnel GRE conexión de red de inquilino empresarial MPLS y red virtual de inquilino](../../media/gre-tunneling-in-windows-server/GRE-.png)  
  
### <a name="BKMK_Integration"></a>Integración con aislamiento de VLAN en función

Este escenario le permite integrar aislamiento basada en VLAN con virtualización de red de Hyper-V. Una red física en la red del proveedor de hospedaje contiene un equilibrador de carga mediante aislamiento de VLAN. Una puerta de enlace multiempresa establece túneles GRE entre el equilibrador de carga en la red física y la puerta de enlace para varios inquilinos en la red virtual.  
  
Se pueden establecer varios túneles entre el origen y destino, y la clave GRE se usa para diferenciar entre los túneles.  
  
![GRE varios túneles conectar redes virtuales de inquilino](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)  
  
### <a name="BKMK_Shared"></a>Recursos de acceso compartido

Este escenario le permite tener acceso a recursos compartidos en una red física que se encuentra en la red del proveedor de hospedaje.  
  
Puede tener un servicio compartido que se encuentra en un servidor en una red física que se encuentra en la red del proveedor de hospedaje que desea compartir con varias redes virtuales de inquilino.  
  
Las redes de inquilinos con subredes no superpuestas, acceso a la red común a través de un túnel GRE. Una puerta de enlace de inquilino único se distribuye entre los túneles GRE, enrutamiento, por tanto, los paquetes a las redes de inquilino correspondiente.  
  
En este escenario, la puerta de enlace de inquilino único puede reemplazarse mediante dispositivos de hardware de terceros.  
  
![Una puerta de enlace de inquilino único con varios túneles para conectar varias redes virtuales](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)  
  
### <a name="BKMK_thirdparty"></a>Servicios de dispositivos de terceros a los inquilinos

Este escenario se puede usar para integrar los dispositivos de terceros (por ejemplo, equilibradores de carga de hardware) en el flujo de tráfico de red virtual de inquilino. Por ejemplo, el tráfico procedente de un sitio de empresa se pasa a través de un túnel de S2S a la puerta de enlace para varios inquilinos. El tráfico se enruta al equilibrador de carga a través de un túnel GRE. El equilibrador de carga enruta el tráfico en varias máquinas virtuales de red virtual de la empresa. Lo mismo sucede para otro inquilino con posiblemente superpuestas direcciones IP en las redes virtuales. El tráfico de red está aislado en el equilibrador de carga usa redes VLAN y es aplicable a todos los dispositivos de capa 3 que admitan redes VLAN.  
  
![GRE varios túneles conectar redes virtuales a los dispositivos de terceros](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)  
  
## <a name="configuration-and-deployment"></a>Configuración e implementación

Un túnel GRE se expone como un protocolo adicional dentro de una interfaz de S2S. De forma similar se implementa como un túnel IPSec S2S, que se describe en el siguiente Blog de redes: [Puerta de enlace VPN de varios inquilinos sitio a sitio (S2S) con Windows Server 2012 R2](https://blogs.technet.com/b/networking/archive/2013/09/29/multi-tenant-site-to-site-s2s-vpn-gateway-with-windows-server-2012-r2.aspx)  
  
Vea el tema siguiente para obtener un ejemplo que implementa las puertas de enlace, incluidas las puertas de enlace de túnel GRE:  
  
[Implementar una infraestructura de red definida por Software con scripts](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
  
## <a name="more-information"></a>Más información

Para obtener más información sobre la implementación de las puertas de enlace S2S, vea los temas siguientes:  
  
-   [RAS Gateway](RAS-Gateway.md)  
  
-   [Protocolo de puerta de enlace de borde &#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)  
  
-   [¡Nuevo! Guía de implementación de la puerta de enlace multiempresa de RAS de Windows Server 2012 R2](https://blogs.technet.com/b/wsnetdoc/archive/2014/03/26/new-windows-server-2012-r2-RAS-multitenant-gateway-deployment-guide.aspx)  
  
-   [Implementar el protocolo de puerta de enlace de borde (BGP) con la puerta de enlace RAS para varios inquilinos](https://blogs.technet.com/b/wsnetdoc/archive/2014/04/03/deploy-border-gateway-protocol-bgp-with-the-RAS-multitenant-gateway.aspx)  
  


