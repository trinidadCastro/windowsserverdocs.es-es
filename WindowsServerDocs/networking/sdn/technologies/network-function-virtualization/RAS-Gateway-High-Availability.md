---
title: RAS puerta de enlace alta disponibilidad
description: Puedes usar este tema para obtener información sobre las configuraciones de alta disponibilidad para la puerta de enlace de RAS Multitenant para Software definido de redes (SDN) en Windows Server 2016.
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
ms.openlocfilehash: 48794ff8312ca00eda25f6d8bdca9929fc47084f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-high-availability"></a>RAS puerta de enlace alta disponibilidad

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre las configuraciones de alta disponibilidad para la puerta de enlace de RAS Multitenant para Software definido de redes (SDN).  
  
Este tema contiene las siguientes secciones.  
  
-   [Introducción a la puerta de enlace RAS](#bkmk_overview)  
  
-   [Información general de grupos de puerta de enlace](#bkmk_pools)  
  
-   [Información general sobre la implementación de puerta de enlace RAS](#bkmk_deployment)  
  
-   [Integración de puerta de enlace RAS con el controlador de red](#bkmk_integration)  
  
## <a name="bkmk_overview"></a>Introducción a la puerta de enlace RAS  
Si tu organización es un proveedor de servicio de nube (CSP) o una empresa con varios inquilinos, puedes implementar RAS puerta de enlace en el modo multiempresa para proporcionar el enrutamiento de tráfico de red hacia y desde las redes físicas y virtuales, incluso Internet.  
  
Puedes implementar RAS puerta de enlace en el modo multiempresa como una puerta de enlace de edge para enrutar el tráfico de red del cliente de inquilino de inquilinos recursos y las redes virtuales.  
  
Cuando implementas varias instancias de máquinas virtuales de puerta de enlace de RAS que proporcionan conmutación por error y alta disponibilidad, lo estás implementando un grupo de puerta de enlace. En Windows Server 2012 R2, la puerta de enlace máquinas virtuales forman a un grupo, un poco complicado que una separación lógica de la implementación de puerta de enlace.  Windows Server 2012 R2 puerta de enlace que ofrece una implementación de redundancia 1:1 para la puerta de enlace máquinas virtuales, resultantes debajo del uso de la capacidad disponible para el sitio a sitio (S2S) de las conexiones VPN.  
  
Este problema se resuelve en Windows Server 2016, que proporciona varios grupos de puerta de enlace - que puede ser de diferentes tipos de separación lógica. El nuevo modo de redundancia M + N permite una configuración más eficaz de conmutación por error.  
  
Para obtener más información acerca de la puerta de enlace de RAS, consulta [puerta de enlace de RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="bkmk_pools"></a>Información general de grupos de puerta de enlace  
En Windows Server 2016, puedes implementar puertas de enlace en uno o varios grupos.  
  
La siguiente ilustración muestra distintos tipos de grupos de puerta de enlace que proporcionan el enrutamiento de tráfico entre redes virtuales.  
  
![Grupos de puerta de enlace de RAS](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Cada grupo tiene las siguientes propiedades.  
  
-   Cada grupo es M + N redundante. Esto significa que un estoy ' número de máquinas virtuales de puerta de enlace active copia un número de'n ' de máquinas virtuales en modo de espera de la puerta de enlace. El valor de N (puertas de enlace en modo de espera) siempre es menor o igual a M (activas puertas de enlace).  
  
-   Un grupo puede realizar cualquiera de las funciones individuales de la puerta de enlace: intercambio de claves de Internet versión 2 S2S (IKEv2), nivel 3 (L3) y enrutamiento encapsulación genérico (GRE) - o el grupo puede realizar todas estas funciones.  
  
-   Puedes asignar una dirección IP pública para todos los grupos o a un subconjunto de grupos. Este en gran medida, reduce el número de direcciones IP públicas que debe usar, porque es posible tener todos los inquilinos Conéctate a la nube en una sola dirección IP. La sección siguiente sobre alta disponibilidad y equilibrio de carga describe cómo funciona esto.  
  
-   Puede escalar fácilmente un grupo de puerta de enlace hacia arriba o hacia abajo, agregando o quitando máquinas virtuales de puerta de enlace en el grupo. Retirada o la adición de puertas de enlace no interrumpir los servicios proporcionados por un grupo. También puedes agregar y quitar todos grupos de puertas de enlace.  
  
-   Pueden terminar de conexiones de un solo usuario en varios grupos de servidores y varias puertas de enlace en un grupo. Sin embargo, si un inquilino tiene conexiones termina en una **todos los** escriba el grupo de puerta de enlace, no puede suscribirse a otros **todos los** tipo o grupos de puerta de enlace de tipo individual.  
  
Grupos de puerta de enlace también proporcionan la flexibilidad necesaria para habilitar escenarios adicionales:  
  
-   Grupos de inquilino de solo: puedes crear un grupo para su uso por un inquilino.  
  
-   Si estás vendiendo servicios en la nube a través de los canales de partners (revendedor), puedes crear conjuntos diferentes de grupos para cada revendedor.  
  
-   Varios grupos pueden proporcionar la misma función de puerta de enlace pero capacidades diferentes. Por ejemplo, puedes crear un grupo de puerta de enlace que admite conexiones de bajo rendimiento IKEv2 S2S y de alto rendimiento.  
  
## <a name="bkmk_deployment"></a>Información general sobre la implementación de puerta de enlace RAS  
La siguiente ilustración muestra una implementación típica de proveedor de servicio de nube (CSP) de puerta de enlace de RAS.  
  
![Información general sobre la implementación de puerta de enlace RAS](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Con este tipo de implementación, los grupos de puerta de enlace se implementan detrás de un equilibrado de carga de Software (SLB), lo que permite el CSP asignar una sola dirección IP pública para toda la implementación. Varias conexiones de puerta de enlace de un inquilino pueden terminar en varios grupos de puerta de enlace - y también en varias puertas de enlace en un grupo. Esto se muestra a través de conexiones de IKEv2 S2S en el diagrama anterior, pero el mismo es aplicable a otras funciones de puerta de enlace, como L3 y GRE puertas de enlace.  
  
En la ilustración, el dispositivo MT BGP es una puerta de enlace de RAS Multitenant con BGP. BGP multiempresa sirve para enrutamiento dinámico. La ruta de un inquilino se centraliza - un único punto, llamado espejo ruta (RR), controla la interconexión BGP para todos los sitios de inquilino. El registro de recursos se distribuye entre todas las puertas de enlace en un grupo. Esto da como resultado una configuración que finalizar las conexiones de un inquilino (ruta de acceso de datos) en varias puertas de enlace, pero el RR del inquilino (BGP interconexión punto - ruta de acceso de control) es sólo en una de las puertas de enlace.  
  
El enrutador BGP se dividen en el diagrama para representar este concepto de enrutamiento centralizada. La implementación de puerta de enlace BGP también proporciona enrutamiento de tránsito, lo que permite a la nube para que actúe como un punto de transporte público de enrutamiento entre dos inquilino sitios. Estas funcionalidades BGP son aplicables a todas las funciones de puerta de enlace.  
  
## <a name="bkmk_integration"></a>Integración de puerta de enlace RAS con el controlador de red  
Puerta de enlace de RAS está totalmente integrado con el controlador de red en Windows Server 2016. Cuando se implementan RAS puerta de enlace y el controlador de red, el controlador de red realiza las siguientes funciones.  
  
-   Implementación de los grupos de puerta de enlace  
  
-   Configuración de conexiones de inquilino en cada puerta de enlace  
  
-   Cambiar el tráfico de red fluye a una puerta de enlace en modo de espera si se produce un error de puerta de enlace  
  
Las secciones siguientes proporcionan información detallada sobre RAS puerta de enlace y el controlador de red.  
  
-   [Aprovisionamiento y equilibrio de carga de conexiones de puerta de enlace (IKEv2, L3 y GRE)](#bkmk_provisioning)  
  
-   [Alta disponibilidad para IKEv2 S2S](#bkmk_ike)  
  
-   [Alta disponibilidad para GRE](#bkmk_gre)  
  
-   [Alta disponibilidad para L3 reenvío puertas de enlace](#bkmk_l3)  
  
### <a name="bkmk_provisioning"></a>Aprovisionamiento y equilibrio de carga de conexiones de puerta de enlace (IKEv2, L3 y GRE)  
Cuando un inquilino solicite una conexión de puerta de enlace, la solicitud se envía al controlador de red. Controlador de red está configurada con información sobre todos los grupos de puerta de enlace, incluida la capacidad de cada grupo de servidores y cada puerta de enlace en cada grupo. Controlador de red selecciona el grupo correcto y la puerta de enlace para la conexión. Esta selección se basa en el requisito de ancho de banda de la conexión. Controlador de red, usa un algoritmo "mejor se ajuste a" para seleccionar las conexiones de forma eficaz en un grupo. En este momento también se designa como el punto de interconexión BGP para la conexión si es la primera conexión del inquilino.  
  
Después de controlador de red se selecciona una puerta de enlace de RAS para la conexión, el controlador de red aprovisiona la configuración necesaria para la conexión en la puerta de enlace. Si la conexión es una conexión de S2S IKEv2, el controlador de red también aprovisiona una regla de traducción de direcciones de red (NAT) en el grupo SLB; Esta regla NAT en el grupo SLB dirige solicitudes de conexión desde el inquilino a la puerta de enlace designado. Inquilinos se diferencian la dirección IP de origen, que se espera que sea único.  
  
> [!NOTE]  
> Conexiones L3 y GRE omitir la SLB y conectan directamente con la puerta de enlace de RAS designado.  Estas conexiones requieren que el enrutador de extremo remoto (u otro dispositivo de terceros) debe configurarse correctamente para conectar con la puerta de enlace de RAS.  
  
Si BGP enrutamiento está habilitado para la conexión, interconexión BGP iniciada por la puerta de enlace de RAS - y rutas se intercambian de forma local y la nube puertas de enlace. Las rutas que aprende BGP (o que están configuradas estáticamente rutas si no se usa BGP) se envían al controlador de red. Controlador de red, a continuación, sondea las rutas a los hosts de Hyper-V en el que se instalan el inquilino máquinas virtuales. En este punto, puede enrutar el tráfico de inquilino en el sitio correcto local. Controlador de red también crea directivas de virtualización de Hyper-V red asociadas que especifican ubicaciones de puerta de enlace y les sondea hasta los hosts de Hyper-V.  
  
### <a name="bkmk_ike"></a>Alta disponibilidad para IKEv2 S2S  
Una puerta de enlace de RAS en un grupo consta de las conexiones y BGP interconexión de diferentes inquilinos. Cada grupo tiene estoy ' activas puertas de enlace y puertas de enlace en modo de espera'n '.  
  
Controlador de red controla el error de puertas de enlace de la siguiente manera.  
  
-   Controlador de red constantemente ping las puertas de enlace en todos los grupos y puede detectan una puerta de enlace que se produjo el error o producir un error. Controlador de red puede detectar los siguientes tipos de errores de puerta de enlace de RAS.  
  
    -   Error de la máquina virtual de puerta de enlace de RAS  
  
    -   Error en el host de Hyper-V en la que se ejecuta la puerta de enlace de RAS  
  
    -   Error de servicio de puerta de enlace de RAS  
  
    Controlador de red almacena la configuración de todas las puertas de enlace activas implementadas. Configuración consta de la configuración de conexión y enrutamiento.  
  
-   Cuando se produce un error en una puerta de enlace, afecta a las conexiones de inquilino en la puerta de enlace, así como las conexiones de inquilino que se encuentran en otras puertas de enlace pero cuya RR reside en la puerta de enlace error. El tiempo de inactividad de las conexiones de este últimos es menor que el primero. Cuando el controlador de red detecta una puerta de enlace error, realice las siguientes tareas.  
  
    -   Quita las rutas de las conexiones afectadas los hosts de cálculo.  
  
    -   Quita las directivas de virtualización de Hyper-V en red en estos hosts.  
  
    -   Selecciona una puerta de enlace en modo de espera, se convierte en una puerta de enlace activa y configura la puerta de enlace.  
  
    -   Cambia las asignaciones NAT en el grupo SLB para señalar las conexiones a la puerta de enlace nuevo.  
  
-   Al mismo tiempo, como la configuración aparezca en la puerta de enlace active nuevo, se restablecen las conexiones de IKEv2 S2S y BGP interconexión. Las conexiones y BGP interconexión se pueden iniciar mediante la puerta de enlace de nube o la puerta de enlace local. Las puertas de enlace actualización sus rutas y las envían al controlador de red. Después de controlador de red aprende las rutas nuevo detectadas las puertas de enlace, el controlador de red envía las rutas y las directivas de virtualización de Hyper-V red asociadas a los hosts de Hyper-V donde residen las máquinas virtuales de los inquilinos afectadas por error. Esta actividad de controlador de red es similar en el caso de una nueva configuración de conexión, solo se produce en una escala mayor.  
  
### <a name="bkmk_gre"></a>Alta disponibilidad para GRE  
El proceso de respuesta de conmutación por error de puerta de enlace de RAS por el controlador de red detección de un error como por ejemplo, copiar conexión y configuración de enrutamiento para la puerta de enlace en modo de espera, conmutación por error de enrutamiento BGP estático o de la afectadas las conexiones (incluida la retirada y volver a la conexión de rutas de calculan hosts y volver a interconexión BGP) y la reconfiguración de directivas de virtualización de Hyper-V en red en los hosts de cálculo - es el mismo para las conexiones y puertas de enlace GRE. El restablecimiento de conexiones GRE sucede de manera diferente, sin embargo, y la solución de alta disponibilidad para GRE tiene algunos requisitos adicionales.  
  
![Alta disponibilidad para GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
En el momento de la implementación de la puerta de enlace, cada VM de puerta de enlace de RAS se le asigna una dirección IP dinámica (DIP). Además, cada VM también se le asigna una dirección IP virtual (VIP) para una alta disponibilidad GRE. Se asignan VIPs solo a las puertas de enlace en grupos que pueden aceptar conexiones GRE y no a grupos no GRE. Se anuncian el VIPs asignados a la parte superior de los conmutadores de bastidor (términos de referencia) con BGP, que, a continuación, anuncia aún más la VIPs en la red física de la nube. Esto hace que las puertas de enlace accesible desde el enrutadores remotos o dispositivos de terceros donde reside el otro extremo de la conexión GRE. Esta interconexión BGP es diferente a la BGP de nivel de inquilino interconexión de intercambio de rutas de inquilino.  
  
En el momento de aprovisionamiento de conexión GRE, controlador de red selecciona una puerta de enlace, configura un extremo GRE en la puerta de enlace seleccionado y devuelve la dirección VIP de la puerta de enlace asignado. Este VIP, a continuación, se configura como destino direcciones de túnel GRE en el enrutador remoto.  
  
Cuando se produce un error en una puerta de enlace, el controlador de red copia la dirección VIP de la puerta de enlace erróneos y otros datos de configuración a la puerta de enlace en modo de espera. Cuando se activa la puerta de enlace en modo de espera, se anuncia la VIP a su conmutador de términos de referencia y aún más en la red física. Enrutadores remotos continuarán conectar los túneles GRE con la misma dirección VIP y la infraestructura de enrutamiento garantiza que los paquetes se enrutan a la puerta de enlace activo nuevo.  
  
### <a name="bkmk_l3"></a>Alta disponibilidad para L3 reenvío puertas de enlace  
Una puerta de enlace de reenvío L3 de virtualización de red de Hyper-V es un puente entre la infraestructura física en el centro de datos y la infraestructura virtualizada en la nube de virtualización de Hyper-V en red. En una puerta de enlace de reenvío L3 multiempresa, cada inquilino usa su propia red lógica de etiquetado VLAN para tener conectividad con la red física del inquilino.  
  
Cuando un inquilino con el nuevo crea una nueva puerta de enlace L3, Administrador de servicios de puerta de enlace de controlador de red selecciona una máquina virtual de puerta de enlace disponible y configura una nueva interfaz de inquilino con una dirección IP de espacio de direcciones de cliente de certificación (CA) altamente disponible (desde VLAN etiquetada lógicas de red del inquilino). La dirección IP se usa como la dirección IP del mismo nivel en la puerta de enlace remoto (física de red) y es el próximo salto para llegar a la red de virtualización de Hyper-V en red del inquilino.  
  
A diferencia de IPsec o GRE conexiones de red, el modificador de términos de referencia no conocerás red etiquetado de VLAN del inquilino dinámicamente. La ruta de red con etiquetas de VLAN del inquilino debe configurarse en el conmutador de términos de referencia y todos los dispositivos intermedios y enrutadores entre infraestructura física y la puerta de enlace para garantizar la conectividad de extremo a extremo.  Siguiente es un ejemplo de configuración de red Virtual de CSP, como se muestra en la ilustración siguiente.  
  
|Red|Subred|ID. DE VLAN|Puerta de enlace predeterminada|  
|-----------|----------|-----------|-------------------|  
|Red lógica de Contoso L3|10.127.134.0/24|1001|10.127.134.1|  
|Ejemplo de Woodgrove L3 lógicas de red|10.127.134.0/24|1002|10.127.134.1|  
  
Siguientes son las configuraciones de puerta de enlace de inquilino de ejemplo, como se muestra en la ilustración siguiente.  
  
|Nombre de inquilino|Puerta de enlace L3 dirección IP|ID. DE VLAN|Dirección IP del mismo nivel|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Ejemplo de Woodgrove|10.127.134.60|1002|10.127.134.65|  
  
La siguiente es la ilustración de estas configuraciones en un centro de datos CSP.  
  
![Alta disponibilidad para L3 reenvío puertas de enlace](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
Los errores de puerta de enlace, detección de un error y el proceso de conmutación por error de puerta de enlace en el contexto de una L3 reenvío de puerta de enlace es similar a los procesos de IKEv2 y puertas de enlace de GRE RAS. Las diferencias consisten en la forma en que se controlan las direcciones IP externas.  
  
Cuando el estado de la máquina virtual de puerta de enlace se convierte en mal estado, el controlador de red selecciona una de las puertas de enlace en modo de espera de la agrupación y aprovisiona volver a las conexiones de red y el enrutamiento en la puerta de enlace en modo de espera. Mientras mueve las conexiones, la puerta de enlace L3 reenvío muy dirección IP de disponible CA espacio también se mueve a la puerta de enlace nueva máquina virtual junto con el espacio de CA dirección IP BGP del inquilino.  
  
Porque la dirección IP de interconexión de L3 se mueve a la puerta de enlace nueva máquina virtual durante la conmutación por error, la infraestructura física remota nuevo es capaz de conectarse a esta dirección IP y, posteriormente, llegar a la carga de trabajo de virtualización de Hyper-V en red. Para BGP enrutamiento dinámico, como el espacio de la CA dirección IP BGP se mueve a la puerta de enlace nueva máquina virtual, el enrutador BGP remoto puede volver a establecer interconexión y Obtén información sobre todas las rutas de virtualización de Hyper-V red nuevamente.  
  
> [!NOTE]  
> Por separado, debes configurar los conmutadores de términos de referencia y todos los enrutadores intermedios para poder usar la red lógica de etiquetado VLAN para la comunicación del inquilino. Además, L3 conmutación por error está restringido a solo los bastidores que están configurados de este modo. Por este motivo, el grupo de puerta de enlace L3 debe estar configurado con cuidado y se debe completar la configuración manual por separado.  
  


