---
title: Plan del paso 2 avanzada de las implementaciones de DirectAccess
description: Este tema forma parte de la Guía de implementación de un único servidor de DirectAccess con avanzada configuración para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3bba28d4-23e2-449f-8319-7d2190f68d56
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 31209b3770fb910c843b6fd39d6e76b8672088a9
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266758"
---
# <a name="step-2-plan-advanced-directaccess-deployments"></a>Plan del paso 2 avanzada de las implementaciones de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planear la infraestructura de DirectAccess, el siguiente paso para realizar una implementación avanzada de DirectAccess en un único servidor con IPv4 e IPv6 es planear la configuración del Asistente para instalación de acceso remoto.  
  
|Tarea|Descripción|  
|----|--------|  
|[2.1 planear la implementación de cliente](#21-plan-for-client-deployment)|Planea cómo permitir a los equipos cliente que se conecten mediante DirectAccess. Decide qué equipos administrados se configurarán como clientes de DirectAccess y planea la implementación del Asistente para la conectividad de red o el Asistente de conectividad de DirectAccess en equipos cliente.|  
|[2.2 Planear la implementación de servidor de DirectAccess](#22-plan-for-directaccess-server-deployment)|Planea cómo implementar el servidor de DirectAccess.|  
|[2.3 planear los servidores de infraestructura](#23-plan-infrastructure-servers)|Planea los servidores de infraestructura para tu implementación de DirectAccess, incluido el servidor de ubicación de red de DirectAccess, los servidores del Sistema de nombres de dominio (DNS) y los servidores de administración de DirectAccess.|  
|[2.4 planear los servidores de aplicaciones](#24-plan-application-servers)|Planea los servidores de aplicaciones IPv4 e IPv6 y, opcionalmente, determina si es necesaria una autenticación descentralizada entre los equipos cliente de DirectAccess y los servidores de aplicaciones internos.|  
|[2.5 planear DirectAccess y los clientes VPN de terceros](#25-plan-directaccess-and-third-party-vpn-clients)|Al implementar DirectAccess con clientes VPN de terceros, puede ser necesario establecer un valor del Registro para permitir la coexistencia de las dos soluciones de acceso remoto.|  
  
## <a name="21-plan-for-client-deployment"></a>2.1 Planear la implementación de clientes  
Tienes que tomar tres decisiones a la hora de planear la implementación de clientes:  
  
1.  ¿DirectAccess estará disponible solo para equipos móviles o para cualquier equipo?  
  
    Cuando configuras los clientes en el Asistente para la instalación del cliente de DirectAccess, puedes elegir que solo los equipos móviles de los grupos de seguridad especificados puedan conectarse usando DirectAccess. Si restringes el acceso a los equipos móviles, el acceso remoto configura automáticamente un filtro WMI para garantizar que el GPO de cliente de DirectAccess se aplique solo a los equipos móviles de los grupos de seguridad especificados. El administrador de Acceso remoto necesita los permisos de seguridad Crear o Modificar para crear o modificar filtros WMI para objetos de directiva de grupo (GPO) para habilitar esta opción.  
  
2.  ¿Qué grupos de seguridad contendrán los equipos cliente de DirectAccess?  
  
    La configuración de los clientes de DirectAccess se encuentran en el GPO de cliente de DirectAccess. El GPO se aplica a los equipos que forman parte de los grupos de seguridad que especifiques en el Asistente para la instalación del cliente de DirectAccess. Puedes especificar que los grupos de seguridad se encuentren en cualquier dominio admitido. Para obtener más información, consulte la sección [1.7 planear Active Directory Domain Services](da-adv-plan-s1-infrastructure.md#17-plan-active-directory-domain-services).  
  
    Antes de configurar DirectAccess, debes crear los grupos de seguridad. Puedes agregar equipos al grupo de seguridad después de completar la implementación de DirectAccess, pero si agregas equipos cliente que residen en un dominio diferente del grupo de seguridad, el GPO de cliente no se aplicará a esos clientes. Por ejemplo, si creaste SG1 en un dominio A para clientes de DirectAccess y más tarde agregas clientes del dominio B a este grupo, el GPO de cliente no se aplicará a los clientes del dominio B. Para evitar este problema, crea un nuevo grupo de seguridad de clientes para cada dominio que contenga equipos cliente de DirectAccess. Si no quieres crear un nuevo grupo de seguridad, también puedes ejecutar el cmdlet de Windows PowerShell **Add-DAClient** con el nombre del nuevo GPO para el nuevo dominio.  
  
3.  ¿Qué opciones configurarás para el asistente de conectividad de red?  
  
    El asistente de conectividad de red se ejecuta en equipos cliente y proporciona información adicional acerca de la conexión de DirectAccess para usuarios finales. En el Asistente para la instalación del cliente de DirectAccess, puedes configurar lo siguiente:  
  
    -   **Comprobadores de conectividad**  
  
        Se crea una sonda web predeterminada que los clientes usan para validar la conectividad a la red interna. El nombre predeterminado es:  
  
        https://directaccess-WebProbeHost.<domain_name>  
  
        El nombre debe registrarse manualmente en DNS. Puedes crear otros comprobadores de la conectividad usando otras direcciones web a través de HTTP o usando **ping**. Debe existir una entrada DNS por cada comprobador de conectividad.  
  
    -   **Una dirección de correo electrónico de soporte técnico**  
  
        Si los usuarios finales tienen problemas de conectividad con DirectAccess, pueden enviar un correo electrónico que contiene información de diagnóstico para que el administrador de DirectAccess solucione el problema.  
  
    -   **Un nombre de conexión de DirectAccess**  
  
        Especifica un nombre de conexión de DirectAccess para ayudar a los usuarios finales a identificar la conexión de DirectAccess en sus equipos.  
  
    -   **Permitir a los clientes de DirectAccess usen la resolución local**  
  
        Los clientes necesitan una manera de resolver los nombres localmente. Si permites que los clientes de DirectAccess usen resolución de nombres local, los usuarios finales pueden usar servidores de DNS locales para resolver los nombres. Cuando los usuarios finales eligen usar servidores DNS locales para la resolución de nombres, DirectAccess no envía solicitudes de resolución para nombres de etiqueta únicos al servidor DNS corporativo interno. En su lugar, usa la resolución de nombres local (mediante los protocolos Resolución de nombres de multidifusión local de vínculos (LLMNR) y NetBIOS a través de TCP/IP).  
  
## <a name="22-plan-for-directaccess-server-deployment"></a>2.2 Planear la implementación del servidor de DirectAccess  
Ten en cuenta las siguientes decisiones a la hora de planear la implementación del servidor de DirectAccess:  
  
-   **Topología de red**  
  
    Hay un par de topologías disponibles para implementar un servidor de DirectAccess:  
  
    -   **Dos adaptadores de red**. Con dos adaptadores de red, DirectAccess se puede configurar con un adaptador conectado directamente a Internet y el otro conectado a la red interna. El servidor también se puede instalar normalmente detrás de un dispositivo perimetral, como un firewall o un enrutador. En esta configuración, un adaptador de red se conecta a la red perimetral y el otro a la red interna.  
  
    -   **Un adaptador de red**. En esta configuración, el servidor de DirectAccess se instala detrás de un dispositivo perimetral, como un firewall o un enrutador. El adaptador de red se conecta a la red interna.  
  
    Para obtener más información acerca de cómo seleccionar la topología para la implementación, consulte [1.1 topología de red de Plan y la configuración](da-adv-plan-s1-infrastructure.md#11-plan-network-topology-and-settings).  
  
-   **Dirección ConnectTo**  
  
    Los equipos cliente usan la dirección ConnectTo para conectar con el servidor de DirectAccess. La dirección que elijas debe coincidir con el nombre de sujeto del certificado IP-HTTPS que implementes para la conexión IP-HTTPS, y debe estar disponible en el DNS público.  
  
-   **Adaptadores de red**  
  
    El Asistente para la instalación del servidor de acceso remoto detecta automáticamente los adaptadores de red que están configurados en el servidor de DirectAccess. Debes asegurarte de que están seleccionados los adaptadores correctos.  
  
-   **Certificado IP-HTTPS**  
  
    El Asistente para la instalación del servidor de acceso remoto detecta automáticamente un certificado que es adecuado para la conexión IP-HTTPS. El nombre de sujeto del certificado que selecciones debe coincidir con la dirección ConnectTo. Si estás usando certificados autofirmados, puedes elegir usar un certificado creado automáticamente por el servidor de acceso remoto.  
  
-   **Prefijos IPv6**  
  
    Si el Asistente para la instalación del servidor de acceso remoto detecta que se ha implementado IPv6 en los adaptadores de red, automáticamente rellena los prefijos IPv6 para la red interna, un prefijo IPv6 para asignar a los equipos cliente de DirectAccess y un prefijo IPv6 para asignar a los equipos cliente de VPN. Si los prefijos generados automáticamente no son correctos para tu infraestructura de IPv6 nativa, debes cambiarlos manualmente. Para obtener más información, consulte [1.1 topología de red de Plan y la configuración](da-adv-plan-s1-infrastructure.md#11-plan-network-topology-and-settings).  
  
-   **Autenticación**  
  
    Decide cómo se autenticarán los clientes de DirectAccess en el servidor de DirectAccess:  
  
    -   **Autenticación de usuario**. Puedes permitir que los usuarios se autentiquen con credenciales de Active Directory o con una autenticación en dos fases. Para obtener más información acerca de la autenticación en dos fases, vea [implementar acceso remoto con autenticación OTP](https://technet.microsoft.com/library/hh831379.aspx).  
  
    -   **Autenticación de equipos**. Puedes configurar la autenticación de equipos para usar certificados o para usar el servidor de DirectAccess como un proxy de Kerberos en nombre del cliente. Para obtener más información, consulte [1.3 requisitos de certificados de Plan](da-adv-plan-s1-infrastructure.md#13-plan-certificate-requirements).  
  
    -   **Los clientes de Windows 7**. De forma predeterminada, los equipos cliente que ejecutan Windows 7 no se pueden conectar a una implementación de Windows Server 2012 R2 o Windows Server 2012 DirectAccess. Si tiene clientes en su organización que ejecutan Windows 7 y necesitan acceso remoto a los recursos internos, puede permitirles conectarse. Los equipos cliente que quieras que tengan acceso a los recursos internos deben ser miembros de un grupo de seguridad que especifiques en el Asistente para la instalación del cliente de DirectAccess.  
  
        > [!NOTE]  
        > Permitir que los clientes que ejecutan Windows 7 para conectarse con DirectAccess requiere que use la autenticación de certificado de equipo.  
  
-   **Configuración de VPN**  
  
    Antes de configurar DirectAccess, decide si vas a proporcionar acceso a VPN a los clientes remotos no compatibles con DirectAccess. Debes proporcionar acceso a VPN si en la organización hay equipos cliente que no admiten la conectividad de DirectAccess (porque no son administrados o porque ejecutan un sistema operativo no compatible con DirectAccess). El Asistente para la instalación del servidor de acceso remoto permite configurar cómo se asignan las direcciones IP (mediante DHCP o desde un grupo de direcciones estáticas) y cómo se autentican los clientes de VPN: usando Active Directory o un servidor de Servicio de autenticación remota telefónica de usuario (RADIUS).  
  
## <a name="23-plan-infrastructure-servers"></a>2.3 Planear los servidores de infraestructura  
DirectAccess necesita tres tipos de servidores de infraestructura:  
  
-   **Servidores DNS**. Para obtener más información, consulta [1.4 Planear los requisitos de DNS](da-adv-plan-s1-infrastructure.md#14-plan-dns-requirements).  
  
-   **Servidor de ubicación de red**. Para obtener más información, consulta [1.5 Planear el servidor de ubicación de red](da-adv-plan-s1-infrastructure.md#15-plan-the-network-location-server).  
  
-   **Servidores de administración**. Para obtener más información, consulta [1.6 Planear los servidores de administración](da-adv-plan-s1-infrastructure.md#16-plan-management-servers).  
  
## <a name="24-plan-application-servers"></a>2.4 Planear los servidores de aplicaciones  
Los servidores de aplicaciones son servidores de la red corporativa que son accesibles para los equipos cliente a través de una conexión de DirectAccess. Para identificar los servidores de aplicaciones, se agregan a un grupo de seguridad. Después, se aplica el GPO de servidor de aplicaciones a los servidores de ese grupo.  
  
> [!NOTE]  
> Agregar los servidores de aplicaciones a un grupo de seguridad solo es necesario si necesitas autenticación y cifrado descentralizados.  
  
También puedes necesitar autenticación y cifrado descentralizados entre el cliente de DirectAccess y los servidores de aplicaciones internos seleccionados. Si configuras la autenticación descentralizada, los clientes de DirectAccess usan una directiva de transporte de IPsec. Esta directiva requiere que la autenticación y la protección del tráfico de las sesiones de IPsec se termine en los servidores de aplicaciones especificados. En este caso, el servidor de acceso remoto reenvía las sesiones de IPsec autenticadas y protegidas a los servidores de aplicaciones.  
  
De forma predeterminada, cuando la autenticación se extiende a los servidores de aplicaciones, la carga de datos se cifra entre el cliente de DirectAccess y el servidor de aplicaciones. Puedes elegir no cifrar el tráfico y usar solo la autenticación. Sin embargo, esto es menos seguro que usar autenticación y cifrado, y solo se admite para servidores de aplicaciones ejecutan Windows Server 2008 R2, los sistemas operativos de Windows Server 2012.  
  
## <a name="25-plan-directaccess-and-third-party-vpn-clients"></a>2.5 Planear DirectAccess y los clientes VPN de terceros  
Algunos clientes VPN de terceros no crean conexiones en la carpeta Conexiones de red. Esto puede hacer que DirectAccess determine que no tiene conectividad de la intranet cuando se establece la conexión VPN y la conectividad con la intranet exista. Esto ocurre cuando los clientes VPN de terceros registran sus interfaces definiéndolos como tipos de extremo de la Especificación de interfaz de dispositivo de red (NDIS). Puedes habilitar la coexistencia con estos tipos de clientes VPN estableciendo el siguiente valor del Registro en 1 en los clientes de DirectAccess:  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\ShowDomainEndpointInterfaces (REG_DWORD)**  
  
Algunos clientes VPN de terceros emplean una configuración de túnel dividido, que permite al equipo cliente VPN tener acceso directamente a Internet, sin tener que enviar el tráfico a través de la conexión VPN a la intranet.  
  
Las configuraciones de túnel dividido suelen dejar la configuración de puerta de enlace predeterminada en el cliente VPN como no configurada o como todo ceros (0.0.0.0). Puedes confirmar este comportamiento estableciendo una conexión VPN correcta con la intranet y usando la herramienta Ipconfig.exe en la línea de comandos para ver la configuración resultante.  
  
Si la conexión VPN muestra su puerta de enlace predeterminada como vacía o como todo ceros (0.0.0.0), tu cliente VPN está configurado de esta manera. De forma predeterminada, el cliente de DirectAccess no identifica las configuraciones de túnel dividido. Para configurar que los clientes de DirectAccess detecten estos tipos de configuraciones del cliente VPN y coexistir con ellos, establece el siguiente valor del Registro en 1:  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\Internet\ EnableNoGatewayLocationDetection (REG_DWORD)**  
  
## <a name="previous-step"></a>Paso anterior  
  
-   [Paso 1: Planear la infraestructura de DirectAccess](da-adv-plan-s1-infrastructure.md)  
  


