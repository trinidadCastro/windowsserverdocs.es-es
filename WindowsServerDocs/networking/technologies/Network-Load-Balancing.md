---
title: Equilibrio de carga de red
description: Este tema proporciona una descripción general de la característica de equilibrio de carga de red (NLB) en Windows Server 2016 e incluye vínculos a instrucciones adicionales sobre cómo crear, configurar y administrar clústeres NLB.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b9fd39381316a8bcd06328e7aa75492ed99bc7f1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-load-balancing"></a>Equilibrio de carga de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema proporciona una descripción general de la característica de equilibrio de carga de red \(NLB\) en Windows Server 2016 e incluye vínculos a instrucciones adicionales sobre cómo crear, configurar y administrar clústeres NLB.

Puedes usar NLB para administrar los servidores de dos o más como un solo clúster virtual. NLB mejora la disponibilidad y la escalabilidad de aplicaciones de servidor de Internet, como las que se usan en la web, FTP, firewall, proxy, privada virtual \(VPN\) y otros servidores mission\ críticas de la red.   

>[!NOTE]
>Windows Server 2016 incluye un nuevo Azure inspirado equilibrado de carga de Software \(SLB\) como un componente de la infraestructura \(SDN\) Software definido de redes. Usa SLB en lugar de NLB si estás usando SDN, estás usando las cargas de trabajo no son de Windows, necesitas la traducción de direcciones de red saliente \(NAT\), o necesitas \(L3\) Layer 3 o equilibrio de carga no TCP en función. Aún puedes usar NLB con Windows Server 2016 para implementaciones de no SDN. Para obtener más información sobre SLB, consulta [equilibrio de carga de Software (SLB) para SDN](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).

La característica de equilibrio de carga de red \(NLB\) distribuye el tráfico entre varios servidores mediante el protocolo de redes TCP/IP TCP\. Al combinar dos o más equipos que ejecutan aplicaciones en un único clúster virtual, NLB proporciona confiabilidad y rendimiento de los servidores web y otros servidores mission\ crítica.  
  
Los servidores de un clúster NLB se denominan *hosts*, y cada host ejecuta una copia independiente de las aplicaciones de servidor. NLB distribuye las solicitudes entrantes de cliente en los hosts del clúster. Puedes configurar la carga que se pueden controlar mediante cada host. También puedes agregar hosts dinámicamente al clúster para controlar el aumento de la carga. NLB también puede dirigir todo el tráfico a un único host designado, que se llama la *host predeterminado*.  
  
NLB permite a todos los equipos del clúster puede alcanzarse mediante el mismo conjunto de direcciones IP y mantiene un conjunto de direcciones IP dedicadas únicas para cada host. Aplicaciones con equilibrio load\, cuando un host se produce un error o se desconecta, la carga se redistribuye automáticamente entre los equipos que siguen operativos. Cuando esté listo, el equipo sin conexión puede a unir el clúster y retomar su parte de la carga de trabajo, que permite a los demás equipos del clúster administrar menos tráfico de forma transparente.  
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
NLB es útil para garantizar que las aplicaciones sin estado, como los servidores web de Internet Information Services \(IIS\), están disponibles con el tiempo de inactividad mínimo y que son escalables \ (agregando servidores adicionales como la increases\ de carga). Las siguientes secciones describen cómo NLB admite alta disponibilidad, escalabilidad y facilidad de uso de los servidores agrupados que se ejecutan estas aplicaciones.  
  
### <a name="high-availability"></a>Alta disponibilidad  
Un sistema de alta disponibilidad confiable proporciona un nivel de servicio con el tiempo de inactividad mínimo aceptable. Para ofrecer una alta disponibilidad, NLB incluye built\ en características que pueden automáticamente:  
  
-   Detectar un host del clúster que se produce un error o se desconecta y, a continuación, recuperar.  
  
-   Equilibrar la carga de red cuando se agregan o eliminan hosts.  
  
-   Recuperar y redistribuir la carga de trabajo en diez segundos.  
  
### <a name="scalability"></a>Escalabilidad  
La escalabilidad es la medida de lo bien un equipo, servicio o aplicación puede crecer para cumplir las demandas de rendimiento. NLB clústeres, escalabilidad es la capacidad de agregar gradualmente uno o varios sistemas a un clúster existente cuando la carga global del clúster supera sus capacidades. Para admitir la escalabilidad, puedes hacer lo siguiente con NLB:  
  
-   Equilibrar las solicitudes de carga en el clúster NLB para los servicios de TCP/IP TCP\ individuales.  
  
-   Admitir hasta 32 equipos en un único clúster.  
  
-   Equilibrar varias solicitudes de carga del servidor \ (desde el mismo cliente o de varios clients\) en varios hosts del clúster.  
  
-   Agregar los hosts del clúster NLB medida que aumenta la carga, sin provocar el clúster un error.  
  
-   Quitar los hosts del clúster cuando se reduce la carga.  
  
-   Habilitar alto rendimiento y baja sobrecarga a través de una implementación totalmente canalizada. La canalización permite que las solicitudes que se enviará al clúster NLB sin tener que esperar una respuesta a una solicitud anterior.  
  
### <a name="manageability"></a>Facilidad de uso  
Para admitir la facilidad de uso, puedes hacer lo siguiente con NLB:  
  
-   Administrar y configurar varios clústeres NLB y los hosts del clúster desde un único equipo mediante el administrador o el [Cmdlets de equilibrio de carga de red (NLB) en Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx).
  
-   Especificar la carga de equilibrio de comportamiento de un solo puerto IP o grupo de puertos mediante el uso de reglas de administración de puerto.  
  
-   Definir reglas de puerto diferente para cada sitio Web. Si usas el mismo conjunto de servidores de equilibrio de load\ para varias aplicaciones o sitios Web, las reglas de puerto se basan en la dirección IP de destino virtual \(using virtual clusters\).  

-   Dirigir todas las solicitudes de cliente a un único host usando opcional, las reglas single\ de host. NLB redirige las solicitudes de cliente a un determinado host que ejecuta aplicaciones específicas.  

-   Bloquear el acceso de red no deseado a ciertos puertos IP.  

-   Habilitar la compatibilidad con el protocolo de administración de grupos de Internet \(IGMP\) en los hosts del clúster para controlar los puertos de conmutación \ (donde los paquetes de red entrantes se envían a todos los puertos en el switch\) al operar en modo de multidifusión.  

-   Iniciar, detener y controlar las acciones de NLB de forma remota mediante comandos de Windows PowerShell o scripts.  

-   Ver el registro de eventos de Windows para comprobar los eventos de NLB. NLB registra todos los cambios de clúster y las acciones en el registro de eventos.  

## <a name="important-functionality"></a>Funcionalidad importante  
 
NLB se instala como un componente estándar de controlador de red de Windows Server. Sus operaciones son transparentes para la pila de red TCP/IP TCP\. La siguiente figura muestra la relación entre NLB y otros componentes de software en una configuración típica.  
  
![Equilibrio de carga de red y otros componentes de software](../media/NLB/nlb.jpg)  
  
A continuación es las características principales de NLB.  
  
- Requiere ningún cambio de hardware para ejecutarse.  
  
- Proporciona las herramientas de equilibrio de carga de red para configurar y administrar varios clústeres y todos los hosts desde un único equipo local o remoto.  
  
- Permite a los clientes acceso al clúster mediante un único nombre lógico de Internet y la dirección IP virtual, que se conoce como la dirección IP \ (conserva los nombres individuales para cada computer\). NLB permite varias direcciones IP virtuales para los servidores.  
  
> [!NOTE]  
> Cuando implementas máquinas virtuales como clústeres virtuales, NLB no requiere servidores de host múltiple para tener varias direcciones IP virtuales.  
  
- Permite NLB enlazar varios adaptadores de red, lo que permite configurar varios clústeres independientes en cada host. Compatibilidad con varios adaptadores de red difiere de clústeres virtuales en que clústeres virtuales permiten configurar varios clústeres en un adaptador de red.  
  
- No requiere modificaciones en aplicaciones de servidor para que se pueden ejecutar en un clúster NLB.  
  
- Puede configurarse para agregar automáticamente un host al clúster si se produce un error de ese host del clúster y posteriormente nuevamente en línea. El host agregado puede iniciar el control de nuevas solicitudes de servidor de los clientes.  
  
-   Te permite realizar equipos sin conexión para mantenimiento preventivo sin afectar a las operaciones del clúster en los otros hosts.  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
Siguiente es los requisitos de hardware para ejecutar un clúster NLB.  
  
-   Todos los hosts del clúster deben residir en la misma subred.  
  
-   No está limitada en el número de adaptadores de red en cada host y hosts diferentes pueden tener un número diferente de adaptadores.  
  
-   En cada clúster, todos los adaptadores de red deben ser unidifusión o multidifusión. NLB no admite un entorno de multidifusión y unidifusión dentro de un único clúster.  
  
-   Si usas el modo de unidifusión, debe admitir el adaptador de red que se usa para controlar el tráfico client\ to\-clúster cambiar su dirección de media access control \(MAC\).  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Siguiente es los requisitos de software para ejecutar un clúster NLB.  
  
-   TCP\/IP solo puede usarse en el adaptador para el que está habilitado NLB en cada host. No agregues ningún otro protocolo \ (por ejemplo, IPX\) a este adaptador.  
  
-   Las direcciones IP de los servidores del clúster deben ser estáticas.  
  
> [!NOTE]  
> NLB no admite el protocolo de configuración dinámica de Host \(DHCP\). NLB deshabilita DHCP en cada interfaz que se configura.  
  
## <a name="BKMK_INSTALL"></a>Información sobre la instalación  
Puede instalar NLB mediante el administrador del servidor o los comandos de Windows PowerShell para NLB.

Opcionalmente, puedes instalar las herramientas de equilibrio de carga de red para administrar un clúster NLB local o remoto. Las herramientas incluyen el Administrador de equilibrio de carga de red y los comandos de NLB Windows PowerShell.

### <a name="installation-with-server-manager"></a>Instalación con el administrador del servidor

En el administrador del servidor, puedes usar el agregar Roles y características de Asistente para agregar la **equilibrio de carga de red** característica. Cuando completes el asistente, NLB está instalado y no es necesario reiniciar el equipo.


### <a name="installation-with-windows-powershell"></a>Instalación de Windows PowerShell  

Para instalar NLB mediante Windows PowerShell, ejecuta el siguiente comando en un símbolo del sistema con privilegios elevados de Windows PowerShell en el equipo donde desea instalar NLB.

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
Una vez completada la instalación, no es necesario reiniciar el equipo.

Para obtener más información, consulta [Install-WindowsFeature](https://technet.microsoft.com/library/jj205467.aspx).

### <a name="network-load-balancing-manager"></a>Administrador de equilibrio de carga de red
Para abrir el Administrador de equilibrio de carga de red en el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **Administrador de equilibrio de carga de red**.
  
## <a name="BKMK_LINKS"></a>Recursos adicionales  
La siguiente tabla proporciona vínculos a información adicional sobre la característica NLB.  
  
|Tipo de contenido|Referencias|  
|----------------|--------------|  
|Implementación|[Guía de implementación de equilibrio de carga de red](https://technet.microsoft.com/library/cc754833(WS.10).aspx) & #124; [Configurar con Terminal Services de equilibrio de carga de red](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|Operaciones|[Administrar clústeres de equilibrio de carga de red](https://technet.microsoft.com/library/cc753954(WS.10).aspx) & #124; [Establecer los parámetros de equilibrio de carga de red](https://technet.microsoft.com/library/cc731619(WS.10).aspx) & #124; [Controlar Hosts de clústeres de equilibrio de carga de red](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|Solución de problemas|[Solución de problemas de clústeres de equilibrio de carga de red](https://technet.microsoft.com/library/cc732592(WS.10).aspx) & #124; [NLB clúster eventos y errores](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|Herramientas y opciones de configuración|[Cmdlets de Windows PowerShell de equilibrio de carga de red](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|Recursos de la Comunidad|[Foro de \(Clustering\) alta disponibilidad](https://go.microsoft.com/fwlink/p/?LinkId=230641)