---
title: Paso 2 planear la implementación de acceso remoto
description: Este tema forma parte de la guía administrar los clientes de DirectAccess de forma remota en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: lizross
author: eross-msft
ms.openlocfilehash: befd517f8e00548524dc9bf9c328d63034c653be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860578"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>Paso 2 planear la implementación de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planear la infraestructura que piensa usar para configurar el servidor de acceso remoto único para la administración remota de los clientes de DirectAccess, está listo para planear la configuración que usará el Asistente para la instalación de acceso remoto.  
  
> [!NOTE]  
> Antes de continuar con estas tareas, consulte [paso 1: planear la infraestructura de acceso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|Tarea|Descripción|  
|----|--------|  
|[Planear una estrategia de implementación de cliente](#plan-a-client-deployment-strategy)|Decide qué equipos administrados se configurarán como clientes de DirectAccess.|  
|[Planear una estrategia de implementación de servidor de acceso remoto](#plan-a-remote-access-server-deployment-strategy)|Planifica cómo implementar el servidor de acceso remoto.|  
|[Planeación de las configuraciones de los servidores de infraestructura](#plan-the-infrastructure-servers-configurations)|Planear los servidores de infraestructura en la implementación de acceso remoto, incluidos el servidor de ubicación de red de DirectAccess, los servidores DNS y los servidores de administración de DirectAccess.|  
  
## <a name="plan-a-client-deployment-strategy"></a>Planear una estrategia de implementación de cliente  
Tienes que tomar tres decisiones a la hora de planear la implementación de clientes:  
  
1.  ¿Estará disponible DirectAccess solo para equipos móviles o para todos los equipos de un grupo de seguridad específico?  
  
    Al configurar los clientes de DirectAccess en el Asistente para la instalación del cliente de DirectAccess, puede permitir que solo los equipos móviles de los grupos de seguridad especificados se conecten al servidor mediante DirectAccess. Si restringes el acceso a los equipos móviles, el acceso remoto configura automáticamente un filtro WMI para garantizar que el GPO de cliente de DirectAccess se aplique solo a los equipos móviles de los grupos de seguridad especificados. El administrador del acceso remoto necesita permisos si desea crear o modificar los filtros WMI de directiva de grupo para habilitar esta configuración.  
  
2.  ¿Qué grupos de seguridad contendrán los equipos cliente de DirectAccess?  
  
    La configuración de DirectAccess está contenida en el objeto de directiva de grupo de cliente de DirectAccess (GPO). El GPO se aplica a los equipos que forman parte de los grupos de seguridad que especifiques en el Asistente para la instalación del cliente de DirectAccess. Puede especificar grupos de seguridad contenidos en cualquier dominio compatible.
  
    Antes de configurar el acceso remoto, debe crear los grupos de seguridad. Puede agregar equipos al grupo de seguridad después de completar la implementación de acceso remoto. Sin embargo, si agrega equipos cliente que residen en un dominio diferente que el grupo de seguridad, el GPO de cliente no se aplicará a esos clientes. Por ejemplo, si ha creado SG1 en el dominio A para los clientes de DirectAccess y posteriormente agrega clientes del dominio B a este grupo, el GPO de cliente no se aplicará a los clientes del dominio B.  
  
    Para evitar este problema, cree un nuevo grupo de seguridad de cliente para cada dominio que contenga equipos cliente. Como alternativa, si no desea crear un nuevo grupo de seguridad, ejecute el cmdlet **Add-DAClient** de Windows PowerShell con el nombre del nuevo GPO para el nuevo dominio.  
  
3.  ¿Qué opciones de configuración se configurarán para el Asistente de conectividad de red de DirectAccess?  
  
    El Asistente de conectividad de red de DirectAccess se ejecuta en los equipos cliente y proporciona información adicional sobre la conexión de DirectAccess a los usuarios finales. En el Asistente para la instalación del cliente de DirectAccess, puedes configurar lo siguiente:  
  
    -   **Comprobadores de conectividad**  
  
        Se crea una sonda web predeterminada que los clientes usan para validar la conectividad a la red interna. El nombre predeterminado es `https://directaccess-WebProbeHost.<domain_name>`. El nombre debe registrarse manualmente en DNS. Puede crear otros comprobadores de conectividad que utilicen otras direcciones web a través de HTTP o PING. Debe existir una entrada DNS por cada comprobador de conectividad.  
  
    -   **Dirección de correo electrónico del servicio de asistencia**  
  
        Si los usuarios finales experimentan problemas de conectividad de DirectAccess, pueden enviar un correo electrónico que contenga información de diagnóstico al administrador de acceso remoto, que puede solucionar el problema.  
  
    -   **Nombre de conexión de DirectAccess**  
  
        Puede especificar un nombre de conexión de DirectAccess para ayudar a los usuarios finales a identificar la conexión de DirectAccess en su equipo.  
  
    -   **Permitir a los clientes de DirectAccess usar la resolución local de nombres**  
  
        Los clientes requieren un medio de resolución local de nombres. Si permites que los clientes de DirectAccess usen resolución de nombres local, los usuarios finales pueden usar servidores de DNS locales para resolver los nombres. Cuando los usuarios finales eligen usar servidores DNS locales para la resolución de nombres, DirectAccess no envía solicitudes de resolución de nombres de etiqueta única al servidor DNS corporativo interno. En su lugar, utiliza la resolución local de nombres (con los protocolos de resolución de nombres de multidifusión local de vínculo (LLMNR) y NetBios sobre TCP/IP).  
  
## <a name="plan-a-remote-access-server-deployment-strategy"></a>Planear una estrategia de implementación de servidor de acceso remoto  
Entre las decisiones que debe tomar cuando planea implementar el servidor de acceso remoto se incluyen las siguientes:  
  
-   **Topología de red**  
  
    Hay dos topologías disponibles al implementar un servidor de acceso remoto:  
  
    -   **Dos adaptadores**: con dos adaptadores de red, el acceso remoto se puede configurar con un adaptador de red conectado directamente a Internet y el otro conectado a la red interna. O bien, el servidor se instala detrás de un dispositivo perimetral, como un firewall o un enrutador. En esta configuración, un adaptador de red está conectado a la red perimetral y el otro está conectado a la red interna.  
  
    -   **Adaptador de red único**: en esta configuración, el servidor de acceso remoto se instala detrás de un dispositivo perimetral, como un firewall o un enrutador. El adaptador de red se conecta a la red interna.  

-   **Adaptadores de red**  
  
    El Asistente para la instalación del servidor de acceso remoto detecta automáticamente los adaptadores de red que están configurados en el servidor de acceso remoto. Debes asegurarte de que están seleccionados los adaptadores correctos.  
  
-   **Certificado IP-HTTPS**  
  
    El Asistente para la instalación del servidor de acceso remoto detecta automáticamente un certificado que es adecuado para la conexión IP-HTTPS. El nombre de sujeto del certificado que selecciones debe coincidir con la dirección ConnectTo. Si utiliza certificados autofirmados, puede optar por usar un certificado creado automáticamente por el servidor de acceso remoto.  
  
-   **Prefijos IPv6**  
  
    Si el Asistente para la instalación del servidor de acceso remoto detecta que se ha implementado IPv6 en los adaptadores de red, automáticamente rellena los prefijos IPv6 para la red interna, un prefijo IPv6 para asignar a los equipos cliente de DirectAccess y un prefijo IPv6 para asignar a los equipos cliente de VPN. Si los prefijos generados automáticamente no son correctos para su infraestructura ISATAP o IPv6 nativa, debes cambiarlos de forma manual.  
  
-   **Autenticación**  
  
    Puede elegir uno de los métodos siguientes para autenticar a los clientes de DirectAccess en el servidor de acceso remoto:  
  
    -   **Autenticación de usuario**: puede permitir a los usuarios autenticarse con credenciales de Active Directory o con la autenticación en dos fases.  
  
    -   **Autenticación**del equipo: puede configurar la autenticación del equipo para usar certificados. O bien, el servidor de acceso remoto puede actuar como un proxy para la autenticación Kerberos sin necesidad de certificados. 
  
    -   **Clientes de Windows 7** De forma predeterminada, los equipos cliente que ejecutan Windows 7 no pueden conectarse a una implementación de acceso remoto que ejecute Windows Server 2012. Si tiene clientes que ejecutan Windows 7 en su organización que requieren acceso remoto a recursos internos, puede permitirles conectarse. Los equipos cliente que quieras que tengan acceso a los recursos internos deben ser miembros de un grupo de seguridad que especifiques en el Asistente para la instalación del cliente de DirectAccess.  
  
        > [!NOTE]  
        > Para permitir que los clientes que ejecutan Windows 7 se conecten mediante DirectAccess, es necesario usar la autenticación de certificado de equipo.  
  
-   **Configuración de VPN**  
  
    Antes de configurar el acceso remoto, decida si va a proporcionar acceso VPN a los clientes remotos. Debe proporcionar acceso a VPN si tiene equipos cliente de su organización que no admiten la conectividad de DirectAccess (por ejemplo, no están administrados o ejecutan un sistema operativo para el que no se admite DirectAccess). El Asistente para la instalación del servidor de acceso remoto le permite configurar el modo en que se asignan las direcciones IP (mediante DHCP o desde un grupo de direcciones estáticas) y cómo se autentican los clientes VPN (mediante Active Directory o un servidor RADIUS).  
  
## <a name="plan-the-infrastructure-servers-configurations"></a>Planeación de las configuraciones de los servidores de infraestructura  
El acceso remoto requiere tres tipos de servidores de infraestructura:  
  
-   **Servidor de ubicación de red**  
  
-   **Servidores DNS** 
  
-   **Servidores de administración** 
  
## <a name="see-also"></a>Vea también  
  
-   [Paso 1: planear la infraestructura de acceso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md)  
  


