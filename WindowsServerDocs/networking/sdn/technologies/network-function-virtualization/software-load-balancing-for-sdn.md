---
title: Software equilibrio de carga (SLB) para SDN
description: Puedes usar este tema para obtener información sobre el equilibrio de carga de Software para Software definido a redes en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e08afddde9c7be8d955a0cfdaf44f0fc31b8155
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="software-load-balancing-slb-for-sdn"></a>Software equilibrio de carga \(SLB\) para SDN

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre el equilibrio de carga de Software para Software definido a redes en Windows Server 2016.  

Proveedores de servicios de nube (CSP) y las empresas que implementan Software definido de redes (SDN) en Windows Server 2016 usar Equilibrio de carga de Software (SLB) para distribuir uniformemente inquilino y tráfico de red de cliente de inquilino entre los recursos de red virtual. La SLB de servidor de Windows permite varios servidores hospedar la misma carga de trabajo que escalabilidad y alta disponibilidad.
  
Windows Server SLB incluye las siguientes funcionalidades.  
  
-   Capa 4 Servicios de equilibrio de carga (L4) para el tráfico de 'Oriental oeste' TCP o UDP y 'Norte sur'.  
  
-   Interno y pública equilibrio de carga de tráfico de red.  
  
-   Admite direcciones IP dinámicas (DIP) en redes de área Local virtual (VLAN) y en redes virtuales que se crea mediante la virtualización de Hyper-V en red.  
  
-   Soporte de sondeo del estado.  
  
-   Listo para la escala de nube, incluida la capacidad de escalado y ajustar la escala funcionalidad para multiplexores y agentes de Host.  
  
Para obtener más información, consulta [características de equilibrio de carga de Software](#bkmk_features) en este tema.  
  
> [!NOTE]  
> Multitenancy para VLAN no es compatible con el controlador de red, pero puedes usar VLAN con SLB para proveedores de servicios administrados cargas de trabajo, por ejemplo, la infraestructura de centro de datos y los servidores Web de alta densidad.  
  
Con Windows Server SLB, se puede escalar el funcionalidades con las máquinas virtuales SLB en los mismos servidores de cálculo de Hyper-V que usas para las otras cargas de trabajo de máquina virtual de equilibrio de carga. Por este motivo, SLB admite la creación rápida y eliminación de equilibrio de carga de extremos que se necesita para operaciones de CSP. Además, Windows Server SLB admite decenas de gigabytes por clúster, proporciona un modelo de aprovisionamiento simple y es fácil de escala y, en.  
  
**Cómo funciona SLB**  
  
SLB funciona mediante la asignación de direcciones IP virtuales (VIP) a las direcciones IP dinámicas (DIP) que forman parte de un conjunto de servicio de nube de recursos en el centro de datos.  
  
VIPs son solo direcciones IP que proporcionan acceso público a un grupo de carga equilibrada máquinas virtuales. Por ejemplo, VIPs son direcciones IP que se exponen en Internet para que los inquilinos y los clientes de inquilino pueden conectarse a recursos de inquilino en el centro de datos de la nube.  
  
DIP son las direcciones IP del miembro máquinas virtuales de un grupo de carga equilibrada detrás de la dirección VIP. Los DIP se asignan dentro de la infraestructura de nube a los recursos del inquilino.  
  
VIPs se encuentran en la SLB multiplexor (MUX).  La Multiplexación consta de una o varias máquinas virtuales (VM).  Controlador de red proporciona cada MUX con cada VIP y cada MUX vez utilizan Protocolo de puerta de enlace de borde (BGP) para anunciar cada VIP a enrutadores en la red física como un /32 ruta.  BGP permite a los enrutadores de red física:  
  
-   Obtén información sobre que VIP está disponible en cada MUX, incluso si la MUXes están en diferentes subredes en una red de nivel 3.  
  
-   Repartir la carga para cada VIP en todos los MUXes disponibles con enrutamiento igual costo Multi-Path (ECMP).  
  
-   Detectar un error de MUX o eliminación y dejar de enviar el tráfico a la Multiplexación erróneas automáticamente.  
  
-   Repartir la carga de la Multiplexación error o quitada en el MUXes en buen estado.  
  
Cuando llegue el tráfico público desde Internet, el MUX SLB examina el tráfico, que contiene a la dirección VIP como un destino y asigna y vuelve a escribir el tráfico para que llegará en un DIP individual. Para el tráfico de red entrante, esta transacción se realiza en un proceso de dos pasos que se divide entre los equipos MUX virtuales (VM) y el host de Hyper-V donde se encuentra el destino DIP:  
  
-   Equilibrio de carga - los usos Multiplexar la VIP para seleccionar un DIP, encapsula el paquete y reenvía el tráfico en el host de Hyper-V donde se encuentra el DIP.  
  
-   Red (NAT): la traducción de direcciones del host Hyper-V quita encapsulación el paquete, la VIP a un DIP se traduce, vuelve a asignar los puertos y reenvía el paquete a la máquina virtual de DIP.  
  
La Multiplexación sabe cómo asignar a VIPs a la correctos DIP debido a las directivas que se define usando el controlador de red de equilibrio de carga. Estas reglas incluyen protocolo, puerto front-end, el puerto de Back-end y el algoritmo de distribución (2, 3 o 5 tuplas).  
  
Cuando inquilino máquinas virtuales de responden y enviar saliente del tráfico de red a Internet o ubicaciones de inquilino remoto, porque el host de Hyper-V, el tráfico realiza NAT omite la Multiplexación y lleve directamente al enrutador de borde del host Hyper-V. Este proceso de omisión de Multiplexación se denomina devolver servidor directa (DSR).  
  
Y, después de establece el flujo de tráfico de red inicial, el tráfico de red entrantes omite el MUX SLB completamente.  
  
En la siguiente ilustración, en un equipo cliente se realiza una consulta DNS para la dirección IP de una empresa sitio de Sharepoint - en este caso, una empresa ficticia denominada Contoso. Se produce el siguiente proceso.  
  
-   El servidor DNS devuelve a la dirección VIP 107.105.47.60 al cliente.  
  
-   El cliente envía una solicitud HTTP a la dirección VIP.  
  
-   La red física tiene varias rutas de acceso disponibles para llegar a la dirección VIP ubicada en cualquier MUX.  Cada enrutador del camino usa ECMP para seleccionar el siguiente segmento de la ruta de acceso hasta que la solicitud se llega a un MUX.  
  
-   Comprueba si hay directivas configuradas la Multiplexación que recibe la solicitud y ve que hay dos DIP disponibles, 10.10.10.5 y 10.10.20.5 en una red virtual para controlar la solicitud a la dirección VIP 107.105.47.60  
  
-   La Multiplexación selecciona el DIP 10.10.10.5 y encapsula los paquetes con VXLAN para enviarlo al host que contiene el DIP con los hosts dirección física de red.  
  
-   El host recibe el paquete encapsulado y la esté inspeccionando.  Quita la encapsulación y vuelve a escribir el paquete para que el destino es ahora la 10.10.10.5 DIP en lugar de la dirección VIP y envía el tráfico a la máquina virtual de DIP.  
  
-   La solicitud ahora ha llegado el sitio de Contoso Sharepoint en 2 de granja de servidores. El servidor genera una respuesta y envía al cliente, con su propia dirección IP como el origen.  
  
-   El host intercepta el paquete de salida en el conmutador virtual que recuerda que el cliente, ahora es el destino, que se realizó la solicitud original a la dirección VIP.  El host reescribe el origen del paquete como la dirección VIP, de modo el cliente no ve la dirección DIP.  
  
-   El host reenvía el paquete directamente a la puerta de enlace predeterminada para la red física que usa su tabla de enrutamiento estándar para reenviar el paquete al cliente que finalmente se recibe la respuesta.  
  
![Proceso de equilibrio de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**La carga del tráfico de equilibrio datacenter interno**  
  
Al cargar el tráfico de red equilibrio interno en el centro de datos, como entre los recursos de inquilino que se ejecutan en servidores diferentes y son miembros de la misma red virtual, el modificador de virtuales de Hyper-V al que están conectadas las máquinas virtuales ejecuta NAT.  
  
Con tráfico interno equilibrio de carga, la primera solicitud se envía al y procesa la Multiplexación, que selecciona el DIP adecuado y lo enruta el tráfico a la DIP. Desde ese punto, el flujo de tráfico establecidos omite la Multiplexación y lleve directamente desde la máquina virtual a la máquina virtual.  
  
**Comprobaciones de estado**  
  
SLB incluye sondeos de estado para validar el estado de la infraestructura de red, incluidos los siguientes.  
  
-   Sondeo TCP al puerto  
  
-   Sondeo HTTP al puerto y dirección URL  
  
A diferencia de un dispositivo de equilibrado de carga tradicional donde el sondeo se origina en el dispositivo y se desplaza a través del cable a la DIP, el sondeo SLB se origina en el host donde lo DIP se encuentra y lleve directamente desde el agente de host SLB a la DIP, distribuir aún más el trabajo en los hosts.  
  
## <a name="bkmk_infrastructure"></a>Infraestructura de equilibrio de carga de software  
Para implementar Windows Server SLB, primero debes implementar el controlador de red en Windows Server 2016 y uno o más MUX SLB las máquinas virtuales.  
  
Además, debe configurar hosts de Hyper-V con el modificador virtuales Hyper-V habilitado SDN y asegurarse de que se está ejecutando el agente de Host SLB.  Los enrutadores que realizan los host deben admitir enrutamiento de costo igual múltiples rutas (ECMP) y protocolo de puerta de enlace de borde (BGP) y deben estar configurados para aceptar solicitudes de interconexión BGP desde el MUXes SLB.  
  
Es una visión general de la infraestructura SLB.  

![Infraestructura de equilibrio de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
Las siguientes secciones proporcionan más información sobre estos elementos de la infraestructura SLB.  
  
### <a name="scvmm"></a>SCVMM  
Con System Center 2016, puedes configurar el controlador de red en Windows Server 2016, incluidos el Administrador de SLB y supervisión de estado. También puedes usar System Center para implementar SLB MUXs e instalar a los agentes de Host SLB en equipos que ejecutan Windows Server 2016 y Hyper-V.  
  
Para obtener más información sobre System Center 2016, consulta [System Center 2016](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/).  
  
> [!NOTE]  
> Si no quieres usar System Center 2016, puedes usar Windows PowerShell u otra aplicación de administración para instalar y configurar el controlador de red y otras infraestructuras SLB. Para obtener más información, consulta [implementar controladores de red mediante Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="network-controller"></a>Controlador de red  
Controlador de red se hospeda el administrador SLB y realiza las siguientes acciones para SLB.  
  
-   Comandos SLB de procesos que lleguen a través de la API Northbound de System Center, Windows PowerShell u otra aplicación de administración de red.  
  
-   Calcula la directiva para su distribución a los hosts de Hyper-V y SLB MUXes.  
  
-   Proporciona el estado de la infraestructura SLB.  
  
### <a name="slb-mux"></a>SLB MUX  
El MUX SLB procesa el tráfico de red entrantes y asigna a VIPs DIP, reenvía el tráfico a la DIP correcto. Cada MUX también usa BGP para publicar rutas VIP en enrutadores de borde. BGP Keep Alive notifica MUXes cuando un MUX se produce un error, lo que permite MUXes activo a distribuir la carga en caso de error MUX - proporcionando equilibrio de carga para los equilibradores de carga.  
  
### <a name="hosts-that-are-running-hyper-v"></a>Hosts que se ejecutan Hyper-V  
Puedes usar SLB con equipos que ejecutan Windows Server 2016 y Hyper-V. Las máquinas virtuales en el host de Hyper-V pueden ejecutar cualquier sistema operativo que es compatible con Hyper-V.  
  
### <a name="slb-host-agent"></a>Agente de Host SLB  
Cuando implementas SLB, debes usar System Center, Windows PowerShell u otra aplicación de administración para implementar al agente de Host SLB en cada equipo host de Hyper-V. Puedes instalar al agente de Host SLB en todas las versiones de Windows Server 2016 que proporcionan soporte de Hyper-V, incluido Nano Server.  
  
El agente de Host de SLB escucha SLB las actualizaciones de directiva de controlador de red. Además, el agente de host programas reglas para SLB en los conmutadores virtuales Hyper-V habilitado SDN que están configurados en el equipo local.  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN habilitado conmutador Virtual de Hyper-V  
Para un conmutador virtual ser compatible con SLB, debes usar los comandos de administrador de Hyper-V Virtual cambiar o Windows PowerShell para crear el conmutador y, a continuación, debes habilitar la plataforma de filtrado Virtual (VFP) para el conmutador virtual.  
  
Para obtener información acerca de cómo habilitar VFP en conmutadores virtuales, ver los comandos de Windows PowerShell [Get VMSystemSwitchExtension](https://technet.microsoft.com/en-us/library/hh848603.aspx) y [habilitar VMSwitchExtension](https://technet.microsoft.com/en-us/library/hh848541.aspx?f=255&MSPPError=-2147217396).  
  
La SDN habilitado conmutador Virtual de Hyper-V realiza las siguientes acciones para SLB.  
  
-   Procesa la ruta de acceso de datos para SLB.  
  
-   Recibe el tráfico de red entrante de la Multiplexación.  
  
-   Omite la Multiplexación para el tráfico de red saliente, enviarlo al enrutador mediante DSR.  
  
-   Se ejecuta en instancias de servidor de Nano de Hyper-V.  
  
### <a name="bgp-enabled-router"></a>BGP habilitado el enrutador  
El enrutador BGP realiza las siguientes acciones para SLB.  
  
-   Tráfico entrante de rutas para la Multiplexación con ECMP.  
  
-   Para el tráfico de red saliente, usa la ruta que proporciona el host.  
  
-   Escucha las actualizaciones de ruta VIPs desde SLB MUX.  
  
-   Quita la rotación SLB SLB MUXes si se produce un error mantener activa.  
  
## <a name="bkmk_features"></a>Características de equilibrio de carga de software  
Los siguientes son algunas de las características y funcionalidades de SLB.  
  
**Funcionalidad básica**  
  
-   SLB proporciona servicios 'Norte sur' y tráfico 'Oriental oeste' TCP o UDP de equilibrio de carga de nivel 4  
  
-   Puedes usar SLB en una red basada en virtualización de la red de Hyper-V  
  
-   Puedes usar SLB con una red basada en VLAN para máquinas virtuales de DIP conectado a un SDN habilita Hyper-V conmutador Virtual.  
  
-   Una instancia SLB puede administrar varios inquilinos  
  
-   SLB y DIP admiten una ruta de retorno escalable y baja latencia, tal como se implementa por directa servidor devolver (DSR)  
  
-   Funciones SLB cuando usas conmutador incrustado agrupación (conjunto) o virtualización solo de entrada y salida de raíz (SR-IOV)  
  
-   SLB incluye el protocolo de Internet versión 4 (IPv4) admiten  
  
-   Para escenarios de puerta de enlace de sitio a sitio, SLB proporciona funcionalidad NAT para habilitar todas las conexiones de sitios para usar una dirección IP pública único  
  
-   Puedes instalar SLB, incluido el agente de Host y la Multiplexación, en Windows Server 2016, completa, Core y Nano instalar.  
  
**Escala y rendimiento**  
  
-   Listo para la escala de nube, incluida la capacidad de escalado y ajustar la escala funcionalidad para MUXes y agentes de Host.  
  
-   Un módulo de controlador de red SLB administrador activo puede admitir 8 instancias MUX  
  
**Alta disponibilidad**  
  
-   Puedes implementar SLB en más de 2 nodos en una configuración de activos  
  
-   Puede agregarse y quitarse del grupo MUX con sin afectar el servicio SLB MUXes. Esto mantiene la disponibilidad SLB cuando   
    se está revisados MUXes individuales.  
  
-   Instancias individuales de MUX tienen una actividad de 99%  
  
-   Supervisión de estado está disponible para entidades de administración  
  
**Alineación**  
  
-   Se puede implementar y configurar SLB con SCVMM  
  
-   SLB proporciona un borde unificado multiempresa integrando sin problemas con dispositivos de Microsoft como la puerta de enlace de RAS Multitenant y Datacenter Firewall espejo de la ruta.  
  

