---
title: Alta disponibilidad de la puerta de enlace de RAS
description: Puede usar este tema para obtener información sobre las configuraciones de alta disponibilidad para la puerta de enlace multiinquilino de RAS para redes definidas por software (SDN) en Windows Server 2016.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 34d826c9-65bc-401f-889d-cf84e12f0144
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 774f0f4299bb5acbe7314080dd1314f74574d16d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859618"
---
# <a name="ras-gateway-high-availability"></a>Alta disponibilidad de la puerta de enlace de RAS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre las configuraciones de alta disponibilidad para la puerta de enlace multiinquilino de RAS para redes definidas por software (SDN).  
  
Este tema contiene las siguientes secciones.  
  
-   [Introducción a la puerta de enlace RAS](#bkmk_overview)  
  
-   [Introducción a los grupos de puerta de enlace](#bkmk_pools)  
  
-   [Introducción a la implementación de puerta de enlace RAS](#bkmk_deployment)  
  
-   [Integración de puerta de enlace RAS con controladora de red](#bkmk_integration)  
  
## <a name="ras-gateway-overview"></a><a name="bkmk_overview"></a>Introducción a la puerta de enlace RAS  
Si su organización es un proveedor de servicios en la nube (CSP) o una empresa con varios inquilinos, puede implementar la puerta de enlace RAS en modo multiempresa para proporcionar el enrutamiento del tráfico de red hacia y desde redes físicas y virtuales, incluido Internet.  
  
Puede implementar la puerta de enlace RAS en el modo multiinquilino como puerta de enlace perimetral para enrutar el tráfico de red del cliente del inquilino a los recursos y las redes virtuales de inquilino.  
  
Cuando se implementan varias instancias de máquinas virtuales de puerta de enlace RAS que proporcionan alta disponibilidad y conmutación por error, se está implementando un grupo de puerta de enlace. En Windows Server 2012 R2, todas las máquinas virtuales de puerta de enlace han formado un solo grupo, lo que hizo un poco complicado la separación lógica de la implementación de la puerta de enlace.  La puerta de enlace de Windows Server 2012 R2 ofrecía una implementación de redundancia de 1:1 para las máquinas virtuales de puerta de enlace, lo que da lugar a una utilización insuficiente de la capacidad disponible para las conexiones VPN de sitio a sitio (S2S).  
  
Este problema se resuelve en Windows Server 2016, que proporciona varios grupos de puertas de enlace, que pueden ser de distintos tipos para la separación lógica. El nuevo modo de redundancia M + N permite una configuración de conmutación por error más eficaz.  
  
Para obtener información general sobre la puerta de enlace RAS, consulte [puerta de enlace ras](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="gateway-pools-overview"></a><a name="bkmk_pools"></a>Introducción a los grupos de puerta de enlace  
En Windows Server 2016, puede implementar puertas de enlace en uno o más grupos.  
  
En la ilustración siguiente se muestran diferentes tipos de grupos de puertas de enlace que proporcionan enrutamiento de tráfico entre redes virtuales.  
  
![Grupos de puerta de enlace RAS](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Cada grupo tiene las siguientes propiedades.  
  
-   Cada grupo es M + N redundante. Esto significa que se realiza una copia de seguridad de un número de máquinas virtuales de puerta de enlace activo con un número "N" de máquinas virtuales de puerta de enlace en espera. El valor de N (puertas de enlace en espera) siempre es menor o igual que M (puertas de enlace activas).  
  
-   Un grupo puede realizar cualquiera de las funciones de puerta de enlace individuales: Intercambio de claves por red versión 2 (IKEv2) S2S, nivel 3 (L3) y encapsulación de enrutamiento genérico (GRE), o el grupo puede realizar todas estas funciones.  
  
-   Puede asignar una sola dirección IP pública a todos los grupos o a un subconjunto de grupos. Al hacerlo, se reduce en gran medida el número de direcciones IP públicas que se deben usar, ya que es posible que todos los inquilinos se conecten a la nube en una única dirección IP. En la sección siguiente sobre alta disponibilidad y equilibrio de carga se describe cómo funciona esto.  
  
-   Puede escalar o reducir verticalmente un grupo de puerta de enlace fácilmente agregando o quitando máquinas virtuales de puerta de enlace en el grupo. La eliminación o adición de puertas de enlace no interrumpe los servicios proporcionados por un grupo. También puede Agregar y quitar grupos de puertas de enlace completos.  
  
-   Las conexiones de un solo inquilino pueden finalizar en varios grupos y en varias puertas de enlace de un grupo. Sin embargo, si un inquilino tiene conexiones finalizando en un grupo de puerta de enlace de **todos los** tipos, no se puede suscribir a otros grupos de puerta de enlace **de tipo o** de tipo individual.  
  
Los grupos de puerta de enlace también proporcionan la flexibilidad para habilitar escenarios adicionales:  
  
-   Grupos de inquilino único: puede crear un grupo para que lo use un inquilino.  
  
-   Si va a vender Cloud Services a través de canales de Partner (reseller), puede crear conjuntos independientes de grupos para cada distribuidor.  
  
-   Varios grupos pueden proporcionar la misma función de puerta de enlace pero distintas capacidades. Por ejemplo, puede crear un grupo de puerta de enlace que admita tanto conexiones de nivel de sitio de IKEv2 de alto rendimiento como de bajo rendimiento.  
  
## <a name="ras-gateway-deployment-overview"></a><a name="bkmk_deployment"></a>Introducción a la implementación de puerta de enlace RAS  
En la ilustración siguiente se muestra una implementación típica de proveedor de servicios en la nube (CSP) de puerta de enlace RAS.  
  
![Introducción a la implementación de puerta de enlace RAS](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Con este tipo de implementación, los grupos de puerta de enlace se implementan detrás de una Load Balancer de software (SLB), lo que permite al CSP asignar una única dirección IP pública para toda la implementación. Varias conexiones de puerta de enlace de un inquilino pueden finalizar en varios grupos de puertas de enlace, y también en varias puertas de enlace dentro de un grupo. Esto se muestra a través de las conexiones de S2S de IKEv2 en el diagrama anterior, pero lo mismo se aplica también a otras funciones de puerta de enlace, como las puertas de enlace L3 y GRE.  
  
En la ilustración, el dispositivo MT BGP es una puerta de enlace multiinquilino de RAS con BGP. BGP multiinquilino se usa para el enrutamiento dinámico. El enrutamiento de un inquilino es centralizado: un único punto, denominado reflector de rutas (RR), controla el emparejamiento BGP para todos los sitios de inquilino. El propio RR se distribuye entre todas las puertas de enlace de un grupo. Esto da como resultado una configuración en la que las conexiones de un inquilino (ruta de acceso de datos) terminan en varias puertas de enlace, pero el RR del inquilino (ruta de acceso de control de punto de emparejamiento de BGP) solo está en una de las puertas de enlace.  
  
El enrutador BGP se separa en el diagrama para describir este concepto de enrutamiento centralizado. La implementación de BGP de puerta de enlace también proporciona enrutamiento de tránsito, que permite que la nube actúe como punto de tránsito para el enrutamiento entre dos sitios de inquilino. Estas funcionalidades de BGP son aplicables a todas las funciones de puerta de enlace.  
  
## <a name="ras-gateway-integration-with-network-controller"></a><a name="bkmk_integration"></a>Integración de puerta de enlace RAS con controladora de red  
La puerta de enlace RAS está totalmente integrada con la controladora de red en Windows Server 2016. Cuando se implementa la puerta de enlace de RAS y la controladora de red, la controladora de red realiza las siguientes funciones.  
  
-   Implementación de los grupos de puerta de enlace  
  
-   Configuración de conexiones de inquilino en cada puerta de enlace  
  
-   Cambiar el flujo de tráfico de red a una puerta de enlace en espera en caso de error de puerta de enlace  
  
En las secciones siguientes se proporciona información detallada sobre la puerta de enlace RAS y la controladora de red.  
  
-   [Aprovisionamiento y equilibrio de carga de las conexiones de puerta de enlace (IKEv2, L3 y GRE)](#bkmk_provisioning)  
  
-   [Alta disponibilidad de IKEv2 S2S](#bkmk_ike)  
  
-   [Alta disponibilidad para GRE](#bkmk_gre)  
  
-   [Alta disponibilidad para puertas de enlace de reenvío L3](#bkmk_l3)  
  
### <a name="provisioning-and-load-balancing-of-gateway-connections-ikev2-l3-and-gre"></a><a name="bkmk_provisioning"></a>Aprovisionamiento y equilibrio de carga de las conexiones de puerta de enlace (IKEv2, L3 y GRE)  
Cuando un inquilino solicita una conexión de puerta de enlace, la solicitud se envía a la controladora de red. La controladora de red está configurada con información sobre todos los grupos de puerta de enlace, incluida la capacidad de cada grupo y cada puerta de enlace en cada grupo. Controladora de red selecciona el grupo y la puerta de enlace correctos para la conexión. Esta selección se basa en el requisito de ancho de banda para la conexión. La controladora de red usa un algoritmo de "ajuste perfecto" para elegir las conexiones de forma eficaz en un grupo. El punto de emparejamiento de BGP para la conexión también se designa en este momento si se trata de la primera conexión del inquilino.  
  
Después de que la controladora de red Seleccione una puerta de enlace RAS para la conexión, la controladora de red aprovisiona la configuración necesaria para la conexión en la puerta de enlace. Si la conexión es una conexión de S2S a través de IKEv2, la controladora de red también aprovisiona una regla de traducción de direcciones de red (NAT) en el grupo SLB; Esta regla NAT en el grupo SLB dirige las solicitudes de conexión desde el inquilino a la puerta de enlace designada. Los inquilinos se diferencian por la IP de origen, que se espera que sea único.  
  
> [!NOTE]  
> Las conexiones L3 y GRE omiten el SLB y se conectan directamente con la puerta de enlace RAS designada.  Estas conexiones requieren que el enrutador de extremo remoto (u otro dispositivo de terceros) debe estar configurado correctamente para conectarse con la puerta de enlace de RAS.  
  
Si el enrutamiento de BGP está habilitado para la conexión, el emparejamiento BGP lo inicia la puerta de enlace de RAS y las rutas se intercambian entre las puertas de enlace locales y en la nube. Las rutas que se aprenden mediante BGP (o que son rutas configuradas de forma estática Si no se usa BGP) se envían a la controladora de red. Después, el controlador de red sondea las rutas hasta los hosts de Hyper-V en los que se instalan las máquinas virtuales de inquilino. En este momento, el tráfico de inquilinos se puede enrutar al sitio local correcto. La controladora de red también crea directivas de virtualización de red de Hyper-V asociadas que especifican ubicaciones de puerta de enlace y las sondea con los hosts de Hyper-V.  
  
### <a name="high-availability-for-ikev2-s2s"></a><a name="bkmk_ike"></a>Alta disponibilidad de IKEv2 S2S  
Una puerta de enlace RAS de un grupo consta de las conexiones y el emparejamiento BGP de distintos inquilinos. Cada grupo tiene "puertas de enlace activas y" N "puertas de enlace en espera.  
  
La controladora de red controla el error de las puertas de enlace de la siguiente manera.  
  
-   La controladora de red hace ping constantemente a las puertas de enlace en todos los grupos y puede detectar una puerta de enlace con errores o con errores. La controladora de red puede detectar los siguientes tipos de errores de puerta de enlace de RAS.  
  
    -   Error de máquina virtual de puerta de enlace RAS  
  
    -   Error del host de Hyper-V en el que se está ejecutando la puerta de enlace RAS  
  
    -   Error del servicio de puerta de enlace RAS  
  
    La controladora de red almacena la configuración de todas las puertas de enlace activas implementadas. La configuración consta de la configuración de conexión y la configuración de enrutamiento.  
  
-   Cuando se produce un error en una puerta de enlace, afecta a las conexiones de inquilino en la puerta de enlace, así como a las conexiones de inquilino que se encuentran en otras puertas de enlace, pero cuyo RR reside en la puerta de enlace con errores. El tiempo de inactividad de las últimas conexiones es menor que el anterior. Cuando la controladora de red detecta una puerta de enlace con errores, realiza las siguientes tareas.  
  
    -   Quita las rutas de las conexiones afectadas de los hosts de proceso.  
  
    -   Quita las directivas de virtualización de red de Hyper-V en estos hosts.  
  
    -   Selecciona una puerta de enlace en espera, la convierte en una puerta de enlace activa y configura la puerta de enlace.  
  
    -   Cambia las asignaciones NAT en el grupo SLB para que las conexiones apunten a la nueva puerta de enlace.  
  
-   Al mismo tiempo, a medida que la configuración aparece en la nueva puerta de enlace activa, se restablecen las conexiones de S2S y el emparejamiento BGP. La puerta de enlace en la nube o la puerta de enlace local pueden iniciar las conexiones y el emparejamiento BGP. Las puertas de enlace actualizan sus rutas y las envían a la controladora de red. Una vez que el controlador de red aprende las nuevas rutas detectadas por las puertas de enlace, la controladora de red envía las rutas y las directivas de virtualización de red de Hyper-V asociadas a los hosts de Hyper-V donde residen las máquinas virtuales de los inquilinos afectados por errores. Esta actividad de la controladora de red es similar a la circunstancia de una nueva configuración de conexión, solo se produce en una escala mayor.  
  
### <a name="high-availability-for-gre"></a><a name="bkmk_gre"></a>Alta disponibilidad para GRE  
El proceso de respuesta de conmutación por error de puerta de enlace RAS por controladora de red (incluida la detección de errores) la copia de la configuración de conexión y enrutamiento en la puerta de enlace en espera, la conmutación por error del enrutamiento BGP/estático de las conexiones afectadas (incluida la retirada y reestructuración de rutas en hosts de proceso y el cambio de emparejamiento de BGP) y la reconfiguración de las directivas de virtualización de red de Hyper-V en hosts de proceso son las mismas para las conexiones Sin embargo, el restablecimiento de las conexiones GRE se produce de manera diferente y la solución de alta disponibilidad para GRE tiene algunos requisitos adicionales.  
  
![Alta disponibilidad para GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
En el momento de la implementación de la puerta de enlace, a cada máquina virtual de puerta de enlace RAS se le asigna una dirección IP dinámica (DIP). Además, a cada máquina virtual de puerta de enlace también se le asigna una dirección IP virtual (VIP) para alta disponibilidad de GRE. Las VIP se asignan solo a las puertas de enlace en grupos que pueden aceptar conexiones GRE y no a grupos no GRE. Las direcciones VIP asignadas se anuncian en los conmutadores de la parte superior del rack (TOR) mediante BGP, que luego anuncia más las VIP en la red física de la nube. Esto hace que se pueda acceder a las puertas de enlace desde los enrutadores remotos o los dispositivos de terceros en los que reside el otro extremo de la conexión GRE. Este emparejamiento BGP es diferente del emparejamiento BGP de nivel de inquilino para el intercambio de rutas de inquilino.  
  
En el momento del aprovisionamiento de la conexión GRE, el controlador de red selecciona una puerta de enlace, configura un extremo GRE en la puerta de enlace seleccionada y devuelve la dirección VIP de la puerta de enlace asignada. Esta VIP se configura entonces como la dirección de túnel GRE de destino en el enrutador remoto.  
  
Cuando se produce un error en una puerta de enlace, la controladora de red copia la dirección VIP de la puerta de enlace errónea y otros datos de configuración en la puerta de enlace en espera. Cuando la puerta de enlace en espera se activa, anuncia la dirección VIP a su conmutador TOR y más adelante en la red física. Los enrutadores remotos continúan conectando los túneles GRE a la misma VIP y la infraestructura de enrutamiento garantiza que los paquetes se enruten a la nueva puerta de enlace activa.  
  
### <a name="high-availability-for-l3-forwarding-gateways"></a><a name="bkmk_l3"></a>Alta disponibilidad para puertas de enlace de reenvío L3  
Una puerta de enlace de reenvío L3 de virtualización de red de Hyper-V es un puente entre la infraestructura física del centro de bits y la infraestructura virtualizada en la nube de virtualización de red de Hyper-V. En una puerta de enlace de reenvío L3 multiinquilino, cada inquilino usa su propia red lógica con etiqueta VLAN para la conectividad con la red física del inquilino.  
  
Cuando un nuevo inquilino crea una nueva puerta de enlace L3, la puerta de enlace de la controladora de red Service Manager selecciona una máquina virtual de puerta de enlace disponible y configura una nueva interfaz de inquilino con una dirección IP de espacio de direcciones de cliente (CA) de alta disponibilidad (de la red lógica etiquetada VLAN del inquilino. ). La dirección IP se usa como dirección IP del mismo nivel en la puerta de enlace remota (red física) y es el próximo salto para llegar a la red de virtualización de red de Hyper-V del inquilino.  
  
A diferencia de las conexiones de red de IPsec o GRE, el conmutador TOR no aprenderá dinámicamente la red con etiqueta VLAN del inquilino. El enrutamiento de la red con etiqueta VLAN del inquilino debe configurarse en el conmutador TOR y en todos los conmutadores y enrutadores intermedios entre la infraestructura física y la puerta de enlace para garantizar la conectividad de un extremo a otro.  A continuación se muestra un ejemplo de configuración de CSP Virtual Network como se describe en la ilustración siguiente.  
  
|Red|Subred|ID. DE VLAN|Puerta de enlace predeterminada|  
|-----------|----------|-----------|-------------------|  
|Red lógica de L3 de Contoso|10.127.134.0/24|1001|10.127.134.1|  
|Red lógica de Woodgrove L3|10.127.134.0/24|1002|10.127.134.1|  
  
A continuación se muestran configuraciones de puerta de enlace de inquilino de ejemplo, tal como se muestra en la ilustración siguiente.  
  
|Nombre de inquilino|Dirección IP de puerta de enlace L3|ID. DE VLAN|Dirección IP del mismo nivel|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Externa|10.127.134.60|1002|10.127.134.65|  
  
A continuación se ilustra la ilustración de estas configuraciones en un centro de recursos de CSP.  
  
![Alta disponibilidad para puertas de enlace de reenvío L3](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
Los errores de puerta de enlace, la detección de errores y el proceso de conmutación por error de puerta de enlace en el contexto de una puerta de enlace de reenvío L3 son similares a los procesos de las puertas de enlace RAS de IKEv2 y GRE. Las diferencias son en la forma en que se administran las direcciones IP externas.  
  
Cuando el estado de la máquina virtual de la puerta de enlace es incorrecto, la controladora de red selecciona una de las puertas de enlace en espera del grupo y vuelve a aprovisionar las conexiones de red y el enrutamiento en la puerta de enlace en espera. Al mover las conexiones, la dirección IP de espacio de CA de alta disponibilidad de la puerta de enlace de reenvío L3 también se mueve a la nueva máquina virtual de puerta de enlace junto con la dirección IP BGP de espacio de la CA del inquilino.  
  
Dado que la dirección IP de emparejamiento de L3 se mueve a la nueva máquina virtual de puerta de enlace durante la conmutación por error, la infraestructura física remota puede conectarse de nuevo a esta dirección IP y, posteriormente, llegar a la carga de trabajo de virtualización de red de Hyper-V. En el caso del enrutamiento dinámico BGP, como la dirección IP BGP de espacio de CA se mueve a la nueva máquina virtual de puerta de enlace, el enrutador BGP remoto puede volver a establecer el emparejamiento y obtener de nuevo todas las rutas de virtualización de red de Hyper-V.  
  
> [!NOTE]  
> Debe configurar por separado los conmutadores TOR y todos los enrutadores intermedios para usar la red lógica etiquetada VLAN para la comunicación de inquilinos. Además, la conmutación por error L3 está restringida a los bastidores que están configurados de esta manera. Por este motivo, el grupo de puerta de enlace L3 debe estar configurado con cuidado y la configuración manual debe completarse por separado.  
  


