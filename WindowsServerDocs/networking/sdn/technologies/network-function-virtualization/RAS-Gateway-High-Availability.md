---
title: Alta disponibilidad de la puerta de enlace de RAS
description: Puede utilizar este tema para obtener información sobre las configuraciones de alta disponibilidad para la puerta de enlace de RAS para varios inquilinos para definidas por Software Networking (SDN) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 34d826c9-65bc-401f-889d-cf84e12f0144
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8ce515ceeb7ab6989ef18055f312983a6518a1a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847766"
---
# <a name="ras-gateway-high-availability"></a>Alta disponibilidad de la puerta de enlace de RAS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información sobre las configuraciones de alta disponibilidad para la puerta de enlace de RAS para varios inquilinos para Software Defined Networking (SDN).  
  
En este tema se incluyen las siguientes secciones.  
  
-   [Información general de puerta de enlace RAS](#bkmk_overview)  
  
-   [Introducción a los grupos de puerta de enlace](#bkmk_pools)  
  
-   [Introducción a la implementación de puerta de enlace RAS](#bkmk_deployment)  
  
-   [Integración de la puerta de enlace RAS con controladora de red](#bkmk_integration)  
  
## <a name="bkmk_overview"></a>Información general de puerta de enlace RAS  
Si su organización es un proveedor de servicios en la nube (CSP) o una empresa con varios inquilinos, puede implementar la puerta de enlace de RAS en el modo multiempresa para proporcionar enrutamiento de tráfico de red hacia y desde redes físicas y virtuales, incluido Internet.  
  
Puede implementar la puerta de enlace de RAS en el modo multiempresa como una puerta de enlace de borde para enrutar el tráfico de red de cliente de inquilino a los recursos y redes virtuales de inquilinos.  
  
Al implementar varias instancias de máquinas virtuales de puerta de enlace de RAS que proporcionan alta disponibilidad y conmutación por error, va a implementar un grupo de servidores de puerta de enlace. En Windows Server 2012 R2, la puerta de enlace las máquinas virtuales forman un único grupo, que realiza un poco difícil una separación lógica de la implementación de puerta de enlace.  Puerta de enlace de Windows Server 2012 R2 ofrece una implementación de redundancia de 1:1 para la puerta de enlace de las máquinas virtuales, lo que provocó las conexiones VPN de infrautilización de la capacidad disponible para el sitio a sitio (S2S).  
  
Este problema se resuelve en Windows Server 2016, que proporciona varios grupos de puerta de enlace: puede ser de distintos tipos para una separación lógica. El nuevo modo de redundancia M+N permite una configuración de conmutación por error más eficaz.  
  
Para obtener más información acerca de la puerta de enlace RAS, consulte [puerta de enlace RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="bkmk_pools"></a>Introducción a los grupos de puerta de enlace  
En Windows Server 2016, puede implementar puertas de enlace en uno o varios grupos.  
  
La siguiente ilustración muestra distintos tipos de grupos de puerta de enlace que proporcionan el enrutamiento del tráfico entre redes virtuales.  
  
![Grupos de puerta de enlace RAS](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Cada grupo tiene las siguientes propiedades.  
  
-   Cada grupo es con redundancia M+N. Esto significa que un ' m ' número de máquinas virtuales de puerta de enlace activo estén respaldado por un número de "n" de las máquinas virtuales de puerta de enlace en espera. El valor de N (puertas de enlace en espera) siempre es menor o igual a M (puertas de enlace activos).  
  
-   Un grupo puede realizar cualquiera de las funciones de puerta de enlace individuales: versión de intercambio de claves por Internet S2S de 2 (IKEv2), nivel 3 (L3) y enrutamiento encapsulación genérico (GRE) - o el grupo puede realizar todas estas funciones.  
  
-   Puede asignar una única dirección IP pública para todos los grupos o a un subconjunto de los grupos. En gran medida de este, se reduce el número de direcciones IP públicas que se debe usar, porque es posible tener todos los inquilinos conectar a la nube en una única dirección IP. La siguiente sección sobre alta disponibilidad y equilibrio de carga describe cómo funciona esto.  
  
-   Puede escalar fácilmente un grupo de servidores de puerta de enlace hacia arriba o hacia abajo agregando o quitando máquinas virtuales de puerta de enlace en el grupo. Eliminación o adición de puertas de enlace no interrumpe los servicios proporcionados por un grupo. También puede agregar y quitar grupos completos de las puertas de enlace.  
  
-   Pueden finalizar las conexiones de un solo inquilino en varios grupos y varias puertas de enlace en un grupo. Sin embargo, si un inquilino tiene conexiones de terminación en una **todos los** escriba el grupo de servidores de puerta de enlace, no se puede suscribir a otros **todas** tipo o grupos de servidores de puerta de enlace de tipo individual.  
  
Grupos de puerta de enlace también proporcionan la flexibilidad necesaria para habilitar escenarios adicionales:  
  
-   Grupos de un único inquilino: puede crear un grupo para su uso por un inquilino.  
  
-   Si vende servicios de nube a través de canales asociado (distribuidor), puede crear conjuntos independientes de los grupos para cada distribuidor.  
  
-   Varios grupos pueden proporcionar la misma función de puerta de enlace, pero las diferentes capacidades. Por ejemplo, puede crear un grupo de puerta de enlace que admita conexiones S2S IKEv2 un rendimiento bajo y alto rendimiento.  
  
## <a name="bkmk_deployment"></a>Introducción a la implementación de puerta de enlace RAS  
La siguiente ilustración muestra una implementación típica del proveedor de servicios en la nube (CSP) de la puerta de enlace RAS.  
  
![Introducción a la implementación de puerta de enlace RAS](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Con este tipo de implementación, los grupos de puerta de enlace se implementan detrás de un equilibrador de carga de Software (SLB), lo que permite el CSP asignar una dirección IP pública única para toda la implementación. Varias conexiones de puerta de enlace de un inquilino pueden terminar en varios grupos de puerta de enlace - y también en varias puertas de enlace dentro de un grupo. Esto se muestra a través de conexiones S2S de IKEv2 en el diagrama anterior, pero el mismo es aplicable a otras funciones de la puerta de enlace, como puertas de enlace L3 y GRE.  
  
En la ilustración, el dispositivo de destino maestro BGP es una puerta de enlace de RAS para varios inquilinos con BGP. Se usa BGP multiinquilino para enrutamiento dinámico. El enrutamiento para un inquilino se centraliza - un punto único, denominado el reflector de ruta (RR), controla el emparejamiento BGP para todos los sitios de inquilino. El registro de recursos se distribuye entre todas las puertas de enlace en un grupo. Esto da como resultado una configuración donde terminan las conexiones de un inquilino (ruta de acceso de datos) en varias puertas de enlace, pero el registro de recursos para el inquilino (BGP entre pares punto - ruta de acceso de control) está en una de las puertas de enlace.  
  
El enrutador BGP se dividen en el diagrama para ilustrar este concepto de enrutamiento centralizado. La implementación de BGP de puerta de enlace también proporciona enrutamiento de tránsito, lo que permite a la nube para que actúe como punto de tránsito de enrutamiento entre dos sitios. Estas capacidades BGP son aplicables a todas las funciones de la puerta de enlace.  
  
## <a name="bkmk_integration"></a>Integración de la puerta de enlace RAS con controladora de red  
Puerta de enlace RAS está totalmente integrado con controladora de red en Windows Server 2016. Cuando se implementan la controladora de red y de puerta de enlace RAS, controladora de red realiza las siguientes funciones.  
  
-   Implementación de los grupos de puerta de enlace  
  
-   Configuración de conexiones de inquilino en cada puerta de enlace  
  
-   Cambiar el tráfico de red fluye a una puerta de enlace en espera si se produce un error de puerta de enlace  
  
Las secciones siguientes proporcionan información detallada acerca de la controladora de red y de puerta de enlace RAS.  
  
-   [Aprovisionamiento y equilibrio de carga de las conexiones de puerta de enlace (IKEv2, GRE y L3)](#bkmk_provisioning)  
  
-   [Alta disponibilidad para IKEv2 S2S](#bkmk_ike)  
  
-   [Alta disponibilidad para GRE](#bkmk_gre)  
  
-   [Alta disponibilidad para L3 puertas de enlace de reenvío](#bkmk_l3)  
  
### <a name="bkmk_provisioning"></a>Aprovisionamiento y equilibrio de carga de las conexiones de puerta de enlace (IKEv2, GRE y L3)  
Cuando un inquilino solicita una conexión de puerta de enlace, la solicitud se envía a la controladora de red. Controladora de red se configura con información acerca de todos los grupos de puerta de enlace, incluida la capacidad de cada grupo y cada puerta de enlace en cada grupo. Controladora de red selecciona el grupo correcto y la puerta de enlace para la conexión. Esta selección se basa en el requisito de ancho de banda para la conexión. Controladora de red utiliza un algoritmo de "ajuste perfecto" para elegir las conexiones de forma eficaz en un grupo. En este momento también se designa como el punto de emparejamiento BGP para la conexión si ésta es la primera conexión del inquilino.  
  
Después de controladora de red se selecciona una puerta de enlace RAS para la conexión, la controladora de red proporciona la configuración necesaria para la conexión en la puerta de enlace. Si la conexión es una conexión S2S IKEv2, controladora de red proporciona también una regla de traducción de direcciones de red (NAT) en el grupo de SLB; Esta regla NAT en el grupo SLB dirige las solicitudes de conexión del inquilino a la puerta de enlace designada. Los inquilinos se diferencian por la dirección IP de origen, que se espera que sea único.  
  
> [!NOTE]  
> Conexiones L3 y GRE omiten el SLB y se conectan directamente con la puerta de enlace de RAS designado.  Estas conexiones requieren que el enrutador de punto de conexión remoto (u otro dispositivo de terceros) debe configurarse correctamente para conectarse con la puerta de enlace de RAS.  
  
Si el enrutamiento de BGP está habilitado para la conexión, entonces emparejamiento BGP se inicia mediante la puerta de enlace de RAS - y rutas se intercambian entre locales y en la nube las puertas de enlace. Las rutas que se aprenden mediante BGP (o que son las rutas configuradas estáticamente, si no se usa BGP) se envían a la controladora de red. Controladora de red, a continuación, sondea las rutas hacia abajo hasta los hosts de Hyper-V en el que se instalan las máquinas virtuales de inquilino. En este momento, se puede enrutar el tráfico de inquilinos al sitio correcto en el entorno local. Controladora de red sondea ellos hasta los hosts de Hyper-V y también crea directivas de virtualización de red de Hyper-V asociadas que especifican las ubicaciones de la puerta de enlace.  
  
### <a name="bkmk_ike"></a>Alta disponibilidad para IKEv2 S2S  
Consta de una puerta de enlace de RAS en un grupo de conexiones y el emparejamiento BGP de inquilinos diferentes. Cada grupo tiene una ' m ' puertas de enlace activas y las puertas de enlace en espera de "n".  
  
Controladora de red controla el error de las puertas de enlace de la siguiente manera.  
  
-   Controladora de red constantemente pings de las puertas de enlace de todos los grupos y pueda detectan una puerta de enlace es error o con errores. Controladora de red puede detectar los siguientes tipos de errores de puerta de enlace RAS.  
  
    -   Error de la máquina virtual de puerta de enlace de RAS  
  
    -   Error del host de Hyper-V en el que se ejecuta la puerta de enlace de RAS  
  
    -   Error del servicio de puerta de enlace RAS  
  
    Controladora de red almacena la configuración de todas las puertas de enlace activas implementadas. Configuración consta de la configuración de conexión y configuración de enrutamiento.  
  
-   Cuando se produce un error en una puerta de enlace, afecta a las conexiones de inquilino en la puerta de enlace, así como las conexiones de inquilino que se encuentran en otras puertas de enlace, pero cuyo RR reside en la puerta de enlace errónea. El tiempo de inactividad de las conexiones de este últimos es menor que el primero. Cuando la controladora de red detecta una puerta de enlace con errores, realiza las siguientes tareas.  
  
    -   Quita las rutas de las conexiones afectadas de los hosts de proceso.  
  
    -   Quita las directivas de virtualización de red de Hyper-V en estos hosts.  
  
    -   Selecciona una puerta de enlace en espera, lo convierte en una puerta de enlace activo y configura la puerta de enlace.  
  
    -   Cambia las asignaciones NAT en el grupo SLB para que apunte a las conexiones a la nueva puerta de enlace.  
  
-   Al mismo tiempo, como la configuración aparece en la nueva puerta de enlace de activo, el emparejamiento BGP y las conexiones S2S de IKEv2 se restablecen. El emparejamiento BGP y las conexiones se pueden iniciar la puerta de enlace en la nube o la puerta de enlace local. Las puertas de enlace de las rutas de actualización y envían a la controladora de red. Después de controladora de red aprende las rutas de nuevo detectadas por las puertas de enlace, controladora de red envía las rutas y las directivas de virtualización de red de Hyper-V asociadas a los hosts de Hyper-V donde residen las máquinas virtuales de los inquilinos afectados por el error. Esta actividad controladora de red es similar en el caso de una nueva configuración de conexión, solo se produce en una escala mayor.  
  
### <a name="bkmk_gre"></a>Alta disponibilidad para GRE  
El proceso de respuesta de la conmutación por error de puerta de enlace RAS de controladora de red - incluida la detección de errores, copiar la conexión y configuración de enrutamiento para la puerta de enlace en espera de conmutación por error de enrutamiento estático o BGP de las conexiones afectadas (incluida la retirada y volver a la estructura de rutas en proceso hosts y volver a emparejamiento BGP), y volver a configurar las directivas de virtualización de red de Hyper-V en hosts de proceso: es el mismo para las conexiones y puertas de enlace de GRE. El restablecimiento de conexiones de GRE ocurre de manera diferente, sin embargo, y la solución de alta disponibilidad para GRE tiene algunos requisitos adicionales.  
  
![Alta disponibilidad para GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
En el momento de la implementación de puerta de enlace, cada máquina virtual de puerta de enlace de RAS se asigna una dirección IP dinámica (DIP). Además, cada máquina virtual de puerta de enlace también se asigna una dirección IP virtual (VIP) para lograr alta disponibilidad GRE. Las direcciones VIP se asignan solo a las puertas de enlace en los grupos que pueden aceptar conexiones GRE y no a los grupos que no sean GRE. La VIP asignadas se anuncian en la parte superior de conmutadores de estantería (TOR) mediante BGP, que, a continuación, anuncia aún más las VIP en la red física en la nube. Esto hace que las puertas de enlace accesible desde los enrutadores remotos o dispositivos de terceros en el que reside el otro extremo de la conexión GRE. Este emparejamiento de BGP es diferente el nivel de inquilino de emparejamiento BGP para el intercambio de las rutas de inquilino.  
  
Controladora de red en el momento de aprovisionamiento de la conexión GRE, selecciona una puerta de enlace, configura un punto de conexión GRE en la puerta de enlace seleccionada y devuelve la dirección VIP de la puerta de enlace asignada. Esta dirección VIP, a continuación, se configura como la dirección de túnel GRE en el enrutador remoto de destino.  
  
Cuando se produce un error en una puerta de enlace, controladora de red copia la dirección VIP de la puerta de enlace con errores y otros datos de configuración en la puerta de enlace en espera. Si la puerta de enlace en espera se activa, se anuncia la dirección VIP a su conmutador TOR y más en la red física. Enrutadores remotos siguen funcionando túneles GRE en la misma dirección VIP y la infraestructura de enrutamiento garantiza que los paquetes se enrutan a la nueva puerta de enlace activo.  
  
### <a name="bkmk_l3"></a>Alta disponibilidad para L3 puertas de enlace de reenvío  
Una puerta de enlace de reenvío L3 de virtualización de red de Hyper-V es un puente entre la infraestructura física del centro de datos y la infraestructura virtualizada en la nube de Hyper-V Network Virtualization. En una puerta de enlace de reenvío L3 para varios inquilinos, cada inquilino usa su propia red lógica de etiquetado de VLAN para la conectividad de red físico del inquilino.  
  
Cuando un nuevo inquilino crea una nueva puerta de enlace L3, Administrador de servicios de puerta de enlace de controlador de red selecciona una máquina virtual de puerta de enlace disponible y configura una nueva interfaz de inquilino con una dirección IP de espacio de direcciones de cliente (CA) altamente disponible (desde la red lógica etiquetada del inquilino VLAN ). La dirección IP se usa como la dirección IP del mismo nivel en la puerta de enlace remota (de red físico) y es el próximo salto para llegar a la red de virtualización de red de Hyper-V del inquilino.  
  
A diferencia de las conexiones de red de IPsec o GRE, el conmutador TOR no aprenderá la red del inquilino VLAN etiquetada dinámicamente. El enrutamiento de red etiquetada de VLAN del inquilino debe configurarse en el conmutador TOR y todos los conmutadores intermedios y enrutadores entre la infraestructura física y la puerta de enlace para garantizar la conectividad de extremo a extremo.  Siguiente es un ejemplo de configuración de red Virtual de CSP, como se muestra en la ilustración siguiente.  
  
|Red|Subred|ID. DE VLAN|Puerta de enlace predeterminada|  
|-----------|----------|-----------|-------------------|  
|Red lógica Contoso L3|10.127.134.0/24|1001|10.127.134.1|  
|Red lógica Woodgrove L3|10.127.134.0/24|1002|10.127.134.1|  
  
Siguientes son las configuraciones de puerta de enlace de inquilino de ejemplo como se muestra en la ilustración siguiente.  
  
|Nombre del inquilino|Dirección IP de puerta de enlace de L3|ID. DE VLAN|Dirección IP del par|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Woodgrove|10.127.134.60|1002|10.127.134.65|  
  
La siguiente es la ilustración de estas configuraciones en un centro de datos CSP.  
  
![Alta disponibilidad para L3 puertas de enlace de reenvío](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
Los errores de puerta de enlace, detección de errores y el proceso de conmutación por error de la puerta de enlace en el contexto de un L3 puerta de enlace de reenvío es similar a los procesos de IKEv2 y puertas de enlace de RAS de GRE. Las diferencias están en el modo en que se tratan las direcciones IP externas.  
  
Cuando la puerta de enlace de estado de la máquina virtual pasa a ser incorrecto, controladora de red selecciona una de las puertas de enlace en modo de espera del grupo y vuelve a aprovisionar las conexiones de red y enrutamiento de la puerta de enlace en espera. Mientras mueve las conexiones, la puerta de enlace de reenvío L3 alta la dirección IP de CA espacio disponible también se mueve a la puerta de enlace nueva máquina virtual junto con el espacio de CA dirección IP BGP del inquilino.  
  
Dado que la dirección IP de emparejamiento L3 se mueve a la puerta de enlace nueva máquina virtual durante la conmutación por error, la infraestructura física remota nuevo es capaz de conectarse a esta dirección IP y, posteriormente, llegar a la carga de trabajo de virtualización de red de Hyper-V. Para enrutamiento dinámico de BGP, como el espacio de CA dirección IP BGP se mueve a la puerta de enlace nueva máquina virtual, el enrutador BGP remoto puede volver a establecer el emparejamiento y obtenga información sobre todas las rutas de virtualización de red de Hyper-V de nuevo.  
  
> [!NOTE]  
> Debe configurar por separado los conmutadores TOR y todos los enrutadores intermedios para poder usar la red lógica de etiquetado de VLAN para la comunicación del inquilino. Además, L3 conmutación por error está restringido a solo los bastidores que están configurados de esta manera. Por este motivo, el grupo de servidores de puerta de enlace L3 debe configurarse con cuidado y se debe completar la configuración manual por separado.  
  


