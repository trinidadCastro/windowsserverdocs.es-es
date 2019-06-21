---
title: Puerta de enlace RAS
description: En este tema, que está pensado para profesionales de tecnología de información (TI), se proporciona información general sobre la puerta de enlace de RAS, incluidos los modos de implementación de puerta de enlace RAS y características en Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: pashort
author: shortpatti
ms.date: 05/23/2018
ms.openlocfilehash: d61dcdbb61449bd2af57b8e2c99ced6235c4deca
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281263"
---
# <a name="ras-gateway"></a>Puerta de enlace RAS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puerta de enlace RAS es un enrutador de software y la puerta de enlace que puede usar en modo de inquilino único o en modo multiempresa.  
  
- **Inquilino único** modo permite a las organizaciones de cualquier tamaño para implementar la puerta de enlace como un exterior, o la red privada virtual (VPN) de orientado a Internet perimetral y el servidor de DirectAccess. En el modo de un solo inquilino, puede implementar la puerta de enlace de RAS en un servidor físico o máquina virtual (VM) que ejecuta Windows Server 2016.  
  
- **Modo multiempresa** permite a las empresas y proveedores de servicios en la nube (CSP) utilice puerta de enlace RAS para habilitar el centro de datos y en la nube enrutamiento de tráfico de red entre redes físicas y virtuales, incluido Internet. Para el modo para varios inquilinos, se recomienda que implemente la puerta de enlace de RAS en máquinas virtuales que ejecutan Windows Server 2016.  
  
> [!NOTE]  
> Puerta de enlace RAS es compatible con IPv4 e IPv6, incluyendo el reenvío de IPv4 e IPv6. Al configurar la puerta de enlace de RAS con traducción de direcciones de red (NAT), únicamente se admite NAT44.  
  
## <a name="who-will-be-interested-in-ras-gateway"></a>¿Quién puede interesarle en puerta de enlace RAS?
  
Si es un administrador del sistema, arquitecto de redes u otro profesional de TI, puerta de enlace RAS podría interesarle bajo una o varias de las siguientes circunstancias:  
  
-   Diseña o proporciona soporte técnico a la infraestructura de TI de una organización que está utilizando o planeando la utilización de Hyper-V para implementar máquinas virtuales (VM) en redes virtuales.  
  
-   Diseña o proporciona soporte técnico a la infraestructura de red de una organización que ha implementado o planea implementar tecnologías de nube.  
  
-   Desea proporcionar conectividad de red completa entre redes físicas y redes virtuales.  
  
-   Desea proporcionar a los clientes de su organización acceso a sus redes virtuales a través de Internet.  
  
-   Desea proporcionar a los empleados de su organización con acceso remoto a la red de su organización.  
  
-   Desea conectar oficinas en diferentes ubicaciones físicas a través de Internet.  
 
En este tema, que está pensado para profesionales de tecnología de información (TI), se proporciona información general sobre la puerta de enlace de RAS, incluidas las características y los modos de implementación de puerta de enlace RAS. 
  
En este tema se incluyen las siguientes secciones.  
  
  
-   [Modos de implementación de puerta de enlace RAS](#bkmk_modes)  
  
-   [Agrupación en clústeres de puerta de enlace RAS para alta disponibilidad](#bkmk_clustering)  
  
-   [Características de la puerta de enlace RAS](#bkmk_features)  
  
-   [Escenarios de implementación de puerta de enlace RAS](#bkmk_deploy)  
  
-   [Herramientas de administración de puerta de enlace RAS](#bkmk_manage)  
  


  
## <a name="bkmk_modes"></a>Modos de implementación de puerta de enlace RAS  
Puerta de enlace RAS incluye los siguientes modos de implementación.  
  
### <a name="single-tenant-mode"></a>Modo de un solo inquilino  
Para la mayoría de las organizaciones mediante la puerta de enlace de RAS en modo de un solo inquilino es la configuración típica. En modo de un solo inquilino, puede implementar la puerta de enlace de RAS como un servidor perimetral de VPN, un servidor de DirectAccess perimetral o ambos simultáneamente. En esta configuración, puerta de enlace RAS proporciona a los empleados remotos con conectividad a la red a través de conexiones de VPN o DirectAccess. Además, el modo de un solo inquilino permite conectar oficinas en diferentes ubicaciones físicas a través de Internet.  
  
### <a name="multitenant-mode"></a>Modo multiempresa  
Si su organización es un CSP o una empresa con varios inquilinos, puede implementar la puerta de enlace de RAS en el modo multiempresa para proporcionar enrutamiento de tráfico de red hacia y desde redes físicas y virtuales.  
  
La arquitectura multiempresa es la capacidad de una infraestructura de nube para admitir las cargas de trabajo de máquina virtual de varios inquilinos, aún aislarlas entre sí, mientras que todas las cargas de trabajo se ejecutan en la misma infraestructura. Las distintas cargas de trabajo de un inquilino individual pueden interconectarse y administrarse de manera remota, pero estos sistemas no se interconectan con las cargas de trabajo de los demás inquilinos, ni tampoco pueden los demás inquilinos administrarlas de manera remota.  
  
Por ejemplo, una empresa puede tener muchas subredes virtuales distintas, cada una de ellas dedicada a prestar servicio a un departamento específico, como Investigación y desarrollo o Contabilidad. En otro ejemplo, un CSP tiene muchos inquilinos con subredes virtuales aisladas en el mismo centro de datos físico. En ambos casos, la puerta de enlace de RAS puede enrutar el tráfico hacia y desde cada inquilino manteniendo el aislamiento diseñado de cada inquilino. Esta funcionalidad permite que la puerta de enlace de RAS que multiempresa.  
  
Las redes virtuales se crean con virtualización de red de Hyper-V, que es una tecnología que se introdujo en Windows Server 2012 y se ha mejorado en Windows Server 2016. Puerta de enlace de RAS se integra con virtualización de red de Hyper-V y es capaz de enrutar el tráfico de red eficaz en circunstancias donde hay muchos clientes diferentes, o inquilinos - que han aislado virtual networks en el mismo centro de datos.  
  
Virtualización de red de Hyper-V proporciona la capacidad de implementar una red de máquina virtual (VM) que es independiente de la red física subyacente. Con las redes de máquina virtual, que se componen de uno o más subredes virtuales, la ubicación física exacta de una subred IP está desacoplada de la topología de red virtual. Como resultado, puede mover fácilmente sus subredes locales en a la nube, mientras conserva su dirección IP existente direcciones y la topología en la nube. Esta capacidad de preservar la infraestructura permite a los servicios existentes seguir trabajando, sin tener en cuenta la ubicación física de las subredes. Es decir, Virtualización de red de Hyper-V hace posible una nube híbrida perfecta.  
  
> [!NOTE]  
> Virtualización de red de Hyper-V es una tecnología de superposición de red con Network Virtualization encapsulación de enrutamiento genérico ([NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00)), que permite a los inquilinos Traer su propio espacio de direcciones y permite a los CSP una mejor escalabilidad que es posible mediante el uso de redes VLAN para el aislamiento de inquilinos.  
  
En Windows Server 2016, puerta de enlace RAS enruta el tráfico de red entre la red física y los recursos de red de máquina virtual, independientemente de donde se encuentran los recursos. Puede usar la puerta de enlace RAS para enrutar el tráfico de red entre redes físicas y virtuales en la misma ubicación física o en muchas ubicaciones físicas distintas.  
  
Por ejemplo, si tiene una red física y una red virtual en la misma ubicación física, puede implementar un equipo que ejecuta Hyper-V que se configura con una máquina virtual de puerta de enlace RAS para actuar como una puerta de enlace de reenvío y enrutar el tráfico entre el virtual y física redes.  
  
En otro ejemplo, si sus redes virtuales residen en la nube, su CSP puede implementar una puerta de enlace RAS para que pueda crear una conexión de sitio a sitio de red privada virtual (VPN) entre el servidor VPN y la puerta de enlace de RAS del CSP; Cuando se ha establecido este vínculo puede conectarse a los recursos virtuales en la nube a través de la conexión VPN.  
  
Para obtener más información, consulte [alta disponibilidad de puerta de enlace de RAS](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_clustering"></a>Agrupación en clústeres de puerta de enlace RAS para alta disponibilidad  
Puerta de enlace de RAS se implementa en un equipo dedicado que ejecuta Hyper-V y que está configurado con una máquina virtual. La máquina virtual, a continuación, se configura como una puerta de enlace de RAS.  
  
Para lograr alta disponibilidad de recursos de red, puede implementar la puerta de enlace de RAS con conmutación por error mediante el uso de dos servidores de host físico que ejecuta Hyper-V que cada también estén ejecutando una máquina virtual (VM) que está configurada como una puerta de enlace. Las VM de la puerta de enlace están configuradas, por lo tanto, como clúster para proporcionar protección de conmutación por error frente a las interrupciones de red y los errores de hardware.  
  
Por ejemplo, si su organización es una empresa con una implementación de nube privada, necesitar sólo dos máquinas virtuales de puerta de enlace de RAS, cada uno de los cuales se instala en un equipo diferente que ejecuta Hyper-V. En este escenario, las máquinas virtuales de puerta de enlace de RAS se agregan a un clúster para proporcionar una alta disponibilidad.  
  
En otro ejemplo, si su organización es un proveedor de servicios de nube (CSP) con doscientos inquilinos en su centro de datos, puede usar ocho máquinas virtuales de puerta de enlace de RAS, con cada par de en clúster de máquinas virtuales de puerta de enlace de RAS que proporciona servicios de enrutamiento para cincuenta inquilinos. En este escenario, dos equipos que ejecutan Hyper-V cada tienen cuatro máquinas virtuales que se configuran como puertas de enlace de RAS. A continuación, configurar cuatro clústeres de máquina virtual de puerta de enlace de RAS, cada clúster que contiene una máquina virtual de cada equipo que ejecuta Hyper-V.  
  
Al implementar la puerta de enlace RAS, los servidores de host que ejecuta Hyper-V y las máquinas virtuales que se configuran como puertas de enlace deben ejecutar Windows Server 2012 R2 o Windows Server 2016.  
  
## <a name="bkmk_features"></a>Características de la puerta de enlace RAS  
Puerta de enlace RAS incluye las siguientes capacidades.  
  
-   **VPN de sitio a sitio**. Esta característica de puerta de enlace RAS permite conectar dos redes en diferentes ubicaciones físicas a través de Internet mediante una conexión VPN de sitio a sitio. Si tiene una oficina central y varias sucursales, puede implementar una puerta de enlace de RAS en cada ubicación de edge y crear conexiones de sitio a sitio para proporcionar el flujo de tráfico de red entre las ubicaciones. Para los CSP que hospedan a muchos inquilinos en su centro de datos, la puerta de enlace de RAS proporciona una solución de puerta de enlace multiempresa que permite a los inquilinos acceder y administrar sus recursos a través de conexiones de VPN de sitio a sitio desde sitios remotos y permite el flujo de tráfico de red entre recursos virtuales en su centro de datos y la red física.  
  
-   **Point-to-site VPN**. Esta característica de puerta de enlace RAS permite que los empleados de la organización o administradores para conectarse a la red de su organización desde ubicaciones remotas. Para las implementaciones de un solo inquilino de puerta de enlace de RAS, los empleados remotos pueden conectarse a la red de su organización mediante una conexión VPN. Esta conexión permite que usen recursos de red interna, como sitios web de intranet y servidores de archivos. Para las implementaciones de varios inquilinos, los administradores de red de inquilinos pueden usar conexiones VPN de punto a sitio para acceder a recursos de red virtual en el centro de datos CSP.  
  
-   **Enrutamiento dinámico con el protocolo de puerta de enlace de borde (BGP)** . BGP reduce la necesidad de configuración de enrutamiento manual en los enrutadores, ya que es un protocolo de enrutamiento dinámico y aprende automáticamente las rutas entre sitios que están conectados mediante conexiones VPN de sitio a sitio. Si su organización tiene varios sitios que están conectados mediante enrutadores BGP habilitado como puerta de enlace RAS, BGP permite que los enrutadores calcular automáticamente y usar rutas válidas entre sí en caso de interrupciones de red o un error. Para obtener más información, consulte [4271 RFC](https://tools.ietf.org/html/rfc4271).  
  
-   **Traducción de direcciones (NAT) de red**. Traducción de direcciones de red (NAT) le permite compartir una conexión a Internet a través de una única interfaz con una única dirección IP pública. Los equipos de la red privada usan direcciones privadas no enrutables. NAT asigna las direcciones privadas a la dirección pública. Esta característica de puerta de enlace RAS permite que los empleados de la organización con las implementaciones de un solo inquilino tener acceso a recursos de Internet desde detrás de la puerta de enlace. Para CSP, esta característica permite que las aplicaciones que se ejecutan en máquinas virtuales para tener acceso a Internet de inquilino. Por ejemplo, un máquina virtual que está configurado como un servidor Web del inquilino puede ponerse en contacto con los recursos financieros externos para procesar las transacciones de tarjeta de crédito.  

  
## <a name="bkmk_deploy"></a>Escenarios de implementación de puerta de enlace RAS  
Estos son los escenarios de implementación recomendada para la puerta de enlace RAS.  
  
-   **Borde de Enterprise - implementación de un solo inquilino**. Con el inquilino único implementación empresarial, puede conectar uno físico a varias otras ubicaciones físicas a través de Internet mediante el uso de la VPN de sitio a sitio - a y Border Gateway Protocol (BGP) permite usar el enrutamiento dinámico. También puede proporcionar acceso a los empleados remotos a la red de su organización con las conexiones VPN de punto a sitio y las conexiones de DirectAccess. (Las conexiones de DirectAccess están siempre disponibles y también proporcionan la ventaja de que puede administrar fácilmente los equipos que están conectados con DirectAccess, ya que están conectadas siempre que están en y conectado a Internet). También puede configurar las puertas de enlace de RAS de inquilino único empresarial con NAT, para que los equipos de la Intranet pueden comunicarse fácilmente con Internet.  
  
-   **En la nube de extremo del proveedor de servicio - implementación multiempresa**. Implementación de varios inquilinos de puerta de enlace RAS para CSP permite ofrecer a los inquilinos todas las características que están disponibles con la implementación de un solo inquilino de perímetro de empresa. Conexiones VPN de sitio a sitio entre redes virtuales de inquilinos en su centro de datos y las ubicaciones de red de inquilino a través de la media de Internet que los inquilinos tienen acceso ininterrumpido a sus recursos de nube todo el tiempo. Acceso VPN de punto a sitio para inquilinos significa que los administradores de inquilinos siempre pueden conectarse a sus redes virtuales en su centro de datos para administrar sus recursos. BGP proporciona enrutamiento dinámico y mantiene los inquilinos conectados a sus activos, incluso cuando se producen problemas de red en Internet o en otro lugar. Y NAT permite el inquilino de las máquinas virtuales para conectarse a los recursos de Internet, como los recursos de procesamiento de tarjetas de crédito.  
  
## <a name="bkmk_manage"></a>Herramientas de administración de puerta de enlace RAS  
A continuación es las herramientas de administración de puerta de enlace de RAS.  
  
-   En Windows Server 2016, para implementar un enrutador de puerta de enlace RAS, debe usar los comandos de Windows PowerShell. Para obtener más información, consulte [Cmdlets de acceso remoto](https://docs.microsoft.com/powershell/module/remoteaccess) para Windows Server 2016 y Windows 10.  
  
-   En System Center 2012 R2 Virtual Machine Manager (VMM), la puerta de enlace de RAS se denomina puerta de enlace de Windows Server. Un conjunto limitado de opciones de configuración de protocolo de puerta de enlace de borde (BGP) están disponibles en la interfaz de software VMM, incluyendo **dirección IP BGP Local** y **números de sistema autónomo (ASN)** ,  **Lista de direcciones IP de BGP del mismo nivel**, y **valores ASN**. Puede, no obstante, utilizar los comandos de Windows PowerShell BGP de acceso remoto para configurar el resto de características de la puerta de enlace de Windows Server. Para obtener más información, consulte [de Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/vmm) y [Cmdlets de acceso remoto](https://technet.microsoft.com/library/hh918399.aspx) para Windows Server 2016 y Windows 10.  
  
## <a name="related-topics"></a>Temas relacionados
- [Alta disponibilidad de puerta de enlace RAS](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
- [Tunelización de GRE en Windows Server 2016](gre-tunneling-windows-server.md)
- [Rendimiento de túnel GRE de puerta de enlace de RAS](RAS-Gateway-GRE-Perf.md)
