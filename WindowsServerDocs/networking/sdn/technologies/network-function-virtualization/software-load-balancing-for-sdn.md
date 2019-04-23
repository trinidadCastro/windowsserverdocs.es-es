---
title: Equilibrio de carga de software (SLB) para SDN
description: Puede utilizar este tema para obtener información sobre el equilibrio de carga de Software para redes definidas por Software en Windows Server 2016.
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
ms.openlocfilehash: 26fb4aa21e80618c4c63bd9edbf8731bf886db62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853766"
---
# <a name="software-load-balancing-slb-for-sdn"></a>Equilibrio de carga de software \(SLB\) para SDN

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para obtener información sobre el equilibrio de carga de Software para redes definidas por Software en Windows Server 2016.  

Proveedores de servicios en la nube (CSP) y las empresas que implementan definidas por Software Networking (SDN) en Windows Server 2016 pueden usar Equilibrio de carga de Software (SLB) para distribuir uniformemente tráfico de red de cliente de inquilino entre recursos de red virtual y del inquilino. El SLB de Windows Server permite habilitar múltiples servidores para que hospeden la misma carga de trabajo, lo que proporciona alta disponibilidad y escalabilidad.
  
SLB de Windows Server incluye las siguientes capacidades.  
  
-   Para el tráfico TCP/UDP de 'Este-oeste' y 'Norte-sur' servicios de equilibrio de carga de nivel 4 (L4).  
  
-   Públicos e internos equilibrio de carga de tráfico de red.  
  
-   En redes de área Local virtual (VLAN) y en redes virtuales que cree mediante el uso de virtualización de red de Hyper-V, es compatible con las direcciones IP dinámicas (DIP).  
  
-   Soporte técnico de sondeo de estado.  
  
-   Está listo para la nube, incluida la capacidad de escalabilidad horizontal y escalar verticalmente la capacidad para multiplexores y agentes de Host.  
  
Para obtener más información, consulte [características de equilibrio de carga de Software](#bkmk_features) en este tema.  
  
> [!NOTE]  
> Multiempresa para VLAN no es compatible con la controladora de red, pero puede usar VLAN con SLB para el proveedor de servicios administrados cargas de trabajo, como la infraestructura de centro de datos y los servidores Web de alta densidad.  
  
Con Windows Server SLB, puede escalar horizontalmente su capacidades con máquinas virtuales de SLB en los mismos servidores de proceso de Hyper-V que usan para las otras cargas de trabajo de máquina virtual de equilibrio de carga. Por este motivo, SLB admite la rápida creación y eliminación de puntos de conexión de equilibrio de carga que se requiere para las operaciones de CSP. Además, SLB de Windows Server es compatible con decenas de gigabytes por clúster, proporciona un sencillo modelo de aprovisionamiento y es fácil de escalado horizontal y reducción.  
  
**Cómo funciona el SLB**  
  
SLB funciona mediante la asignación de direcciones IP virtuales (VIP) a las direcciones IP dinámicas (DIP) que forman parte de un conjunto de servicios en la nube de recursos en el centro de datos.  
  
Las direcciones VIP son direcciones IP individuales que proporcionan acceso público a un grupo de carga equilibrada de las máquinas virtuales. Por ejemplo, las direcciones VIP son las direcciones IP que se exponen en Internet para que los inquilinos y los clientes del inquilino pueden conectarse a los recursos del inquilino en el centro de datos en la nube.  
  
DIP son las direcciones IP de las máquinas virtuales de un grupo con equilibrio de carga detrás de la dirección VIP de miembro. DIP se asignan dentro de la infraestructura de nube a los recursos del inquilino.  
  
Direcciones IP virtuales se encuentran en el SLB multiplexor (MUX).  El MUX consta de uno o más máquinas virtuales (VM).  Controladora de red ofrece cada MUX con cada VIP y cada MUX a su vez utilizan Border Gateway Protocol (BGP) para anunciar cada VIP a los enrutadores de la red física como un/32 rutas.  BGP permite que los enrutadores de la red física:  
  
-   Obtenga información sobre el que una dirección VIP está disponible en cada MUX, incluso si el MUX está en subredes diferentes en una red de capa 3.  
  
-   Distribuir la carga para cada VIP entre todos los MUX disponibles de uso del enrutamiento multidireccional de igual costo (ECMP).  
  
-   Detectar un error MUX o la eliminación y automáticamente dejará de enviarle tráfico del MUX con errores.  
  
-   Distribuir la carga desde el MUX con errores o eliminado entre la MUX en buen estado.  
  
Cuando llegue tráfico público desde Internet, SLB MUX examina el tráfico, que contiene a la dirección VIP como un destino y se asigna y vuelve a escribir el tráfico para que llegará a una DIP de individual. Para el tráfico de red entrante, esta transacción se realiza en un proceso en dos pasos que se divide entre las máquinas virtuales (VM) de MUX y el host de Hyper-V donde se encuentra la DIP de destino:  
  
-   Equilibrio de carga - los usos MUX la dirección VIP para seleccionar una DIP, encapsula el paquete y reenvía el tráfico al host de Hyper-V donde se encuentra la DIP.  
  
-   (NAT): la traducción de direcciones de red del host de Hyper-V quita la encapsulación de los paquetes, traduce la dirección VIP a una DIP, vuelve a asignar los puertos y reenvía el paquete a la máquina virtual de DIP.  
  
El MUX sabe cómo asignar a las VIP a la DIP correcto debido a las directivas que definen mediante el uso de la controladora de red de equilibrio de carga. Estas reglas incluyen el protocolo, puerto de front-end, puerto de Back-end y el algoritmo de distribución (2, 3 o 5 tuplas).  
  
Al responden las máquinas virtuales de inquilino y de envío saliente tráfico de red a Internet o ubicaciones de inquilinos remotos, porque la NAT se realiza por el host de Hyper-V, el tráfico omite el MUX y va directamente al enrutador perimetral desde el host de Hyper-V. Este proceso de omisión MUX se denomina Direct Server Return (DSR).  
  
Y después de establece el flujo de tráfico de red inicial, el tráfico de red entrante omite completamente el SLB MUX.  
  
En la siguiente ilustración, un equipo cliente realiza una consulta DNS para la dirección IP de una empresa sitio de SharePoint: en este caso, una compañía ficticia denominada Contoso. Se produce el siguiente proceso.  
  
-   El servidor DNS devuelve a la dirección VIP 107.105.47.60 al cliente.  
  
-   El cliente envía una solicitud HTTP a la dirección VIP.  
  
-   La red física tiene varias rutas de acceso disponibles para llegar a la dirección VIP ubicada en cualquier MUX.  Cada enrutador del recorrido usa ECMP para elegir el siguiente segmento de la ruta de acceso hasta que la solicitud llega a un MUX.  
  
-   Comprueba las directivas configuradas del MUX que recibe la solicitud y ve que hay dos DIP disponibles, 10.10.10.5 y 10.10.20.5 en una red virtual para atender la solicitud a la dirección VIP 107.105.47.60  
  
-   El MUX selecciona la DIP 10.10.10.5 y encapsula los paquetes mediante VXLAN por lo que puede enviarla al host que contiene la DIP mediante hosts de la dirección de red física.  
  
-   El host recibe el paquete encapsulado e inspecciona.  Quita la encapsulación y vuelve a escribir el paquete para que el destino es ahora la DIP 10.10.10.5 en lugar de la dirección VIP y envía el tráfico a la máquina virtual de DIP.  
  
-   La solicitud ahora ha alcanzado el sitio de Contoso SharePoint en el servidor 2 de granja de servidores. El servidor genera una respuesta y lo envía al cliente, con su propia dirección IP como el origen.  
  
-   El host intercepta el paquete de salida en el conmutador virtual que le recuerda que el cliente, ahora es el destino, que se realizó la solicitud original a la dirección VIP.  El host vuelve a escribir el origen del paquete como la dirección VIP, de modo al cliente no ve la dirección DIP.  
  
-   El host reenvía el paquete directamente a la puerta de enlace predeterminada para la red física que usa su tabla de enrutamiento estándar para reenviar el paquete de la sesión en el cliente que finalmente recibe la respuesta.  
  
![Proceso de equilibrio de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**La carga del tráfico de equilibrio internas del centro de datos**  
  
Cuando la carga del tráfico de red equilibrio interno en el centro de datos, como entre los recursos de inquilino que se ejecutan en servidores diferentes y son miembros de la misma red virtual, el conmutador Virtual de Hyper-V al que están conectadas las máquinas virtuales lleva a cabo NAT.  
  
Con equilibrio de carga de tráfico interno, la primera solicitud se envía al y procesó el MUX, que selecciona la DIP adecuada y enruta el tráfico a la DIP. Desde ese punto, el flujo de tráfico establecidos omite el MUX y va directamente desde la máquina virtual a máquina virtual.  
  
**Sondeos de estado**  
  
SLB incluye los sondeos de mantenimiento para validar el estado de la infraestructura de red, incluidos los siguientes.  
  
-   Sondeo TCP al puerto  
  
-   Sondeo HTTP al puerto y dirección URL  
  
A diferencia de un dispositivo de equilibrador de carga tradicional donde el sondeo se origina en el dispositivo y viaja a través de la conexión a la DIP, el sondeo SLB se origina en el host donde la DIP se encuentra y va directamente desde el agente de host SLB a la DIP, distribuir aún más el funcionan en los hosts.  
  
## <a name="bkmk_infrastructure"></a>Infraestructura de equilibrio de carga de software  
Para implementar SLB de Windows Server, primero debe implementar la controladora de red en Windows Server 2016 y máquinas virtuales MUX de SLB de uno o más.  
  
Además, debe configurar hosts de Hyper-V con el conmutador Virtual de Hyper-V habilitado SDN y asegúrese de que se está ejecutando el agente de Host de SLB.  Los enrutadores que sirven a los hosts deben admitir el enrutamiento de igual costo (ECMP) de múltiples rutas y Border Gateway Protocol (BGP) y deben estar configurados para aceptar solicitudes de emparejamiento de BGP desde el SLB/MUX.  
  
Es una visión general de la infraestructura de SLB.  

![Infraestructura de equilibrio de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
Las secciones siguientes proporcionan más información sobre estos elementos de la infraestructura de SLB.  
  
### <a name="scvmm"></a>SCVMM  
Con System Center 2016, puede configurar la controladora de red en Windows Server 2016, incluido el Monitor de estado y el Administrador de SLB. También puede usar System Center para implementar SLB MUXs e instalar a los agentes de Host SLB en equipos que ejecutan Hyper-V y Windows Server 2016.  
  
Para obtener más información sobre System Center 2016, consulte [System Center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/).  
  
> [!NOTE]  
> Si no desea usar System Center 2016, puede usar Windows PowerShell u otra aplicación de administración para instalar y configurar la controladora de red y otras infraestructuras SLB. Para obtener más información, consulte [implementar controladora de red mediante Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="network-controller"></a>Controladora de red  
Controladora de red se hospeda el Administrador de SLB y realiza las siguientes acciones para SLB.  
  
-   Procesa los comandos SLB que lleguen a través de la API Northbound de System Center, Windows PowerShell u otra aplicación de administración de red.  
  
-   Calcula la directiva para su distribución a los hosts de Hyper-V y SLB/MUX.  
  
-   Proporciona el estado de mantenimiento de la infraestructura de SLB.  
  
### <a name="slb-mux"></a>SLB/MUX  
SLB MUX procesa el tráfico de red entrante y asigna a las direcciones VIP a DIP, a continuación, se reenvía el tráfico a la DIP correcta. Cada MUX también utiliza BGP para publicar las rutas de dirección VIP a enrutadores de borde. BGP Keep Alive notifica MUX cuando falla una MUX, lo que permite MUX de activo a distribuir la carga en caso de error MUX - proporcionando equilibrio de carga para los equilibradores de carga.  
  
### <a name="hosts-that-are-running-hyper-v"></a>Hosts que ejecutan Hyper-V  
Puede usar SLB con equipos que ejecutan Hyper-V y Windows Server 2016. Las máquinas virtuales en el host de Hyper-V pueden ejecutar cualquier sistema operativo que sea compatible con Hyper-V.  
  
### <a name="slb-host-agent"></a>Agente de Host SLB  
Al implementar SLB, debe usar System Center, Windows PowerShell u otra aplicación de administración para implementar al agente de Host SLB en cada equipo host de Hyper-V. Puede instalar al agente de Host SLB en todas las versiones de Windows Server 2016 que proporcionan compatibilidad de Hyper-V, incluidas Nano Server.  
  
El agente de Host SLB realiza escuchas para las actualizaciones de directiva SLB de controladora de red. Además, el agente de host programas reglas para SLB en los conmutadores virtuales Hyper-V habilitado en SDN que están configurados en el equipo local.  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN habilitado conmutador Virtual de Hyper-V  
Para un conmutador virtual para que sea compatible con el SLB, debe usar los comandos del Administrador de conmutadores virtuales de Hyper-V o Windows PowerShell para crear el conmutador y, a continuación, debe habilitar la plataforma de filtrado Virtual (VFP) para el conmutador virtual.  
  
Para obtener información sobre cómo habilitar VFP en conmutadores virtuales, vea los comandos de Windows PowerShell [Get-VMSystemSwitchExtension](https://technet.microsoft.com/library/hh848603.aspx) y [Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx?f=255&MSPPError=-2147217396).  
  
SDN habilitado Hyper-V Virtual Switch realiza las siguientes acciones para SLB.  
  
-   Procesa la ruta de acceso de datos para el SLB.  
  
-   Recibe el tráfico de red entrante desde el MUX.  
  
-   Omite el MUX para el tráfico de red saliente, enviarlo al enrutador con DSR.  
  
-   Se ejecuta en instancias de servidor Nano de Hyper-V.  
  
### <a name="bgp-enabled-router"></a>BGP habilitado enrutador  
El enrutador BGP realiza las siguientes acciones para SLB.  
  
-   Enruta el tráfico entrante para el MUX con ECMP.  
  
-   Para el tráfico de red saliente, usa la ruta proporcionada por el host.  
  
-   Escucha las actualizaciones de ruta para VIP de SLB MUX.  
  
-   Quita el SLB/MUX de la rotación de SLB si se produce un error Keep Alive.  
  
## <a name="bkmk_features"></a>Características de equilibrio de carga de software  
Estos son algunas de las características y capacidades de SLB.  
  
**Funcionalidad básica**  
  
-   SLB proporciona servicios para el tráfico TCP/UDP de 'Este-oeste' y 'Norte-sur' de equilibrio de carga de nivel 4  
  
-   Puede usar SLB en una red basada en virtualización de red de Hyper-V  
  
-   Puede usar SLB con una red basada en VLAN para máquinas virtuales de DIP conectadas a una SDN habilitado Hyper-V Virtual Switch.  
  
-   Una instancia SLB puede administrar varios inquilinos  
  
-   SLB y DIP admiten una ruta de retorno escalable y de baja latencia, tal como se implementa mediante Direct Server Return (DSR)  
  
-   Funciones SLB cuando usas Switch Embedded Teaming (SET) o virtualización de entrada/salida de raíz única (SR-IOV)  
  
-   SLB incluye el protocolo de Internet versión 4 (IPv4) admiten  
  
-   Para escenarios de puerta de enlace de sitio a sitio, SLB proporciona funcionalidad NAT para habilitar todas las conexiones de sitio a sitio utilizar una sola dirección IP pública  
  
-   Puede instalar SLB, incluido el agente de Host y MUX, en Windows Server 2016, completa, Core y Nano Install.  
  
**Escala y rendimiento**  
  
-   Está listo para la nube, incluida la capacidad de escalabilidad horizontal y escalar verticalmente la capacidad para MUX y agentes de Host.  
  
-   Un módulo de controlador de red de administrador de SLB activo puede admitir 8 instancias MUX  
  
**Alta disponibilidad**  
  
-   Puede implementar SLB a más de 2 nodos en una configuración activa/activa  
  
-   Se puede agregar y quita del grupo MUX sin afectar el servicio SLB MUX. Esto mantiene la disponibilidad SLB cuando   
    MUX individuales es que se va a revisar.  
  
-   Las instancias individuales de MUX tienen un tiempo de actividad del 99%  
  
-   Datos de supervisión de estado está disponible para entidades de administración  
  
**Alineación**  
  
-   Puede implementar y configurar el SLB con SCVMM  
  
-   SLB proporciona un borde unificado para varios inquilinos mediante la integración sin problemas con dispositivos de Microsoft, como la puerta de enlace de RAS para varios inquilinos, Firewall de centro de datos y ruta Reflector.  
  

