---
title: Equilibrio de carga de software (SLB) para SDN
description: Puede usar este tema para obtener información sobre el equilibrio de carga de software para redes definidas por software en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35743d9e1a25c71a35eed018a4a3882a3d094d76
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355568"
---
# <a name="software-load-balancing-slb-for-sdn"></a>Equilibrio de carga de software \(SLB @ no__t-1 para SDN

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre el equilibrio de carga de software para redes definidas por software en Windows Server 2016.  

Los proveedores de servicios en la nube (CSP) y las empresas que implementan redes definidas por software (SDN) en Windows Server 2016 pueden usar el equilibrio de carga de software (SLB) para distribuir uniformemente el tráfico de red del inquilino y del cliente del inquilino entre los recursos de red virtual. El SLB de Windows Server permite habilitar múltiples servidores para que hospeden la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad.
  
El SLB de Windows Server incluye las siguientes funcionalidades.  
  
-   Servicios de equilibrio de carga de capa 4 (L4) para el tráfico TCP/UDP ' Norte-Sur ' y ' este de oeste '.  
  
-   Equilibrio de carga de tráfico de red interno y público.  
  
-   Admite direcciones IP dinámicas (DIP) en redes de área local virtuales (VLAN) y en redes virtuales que se crean con virtualización de red de Hyper-V.  
  
-   Compatibilidad con sondeos de estado.  
  
-   Listo para el escalado en la nube, incluida la capacidad de escalado horizontal, y capacidad de escalado vertical para multiplexores y agentes de host.  
  
Para obtener más información, consulte [características de equilibrio de carga de software](#bkmk_features) en este tema.  
  
> [!NOTE]  
> La controladora de red no admite el uso de varios inquilinos para VLAN. sin embargo, puede usar redes VLAN con SLB para cargas de trabajo administradas por el proveedor de servicios, como la infraestructura del centro de recursos y los servidores Web de alta densidad.  
  
Mediante el SLB de Windows Server, puede escalar horizontalmente las capacidades de equilibrio de carga mediante máquinas virtuales de SLB en los mismos servidores de proceso de Hyper-V que usa para las otras cargas de trabajo de máquinas virtuales. Por este motivo, SLB admite la creación rápida y la eliminación de los extremos de equilibrio de carga necesarios para las operaciones de CSP. Además, el SLB de Windows Server admite decenas de gigabytes por clúster, proporciona un modelo de aprovisionamiento simple y es fácil de escalar y reducir horizontalmente.  
  
**Cómo funciona SLB**  
  
SLB funciona mediante la asignación de direcciones IP virtuales (VIP) a direcciones IP dinámicas (DIP) que forman parte de un conjunto de recursos de servicio en la nube en el centro de trabajo.  
  
Las VIP son direcciones IP únicas que proporcionan acceso público a un grupo de máquinas virtuales con equilibrio de carga. Por ejemplo, las VIP son direcciones IP que se exponen en Internet para que los inquilinos y los clientes de inquilinos puedan conectarse a los recursos de inquilino en el centro de servicios en la nube.  
  
Las DIP son las direcciones IP de las máquinas virtuales miembro de un grupo con equilibrio de carga detrás de la VIP. Las DIP se asignan dentro de la infraestructura de la nube a los recursos del inquilino.  
  
Las VIP se encuentran en el multiplexor SLB (MUX).  El MUX se compone de una o más máquinas virtuales (VM).  La controladora de red proporciona cada MUX con cada VIP y, a su vez, cada MUX usa Protocolo de puerta de enlace de borde (BGP) para anunciar cada VIP a los enrutadores de la red física como una ruta/32.  BGP permite que los enrutadores de red físicos:  
  
-   Obtenga información acerca de que hay una VIP disponible en cada MUX, incluso si las MUX se encuentran en distintas subredes de una red de nivel 3.  
  
-   Reparta la carga de cada VIP en todas las MUX disponibles mediante el enrutamiento de múltiples rutas (ECMP) de igual costo.  
  
-   Detectar automáticamente un error o eliminación de MUX y dejar de enviar tráfico al MUX con errores.  
  
-   Reparta la carga desde el MUX con errores o eliminado en el MUX correcto.  
  
Cuando el tráfico público llega desde Internet, el MUX de SLB examina el tráfico, que contiene la dirección VIP como destino, y asigna y reescribe el tráfico para que llegue a una DIP individual. En el tráfico de red entrante, esta transacción se realiza en un proceso de dos pasos que se divide entre las máquinas virtuales de MUX y el host de Hyper-V en el que se encuentra la DIP de destino:  
  
-   Equilibrio de carga: MUX usa la VIP para seleccionar una DIP, encapsula el paquete y reenvía el tráfico al host de Hyper-V donde se encuentra la DIP.  
  
-   Traducción de direcciones de red (NAT): el host de Hyper-V quita la encapsulación del paquete, traduce la VIP a una DIP, vuelve a asignar los puertos y reenvía el paquete a la máquina virtual DIP.  
  
El MUX sabe cómo asignar VIP a los DIP correctos debido a las directivas de equilibrio de carga que se definen mediante el uso de la controladora de red. Estas reglas incluyen el protocolo, el puerto front-end, el puerto back-end y el algoritmo de distribución (5, 3 o 2 tuplas).  
  
Cuando las máquinas virtuales de inquilinos responden y envían tráfico de red saliente a las ubicaciones de Internet o de inquilinos remotos, ya que el host de Hyper-V realiza la NAT, el tráfico omite el MUX y va directamente al enrutador perimetral desde el host de Hyper-V. Este proceso de omisión de MUX se denomina Direct Server Return (DSR).  
  
Y después de establecer el flujo de tráfico de red inicial, el tráfico de red entrante omite completamente el MUX de SLB.  
  
En la ilustración siguiente, un equipo cliente realiza una consulta DNS para la dirección IP de un sitio de SharePoint de la compañía; en este caso, una empresa ficticia denominada contoso. Se produce el siguiente proceso.  
  
-   El servidor DNS devuelve el 107.105.47.60 de VIP al cliente.  
  
-   El cliente envía una solicitud HTTP a la dirección VIP.  
  
-   La red física tiene varias rutas de acceso disponibles para llegar a la dirección VIP ubicada en cualquier MUX.  Cada enrutador a lo largo del proceso usa ECMP para elegir el siguiente segmento de la ruta de acceso hasta que la solicitud llega a un MUX.  
  
-   El MUX que recibe la solicitud comprueba las directivas configuradas y observa que hay dos DIP disponibles, 10.10.10.5 y 10.10.20.5, en una red virtual para controlar la solicitud a la VIP 107.105.47.60  
  
-   El MUX selecciona la 10.10.10.5 DIP y encapsula los paquetes con VXLAN para que pueda enviarlos al host que contiene la DIP mediante la dirección de red física del host.  
  
-   El host recibe el paquete encapsulado y lo inspecciona.  Quita la encapsulación y vuelve a escribir el paquete para que el destino sea ahora el 10.10.10.5 DIP en lugar de la dirección VIP y envía el tráfico a la máquina virtual DIP.  
  
-   La solicitud ahora alcanzó el sitio de Contoso SharePoint en la granja de servidores 2. El servidor genera una respuesta y la envía al cliente, usando su propia dirección IP como origen.  
  
-   El host intercepta el paquete saliente en el conmutador virtual que recuerda que el cliente, ahora el destino, realizó la solicitud original a la VIP.  El host reescribe el origen del paquete para que sea la dirección VIP, de modo que el cliente no vea la dirección DIP.  
  
-   El host reenvía el paquete directamente a la puerta de enlace predeterminada de la red física que usa su tabla de enrutamiento estándar para reenviar el paquete al cliente que finalmente recibe la respuesta.  
  
![Proceso de equilibrio de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**Tráfico interno del centro de recursos de equilibrio de carga**  
  
Al equilibrar la carga del tráfico de red interno al centro de usuarios, por ejemplo, entre los recursos de inquilino que se ejecutan en servidores diferentes y que son miembros de la misma red virtual, el conmutador virtual de Hyper-V al que se conectan las máquinas virtuales realiza NAT.  
  
Con el equilibrio de carga de tráfico interno, el MUX envía y procesa la primera solicitud, que selecciona la DIP adecuada y enruta el tráfico a la DIP. A partir de ese momento, el flujo de tráfico establecido omite el MUX y va directamente de la máquina virtual a la máquina virtual.  
  
**Sondeos de estado**  
  
SLB incluye sondeos de estado para validar el estado de la infraestructura de red, incluido lo siguiente.  
  
-   Sondeo TCP a Puerto  
  
-   Sondeo HTTP para el puerto y la dirección URL  
  
A diferencia de un dispositivo de equilibrador de carga tradicional en el que el sondeo se origina en el dispositivo y se desplaza por la conexión a la DIP, el sondeo de SLB se origina en el host donde se encuentra la DIP y va directamente desde el agente de host de SLB a la DIP, con lo que se distribuye el trabajar en los hosts.  
  
## <a name="bkmk_infrastructure"></a>Infraestructura de equilibrio de carga de software  
Para implementar SLB de Windows Server, primero debe implementar la controladora de red en Windows Server 2016 y una o varias máquinas virtuales SLB MUX.  
  
Además, debe configurar hosts de Hyper-V con el conmutador virtual de Hyper-V habilitado para SDN y asegurarse de que se está ejecutando el agente de host de SLB.  Los enrutadores que atienden a los hosts deben ser compatibles con el enrutamiento y Protocolo de puerta de enlace de borde (BGP) de múltiples rutas de costo (ECMP) y deben configurarse para aceptar solicitudes de emparejamiento de BGP desde el MUX de SLB.  
  
A continuación se encuentra una introducción a la infraestructura de SLB.  

![Infraestructura de equilibrio de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
En las secciones siguientes se proporciona más información sobre estos elementos de la infraestructura de SLB.  
  
### <a name="scvmm"></a>SCVMM  
Con System Center 2016, puede configurar el controlador de red en Windows Server 2016, incluido el administrador de SLB y el monitor de estado. También puede usar System Center para implementar SLB MUXs e instalar agentes de host de SLB en equipos que ejecutan Windows Server 2016 e Hyper-V.  
  
Para obtener más información acerca de System Center 2016, consulte [System center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/).  
  
> [!NOTE]  
> Si no desea usar System Center 2016, puede usar Windows PowerShell u otra aplicación de administración para instalar y configurar la controladora de red y otra infraestructura de SLB. Para obtener más información, consulte implementación de la [controladora de red con Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="network-controller"></a>Controladora de red  
La controladora de red hospeda el administrador de SLB y realiza las acciones siguientes para SLB.  
  
-   Procesa comandos SLB que se incorporan a través de la API de Northbound desde System Center, Windows PowerShell u otra aplicación de administración de redes.  
  
-   Calcula la Directiva para la distribución en hosts de Hyper-V y SLB Mux.  
  
-   Proporciona el estado de mantenimiento de la infraestructura de SLB.  
  
### <a name="slb-mux"></a>MUX DE SLB  
El MUX de SLB procesa el tráfico de red entrante y asigna VIP a DIP y, a continuación, reenvía el tráfico a la DIP correcta. Cada MUX también usa BGP para publicar rutas de VIP en enrutadores perimetrales. BGP Keep Alive notifica a MUX cuando se produce un error en MUX, lo que permite a Active MUX redistribuir la carga en caso de que se produzca un error de MUX, lo que proporciona esencialmente equilibrio de carga para los equilibradores de carga.  
  
### <a name="hosts-that-are-running-hyper-v"></a>Hosts que ejecutan Hyper-V  
Puede usar SLB con equipos que ejecutan Windows Server 2016 e Hyper-V. Las máquinas virtuales del host de Hyper-V pueden ejecutar cualquier sistema operativo compatible con Hyper-V.  
  
### <a name="slb-host-agent"></a>Agente de host de SLB  
Al implementar SLB, debe usar System Center, Windows PowerShell u otra aplicación de administración para implementar el agente de host de SLB en cada equipo host de Hyper-V. Puede instalar el agente de host de SLB en todas las versiones de Windows Server 2016 que proporcionan compatibilidad con Hyper-V, incluido nano Server.  
  
El agente de host de SLB escucha las actualizaciones de directivas de SLB desde la controladora de red. Además, el agente de host de programa las reglas para SLB en los conmutadores virtuales de Hyper-V habilitados para SDN que están configurados en el equipo local.  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>Conmutador virtual de Hyper-V habilitado con SDN  
Para que un conmutador virtual sea compatible con SLB, debe usar el administrador de conmutadores virtuales de Hyper-V o los comandos de Windows PowerShell para crear el conmutador y, a continuación, debe habilitar la plataforma de filtrado virtual (VFP) para el conmutador virtual.  
  
Para obtener información sobre cómo habilitar VFP en conmutadores virtuales, vea los comandos de Windows PowerShell [Get-VMSystemSwitchExtension](https://technet.microsoft.com/library/hh848603.aspx) y [enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx?f=255&MSPPError=-2147217396).  
  
El conmutador virtual de Hyper-V habilitado con SDN realiza las siguientes acciones para SLB.  
  
-   Procesa la ruta de acceso de datos para SLB.  
  
-   Recibe el tráfico de red entrante del MUX.  
  
-   Omite el MUX para el tráfico de red saliente y lo envía al enrutador mediante DSR.  
  
-   Se ejecuta en instancias de nano Server de Hyper-V.  
  
### <a name="bgp-enabled-router"></a>Enrutador habilitado para BGP  
El enrutador BGP realiza las siguientes acciones para SLB.  
  
-   Enruta el tráfico entrante al MUX mediante ECMP.  
  
-   En el caso del tráfico de red saliente, usa la ruta proporcionada por el host.  
  
-   Escucha las actualizaciones de ruta para las direcciones VIP de SLB MUX.  
  
-   Quita SLB MUX de la rotación de SLB si se produce un error de Keep Alive.  
  
## <a name="bkmk_features"></a>Características de equilibrio de carga de software  
A continuación se muestran algunas de las características y capacidades de SLB.  
  
**Funcionalidad básica**  
  
-   SLB proporciona servicios de equilibrio de carga de nivel 4 para el tráfico TCP/UDP ' North-South ' y ' East-West '  
  
-   Puede usar SLB en una red basada en virtualización de red de Hyper-V  
  
-   Puede usar SLB con una red basada en VLAN para las máquinas virtuales DIP conectadas a un conmutador virtual de Hyper-V habilitado para SDN.  
  
-   Una instancia de SLB puede administrar varios inquilinos  
  
-   SLB y DIP admiten una ruta de acceso de devolución de baja latencia y escalable, tal y como la implementa Direct Server Return (DSR)  
  
-   SLB funciona cuando también se usa switch Embedded Teaming (SET) o virtualización de entrada/salida de raíz única (SR-IOV)  
  
-   SLB incluye compatibilidad con el protocolo de Internet versión 4 (IPv4)  
  
-   En escenarios de puerta de enlace de sitio a sitio, SLB proporciona funcionalidad NAT para permitir que todas las conexiones de sitio a sitio utilicen una única dirección IP pública.  
  
-   Puede instalar SLB, incluido el agente de host y el MUX, en Windows Server 2016, la instalación completa, básica y nano.  
  
**Escala y rendimiento**  
  
-   Listo para el escalado en la nube, incluida la capacidad de escalado horizontal, y la capacidad de escalar verticalmente para MUX y agentes de host.  
  
-   Un módulo de controladora de red de administrador SLB activo puede admitir 8 instancias de MUX  
  
**Alta disponibilidad**  
  
-   Puede implementar SLB en más de 2 nodos en una configuración activa/activa.  
  
-   MUX se puede Agregar y quitar del grupo de MUX sin que ello afecte al servicio SLB. Esto mantiene la disponibilidad de SLB cuando   
    se están revisando los MUX individuales.  
  
-   Las instancias individuales de MUX tienen un tiempo de actividad del 99%  
  
-   Los datos de supervisión de estado están disponibles para las entidades de administración  
  
**Ecuación**  
  
-   Puede implementar y configurar SLB con SCVMM  
  
-   SLB proporciona un borde unificado multiinquilino mediante la integración sin problemas con las aplicaciones de Microsoft, como la puerta de enlace multiinquilino de RAS, el Firewall de centros de seguridad y el reflector de rutas.  
  

