---
title: Equilibrio de carga de red
description: En este tema se proporciona información general sobre la característica de equilibrio de carga de red \(NLB\) en Windows Server 2016. Puede usar NLB para administrar dos o más servidores como un solo clúster virtual. NLB mejora la disponibilidad y escalabilidad de las aplicaciones de servidor de Internet, como las que se usan en Web, FTP, firewall, proxy, red privada virtual \(VPN\)y otros servidores críticos de misión\-.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 80dae16442041e3b46babaca6d163095c1c5e475
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309694"
---
# <a name="network-load-balancing"></a>Equilibrio de carga de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona información general sobre la característica de equilibrio de carga de red \(NLB\) en Windows Server 2016. Puede usar NLB para administrar dos o más servidores como un solo clúster virtual. NLB mejora la disponibilidad y escalabilidad de las aplicaciones de servidor de Internet, como las que se usan en Web, FTP, firewall, proxy, red privada virtual \(VPN\)y otros servidores críticos de misión\-.  

> [!NOTE]
> Windows Server 2016 incluye un nuevo software inspirado en Azure Load Balancer \(SLB\) como componente de las redes definidas por software \(la infraestructura de\) de SDN. Use SLB en lugar de NLB si usa SDN, si usa cargas de trabajo que no son de Windows, necesita traducción de direcciones de red salientes \(NAT\)o necesita el equilibrio de carga de nivel 3 \(\) L3 o no basado en TCP. Puede seguir usando NLB con Windows Server 2016 para las implementaciones que no son de SDN. Para obtener más información sobre SLB, consulte [equilibrio de carga de software (SLB) para Sdn](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).

La característica de equilibrio de carga de red \(NLB\) distribuye el tráfico entre varios servidores mediante el protocolo de red TCP\/IP. Mediante la combinación de dos o más equipos que ejecutan aplicaciones en un solo clúster virtual, NLB proporciona confiabilidad y rendimiento para los servidores web y otros servidores de misión\-críticos.  
  
Los servidores de un clúster NLB se denominan *hosts*y cada uno de ellos ejecuta una copia independiente de las aplicaciones de servidor. NLB distribuye las solicitudes de clientes entrantes por los hosts que forman el clúster. Se puede configurar la carga que administrará cada host. También puede agregar hosts de manera dinámica al clúster para administrar los aumentos de carga. Además, NLB puede dirigir todo el tráfico a un solo host especificado, que se denomina *host predeterminado*.  
  
NLB permite que todos los equipos del clúster se dirijan al mismo conjunto de direcciones IP y mantiene un conjunto de direcciones IP exclusivas y dedicadas para cada host. En el caso de las aplicaciones de carga\-equilibradas, cuando se produce un error en un host o se queda sin conexión, la carga se redistribuye automáticamente entre los equipos que siguen funcionando. Cuando esté listo, el equipo sin conexión puede volverse a unir de manera transparente al clúster y volver a recuperar su cuota de carga de trabajo, lo que permite a los otros equipos del clúster administrar menos tráfico.  
  
## <a name="practical-applications"></a>Aplicaciones prácticas  
NLB es útil para garantizar que las aplicaciones sin estado, como los servidores web que ejecutan Internet Information Services \(IIS\), estén disponibles con un tiempo de inactividad mínimo y sean escalables \(agregando servidores adicionales a medida que aumenta la carga\). En las secciones siguientes, se describe la manera en que NLB admite alta disponibilidad, escalabilidad y capacidad de administración de los servidores en clúster que ejecutan estas aplicaciones.  
  
### <a name="high-availability"></a>Alta disponibilidad  
Un sistema con alta disponibilidad proporciona de un modo confiable un nivel de servicio aceptable y un tiempo de inactividad mínimo. Para proporcionar alta disponibilidad, NLB incluye\-integradas en las características que pueden:  
  
-   Detectar un host de clúster que experimente un error o que se desconecte y luego recuperarlo.  
  
-   Equilibrar la carga de la red cuando se agregan o quitan hosts.  
  
-   Recuperar y redistribuir la carga de trabajo en diez segundos.  
  
### <a name="scalability"></a>Escalabilidad  
La escalabilidad cuantifica en qué grado puede un equipo, servicio o aplicación aumentar su capacidad y cubrir una mayor demanda de rendimiento. Para los clústeres NLB, es la capacidad de agregar gradualmente uno o varios sistemas a un clúster existente cuando la carga global del clúster supera sus posibilidades. Para admitir la escalabilidad, se puede hacer lo siguiente con NLB:  
  
-   Equilibre las solicitudes de carga en el clúster NLB para los servicios individuales de TCP\/IP.  
  
-   Ser compatible con hasta 32 equipos en un solo clúster.  
  
-   Equilibrar varias solicitudes de carga de servidor \(desde el mismo cliente o desde varios clientes\) en varios hosts del clúster.  
  
-   Agregar hosts al clúster NLB a medida que aumenta la carga sin que se produzca un error en el clúster.  
  
-   Quitar hosts del clúster cuando disminuye la carga.  
  
-   Habilitar el alto rendimiento y la sobrecarga baja mediante de una implementación totalmente canalizada. La canalización permite enviar las solicitudes al clúster NLB sin tener que esperar la respuesta a una solicitud anterior.  
  
### <a name="manageability"></a>Manageability  
Para admitir la capacidad de administración, se puede hacer lo siguiente con NLB:  
  
-   Administrar y configurar varios clústeres NLB y los hosts del clúster desde un solo equipo mediante el uso del administrador de NLB o los [cmdlets de equilibrio de carga de red (NLB) en Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx).
  
-   Especificar el comportamiento del equilibrio de carga para un solo puerto IP o un grupo de puertos mediante el uso de reglas de administración de puertos.  
  
-   Definir reglas de puertos diferentes para cada sitio web. Si usa el mismo conjunto de servidores con equilibrio de\-de carga para varias aplicaciones o sitios web, las reglas de puerto se basan en la dirección IP virtual de destino \(el uso de clústeres virtuales\).  

-   Dirigir todas las solicitudes de cliente a un solo host mediante el uso de reglas opcionales de host único\-. NLB enruta las solicitudes de clientes a un host concreto que ejecuta aplicaciones específicas.  

-   Bloquear el acceso de red no deseado para determinados puertos IP.  

-   Habilite el protocolo de administración de grupos de Internet \(IGMP\) compatibilidad en los hosts del clúster para controlar el desbordamiento del puerto del conmutador \(en el que los paquetes de red entrantes se envían a todos los puertos del conmutador\) al funcionar en modo de multidifusión.  

-   Iniciar, detener y controlar las acciones de NLB en forma remota mediante los comandos o scripts de Windows PowerShell.  

-   Ver el registro de eventos de Windows para comprobar si hay eventos de NLB. NLB registra en el registro de eventos todas las acciones y los cambios que se han realizado en el clúster.  

## <a name="important-functionality"></a>Funcionalidad importante  
 
NLB se instala como un componente estándar del controlador de red de Windows Server. Sus operaciones son transparentes para la pila de redes IP de TCP\/. En la siguiente ilustración se muestra la relación entre el NLB y otros componentes de software en una configuración típica.  
  
![Equilibrio de carga de red y otros componentes de software](../media/NLB/nlb.jpg)  
  
A continuación se indican las características principales de NLB.  
  
- No se requieren cambios de hardware para la ejecución.  
  
- Proporciona herramientas de equilibrio de carga de red que permiten configurar y administrar varios clústeres y todos los hosts desde un solo equipo remoto o local.  
  
- Permite a los clientes tener acceso al clúster mediante un solo nombre de Internet lógico y una dirección IP virtual, conocida como la dirección IP del clúster \(conserva los nombres individuales de cada equipo\). NLB permite varias direcciones IP virtuales para servidores de host múltiple.  
  
> [!NOTE]  
> Cuando se implementan máquinas virtuales como clústeres virtuales, NLB no requiere que los servidores sean de hosts múltiples para tener varias direcciones IP virtuales.  
  
- NLB se puede enlazar a varios adaptadores de red, lo que permite configurar varios clústeres independientes en cada host. La compatibilidad con varios adaptadores de red difiere de los clústeres virtuales en que los clústeres virtuales le permiten configurar varios clústeres en un único adaptador de red.  
  
- No es necesario realizar modificaciones en las aplicaciones de servidor para que puedan ejecutar un clúster NLB.  
  
- NLB se puede configurar para agregar de manera automática un host al clúster si se produce un error en dicho clúster y posteriormente se vuelve a conectar en línea. El host agregado puede comenzar a administrar nuevas solicitudes de servidor de los clientes.  
  
-   Permite desconectar los equipos para llevar a cabo el mantenimiento preventivo sin alterar las operaciones del clúster en los demás hosts.  
  
## <a name="hardware-requirements"></a>Requisitos de hardware  
A continuación se indican los requisitos de hardware para ejecutar un clúster NLB.  
  
-   Todos los hosts del clúster deben residir en la misma subred.  
  
-   No existe ningún tipo de limitación en cuanto al número de adaptadores de red en cada host y los hosts diferentes pueden tener un número distinto de adaptadores.  
  
-   Dentro de cada clúster, todos los adaptadores de red deben ser de unidifusión o multidifusión. NLB no admite un entorno mixto de unidifusión y multidifusión dentro de un solo clúster.  
  
-   Si usa el modo de unidifusión, el adaptador de red que se usa para administrar\-de cliente para\-tráfico del clúster debe admitir el cambio de su Media Access Control \(dirección MAC\).  
  
## <a name="software-requirements"></a>Requisitos de software  
A continuación se indican los requisitos de software para ejecutar un clúster NLB.  
  
-   Solo se puede usar TCP\/IP en el adaptador para el que está habilitado NLB en cada host. No agregue ningún otro protocolo \(por ejemplo, IPX\) a este adaptador.  
  
-   Las direcciones IP de los servidores del clúster deben ser estáticas.  
  
> [!NOTE]  
> NLB no admite el protocolo de configuración dinámica de host \(DHCP\). NLB deshabilita DHCP en cada interfaz que configura.  
  
## <a name="installation-information"></a>Información de instalación  
Puede instalar NLB mediante Administrador del servidor o los comandos de Windows PowerShell para NLB.

Opcionalmente, puede instalar las herramientas de equilibrio de carga de red para administrar un clúster NLB local o remoto. Las herramientas incluyen el administrador de equilibrio de carga de red y los comandos de Windows PowerShell de NLB.

### <a name="installation-with-server-manager"></a>Instalación con Administrador del servidor

En Administrador del servidor, puede usar el Asistente para agregar roles y características para agregar la característica de **equilibrio de carga de red** . Al completar el asistente, se instala NLB y no es necesario reiniciar el equipo.


### <a name="installation-with-windows-powershell"></a>Instalación con Windows PowerShell  

Para instalar NLB mediante Windows PowerShell, ejecute el siguiente comando en un símbolo del sistema de Windows PowerShell con privilegios elevados en el equipo en el que desea instalar NLB.

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
Una vez completada la instalación, no es necesario reiniciar el equipo.

Para obtener más información, vea [install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps).

### <a name="network-load-balancing-manager"></a>Administrador de equilibrio de carga de red
Para abrir el Administrador de equilibrio de carga de red en el Administrador del servidor, haga clic en **Herramientas** y, luego, en **Administrador de equilibrio de carga de red**.
  
## <a name="additional-resources"></a>Recursos adicionales  
En la tabla siguiente se proporcionan vínculos a información adicional acerca de la característica NLB.  
  
|Tipo de contenido|Referencias|  
|----------------|--------------|  
|Implementación|[Guía](https://technet.microsoft.com/library/cc754833(WS.10).aspx) &#124; [de implementación de equilibrio de carga de red configurar el equilibrio de carga de red con Terminal Services](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|Operaciones|[Administración de clústeres](https://technet.microsoft.com/library/cc753954(WS.10).aspx) &#124; de equilibrio de carga de red [configuración de parámetros de equilibrio](https://technet.microsoft.com/library/cc731619(WS.10).aspx) &#124; [de carga de red controlar hosts en clústeres de equilibrio de carga de red](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|Solucionar problemas|[Solución de problemas de clústeres](https://technet.microsoft.com/library/cc732592(WS.10).aspx) &#124; [de equilibrio de carga de red eventos y errores de clúster NLB](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|Herramientas y configuración|[Cmdlets de Windows PowerShell de equilibrio de carga de red](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|Recursos de la Comunidad|[Foro de\) de clústeres de \(de alta disponibilidad](https://go.microsoft.com/fwlink/p/?LinkId=230641)
