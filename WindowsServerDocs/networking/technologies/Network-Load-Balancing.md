---
title: Equilibrio de carga de red
description: En este tema, le proporcionaremos información general sobre el equilibrio de carga de red \(NLB\) característica de Windows Server 2016. Puede usar NLB para administrar dos o más servidores como un solo clúster virtual. NLB mejora la disponibilidad y escalabilidad de las aplicaciones de servidor de Internet tales como las usadas en web, FTP, firewall, proxy, redes privadas virtuales \(VPN\)y otro misión\-servidores críticos.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 0ea129fe2230332c0099d735f064768bce9fc50c
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812277"
---
# <a name="network-load-balancing"></a>Equilibrio de carga de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, le proporcionaremos información general sobre el equilibrio de carga de red \(NLB\) característica de Windows Server 2016. Puede usar NLB para administrar dos o más servidores como un solo clúster virtual. NLB mejora la disponibilidad y escalabilidad de las aplicaciones de servidor de Internet tales como las usadas en web, FTP, firewall, proxy, redes privadas virtuales \(VPN\)y otro misión\-servidores críticos.  

> [!NOTE]
> Windows Server 2016 incluye un nuevo equilibrador de carga de Software inspiradas en Azure \(SLB\) como un componente de las redes de definidas por Software \(SDN\) infraestructura. Use SLB en lugar de NLB si usas SDN, usan las cargas de trabajo que no sean Windows, necesita la traducción de direcciones de red saliente \(NAT\), o necesita capa 3 \(L3\) o equilibrio de carga basadas en que no es TCP. Aún puede usar NLB con Windows Server 2016 para las implementaciones que no son de SDN. Para obtener más información acerca de SLB, consulte [equilibrio de carga de Software (SLB) para SDN](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).

El equilibrio de carga de red \(NLB\) característica distribuye el tráfico entre varios servidores mediante el uso de TCP\/protocolo de red IP. Al combinar dos o más equipos que ejecutan aplicaciones en un solo clúster virtual, NLB ofrece confiabilidad y rendimiento para servidores web y otro misión\-servidores críticos.  
  
Los servidores de un clúster NLB se denominan *hosts*y cada uno de ellos ejecuta una copia independiente de las aplicaciones de servidor. NLB distribuye las solicitudes de cliente entrantes entre los hosts que forman el clúster. Se puede configurar la carga que administrará cada host. También se pueden agregar hosts de manera dinámica al clúster para administrar los aumentos de carga. Además, NLB puede dirigir todo el tráfico a un solo host especificado, que se denomina *host predeterminado*.  
  
NLB permite que todos los equipos del clúster se dirijan al mismo conjunto de direcciones IP y mantiene un conjunto de direcciones IP exclusivas y dedicadas para cada host. Para la carga\-con equilibrio de las aplicaciones, cuando se produce un error de un host o se queda sin conexión, la carga se redistribuye automáticamente entre los equipos que siguen funcionando. Cuando esté listo, el equipo sin conexión puede volverse a unir de manera transparente al clúster y volver a recuperar su cuota de carga de trabajo, lo que permite a los otros equipos del clúster administrar menos tráfico.  
  
## <a name="practical-applications"></a>Aplicaciones prácticas  
NLB es útil para garantizar que sin estado de las aplicaciones, como servidores web que ejecutan Internet Information Services \(IIS\), están disponibles con el tiempo de inactividad mínimo y sean escalables \(agregando servidores adicionales como aumenta la carga\). En las secciones siguientes, se describe la manera en que NLB admite alta disponibilidad, escalabilidad y capacidad de administración de los servidores en clúster que ejecutan estas aplicaciones.  
  
### <a name="high-availability"></a>Alta disponibilidad  
Un sistema con alta disponibilidad proporciona de un modo confiable un nivel de servicio aceptable y un tiempo de inactividad mínimo. Para proporcionar alta disponibilidad, NLB incluye creado\-en las características que pueden automáticamente:  
  
-   Detectar un host de clúster que experimente un error o que se desconecte y luego recuperarlo.  
  
-   Equilibrar la carga de la red cuando se agregan o quitan hosts.  
  
-   Recuperar y redistribuir la carga de trabajo en diez segundos.  
  
### <a name="scalability"></a>Escalabilidad  
La escalabilidad cuantifica en qué grado puede un equipo, servicio o aplicación aumentar su capacidad y cubrir una mayor demanda de rendimiento. Para los clústeres NLB, es la capacidad de agregar gradualmente uno o varios sistemas a un clúster existente cuando la carga global del clúster supera sus posibilidades. Para admitir la escalabilidad, se puede hacer lo siguiente con NLB:  
  
-   Equilibrar las solicitudes de carga en el clúster de NLB para TCP individual\/servicios IP.  
  
-   Ser compatible con hasta 32 equipos en un solo clúster.  
  
-   Equilibrar varias solicitudes de carga de servidor \(del mismo cliente o desde varios clientes\) en varios hosts del clúster.  
  
-   Agregar hosts al clúster NLB a medida que aumenta la carga sin que se produzca un error en el clúster.  
  
-   Quitar hosts del clúster cuando disminuye la carga.  
  
-   Habilitar el alto rendimiento y la sobrecarga baja mediante de una implementación totalmente canalizada. La canalización permite enviar las solicitudes al clúster NLB sin tener que esperar la respuesta a una solicitud anterior.  
  
### <a name="manageability"></a>Manageability  
Para admitir la capacidad de administración, se puede hacer lo siguiente con NLB:  
  
-   Administrar y configurar varios clústeres NLB y los hosts del clúster desde un solo equipo mediante el Administrador de NLB o el [Cmdlets de equilibrio de carga de red (NLB) en Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx).
  
-   Especificar el comportamiento del equilibrio de carga para un solo puerto IP o un grupo de puertos mediante el uso de reglas de administración de puertos.  
  
-   Definir reglas de puertos diferentes para cada sitio web. Si usa el mismo conjunto de carga\-servidores con equilibrio de varias aplicaciones o sitios Web, las reglas de puerto se basan en la dirección IP virtual de destino \(mediante clústeres virtuales\).  

-   Dirigir todas las solicitudes de cliente a un solo host mediante el uso opcional, una sola\-hospedar las reglas. NLB enruta las solicitudes de clientes a un host concreto que ejecuta aplicaciones específicas.  

-   Bloquear el acceso de red no deseado para determinados puertos IP.  

-   Habilitar el protocolo de administración de grupos de Internet \(IGMP\) soporte técnico en los hosts del clúster para controlar el desborde de puerto \(donde se envían los paquetes de red entrantes a todos los puertos del conmutador\) cuando se trabaja en modo de multidifusión.  

-   Iniciar, detener y controlar las acciones de NLB en forma remota mediante los comandos o scripts de Windows PowerShell.  

-   Ver el registro de eventos de Windows para comprobar si hay eventos de NLB. NLB registra en el registro de eventos todas las acciones y los cambios que se han realizado en el clúster.  

## <a name="important-functionality"></a>Funcionalidad importante  
 
NLB se instala como un componente estándar de controlador de red de Windows Server. Sus operaciones son transparentes para TCP\/pila de redes IP. En la siguiente ilustración se muestra la relación entre NLB y otros componentes de software en una configuración típica.  
  
![Equilibrio de carga de red y otros componentes de software](../media/NLB/nlb.jpg)  
  
A continuación es las principales características de NLB.  
  
- No se requieren cambios de hardware para la ejecución.  
  
- Proporciona herramientas de equilibrio de carga de red que permiten configurar y administrar varios clústeres y todos los hosts desde un solo equipo remoto o local.  
  
- Permite a los clientes tener acceso al clúster mediante un único nombre de Internet lógico y la dirección IP virtual, que se conoce como la dirección IP del clúster \(conserva los nombres individuales para cada equipo\). NLB permite la existencia de varias direcciones IP virtuales para los servidores de hosts múltiples.  
  
> [!NOTE]  
> Al implementar máquinas virtuales como clústeres virtuales, NLB no requiere que los servidores sean de hosts múltiples para tener varias direcciones IP virtuales.  
  
- NLB se puede enlazar a varios adaptadores de red, lo que permite configurar varios clústeres independientes en cada host. La compatibilidad con varios adaptadores de red difiere de los clústeres virtuales en que los clústeres virtuales le permiten configurar varios clústeres en un único adaptador de red.  
  
- No es necesario realizar modificaciones en las aplicaciones de servidor para que puedan ejecutar un clúster NLB.  
  
- NLB se puede configurar para agregar de manera automática un host al clúster si se produce un error en dicho clúster y posteriormente se vuelve a conectar en línea. El host agregado puede comenzar a administrar nuevas solicitudes de servidor de los clientes.  
  
-   Permite desconectar los equipos para llevar a cabo el mantenimiento preventivo sin alterar las operaciones del clúster en los demás hosts.  
  
## <a name="hardware-requirements"></a>Requisitos de hardware  
Siguientes son los requisitos de hardware para ejecutar un clúster NLB.  
  
-   Todos los hosts del clúster deben residir en la misma subred.  
  
-   No existe ningún tipo de limitación en cuanto al número de adaptadores de red en cada host y los hosts diferentes pueden tener un número distinto de adaptadores.  
  
-   Dentro de cada clúster, todos los adaptadores de red deben ser de unidifusión o multidifusión. NLB no admite un entorno mixto de unidifusión y multidifusión dentro de un solo clúster.  
  
-   Si usa el modo de unidifusión, el adaptador de red que se utiliza para administrar el cliente\-a\-debe admitir el tráfico del clúster cambiando su control de acceso de medios \(MAC\) dirección.  
  
## <a name="software-requirements"></a>Requisitos de software  
Siguientes son los requisitos de software para ejecutar un clúster NLB.  
  
-   Solo TCP\/IP puede usarse en el adaptador para el que se habilita en cada host. No agregue ningún otro protocolo \(por ejemplo, IPX\) a este adaptador.  
  
-   Las direcciones IP de los servidores del clúster deben ser estáticas.  
  
> [!NOTE]  
> NLB no admite el protocolo de configuración dinámica de Host \(DHCP\). NLB deshabilita DHCP en cada interfaz que configura.  
  
## <a name="installation-information"></a>Información de instalación  
Puede instalar NLB mediante el administrador del servidor o los comandos de Windows PowerShell para NLB.

Opcionalmente, puede instalar las herramientas de equilibrio de carga de red para administrar un clúster NLB local o remoto. Las herramientas incluyen el Administrador de equilibrio de carga de red y los comandos de PowerShell de Windows NLB.

### <a name="installation-with-server-manager"></a>Instalación con el administrador del servidor

En el administrador del servidor, puede usar el agregar Roles y características Asistente para agregar la **Network Load Balancing** característica. Cuando complete el asistente, NLB se instala y no es necesario reiniciar el equipo.


### <a name="installation-with-windows-powershell"></a>Instalación con Windows PowerShell  

Para instalar NLB mediante Windows PowerShell, ejecute el siguiente comando en un símbolo del sistema de Windows PowerShell con privilegios elevados en el equipo donde desea instalar NLB.

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
Una vez completada la instalación, no es necesario ningún reinicio del equipo.

Para obtener más información, consulte [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps).

### <a name="network-load-balancing-manager"></a>Administrador de equilibrio de carga de red
Para abrir el Administrador de equilibrio de carga de red en el Administrador del servidor, haga clic en **Herramientas** y, luego, en **Administrador de equilibrio de carga de red**.
  
## <a name="additional-resources"></a>Recursos adicionales  
En la tabla siguiente proporciona vínculos a información adicional acerca de la característica NLB.  
  
|Tipo de contenido|Referencias|  
|----------------|--------------|  
|Implementación|[Guía de implementación de equilibrio de carga de red](https://technet.microsoft.com/library/cc754833(WS.10).aspx) &#124; [configurar red equilibrio de carga con servicios de Terminal Server](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|Operaciones|[Administración de clústeres de equilibrio de carga de red](https://technet.microsoft.com/library/cc753954(WS.10).aspx) &#124; [establecer parámetros de equilibrio de carga de red](https://technet.microsoft.com/library/cc731619(WS.10).aspx) &#124; [controlar Hosts en clústeres de equilibrio de carga de red](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|Solución de problemas|[Solución de problemas de clústeres de equilibrio de carga de red](https://technet.microsoft.com/library/cc732592(WS.10).aspx) &#124; [errores y eventos de clúster NLB](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|Herramientas y configuración|[Cmdlets de PowerShell de Windows de equilibrio de carga de red](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|Recursos de la comunidad|[Alta disponibilidad \(clústeres\) foro](https://go.microsoft.com/fwlink/p/?LinkId=230641)
