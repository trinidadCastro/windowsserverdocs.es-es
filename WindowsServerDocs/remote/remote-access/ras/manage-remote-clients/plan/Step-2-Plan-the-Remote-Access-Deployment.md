---
title: Paso 2 Planear la implementación de acceso remoto
description: En este tema forma parte de la Guía de los clientes de DirectAccess de administrar remotamente en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 78303bbdd29819389944348a279fb4a52f1570fb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282789"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>Paso 2 Planear la implementación de acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de planear la infraestructura que va a usar para configurar el servidor de acceso remoto único para la administración remota de los clientes de DirectAccess, está listo para planear la configuración que va a usar el Asistente para instalación de acceso remoto.  
  
> [!NOTE]  
> Antes de continuar con estas tareas, consulte [paso 1: Planear la infraestructura de acceso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|Tarea|Descripción|  
|----|--------|  
|[Planear una estrategia de implementación de cliente](#plan-a-client-deployment-strategy)|Decide qué equipos administrados se configurarán como clientes de DirectAccess.|  
|[Planear una estrategia de implementación del servidor de acceso remoto](#plan-a-remote-access-server-deployment-strategy)|Planifica cómo implementar el servidor de acceso remoto.|  
|[Planear las configuraciones de los servidores de infraestructura](#plan-the-infrastructure-servers-configurations)|Planear los servidores de infraestructura en la implementación de acceso remoto, incluido el servidor de ubicación de red de DirectAccess, servidores DNS y servidores de administración de DirectAccess.|  
  
## <a name="plan-a-client-deployment-strategy"></a>Planear una estrategia de implementación de cliente  
Tienes que tomar tres decisiones a la hora de planear la implementación de clientes:  
  
1.  ¿DirectAccess sólo estará disponible para los equipos móviles, o para todos los equipos de un grupo de seguridad especificado?  
  
    Al configurar los clientes de DirectAccess en el Asistente para instalación de cliente de DirectAccess, puede elegir permitir que solo los equipos móviles en grupos de seguridad especificados para conectarse al servidor mediante el uso de DirectAccess. Si restringes el acceso a los equipos móviles, el acceso remoto configura automáticamente un filtro WMI para garantizar que el GPO de cliente de DirectAccess se aplique solo a los equipos móviles de los grupos de seguridad especificados. El administrador del acceso remoto necesita permisos si desea crear o modificar los filtros WMI de directiva de grupo para habilitar esta configuración.  
  
2.  ¿Qué grupos de seguridad contendrán los equipos cliente de DirectAccess?  
  
    Configuración de DirectAccess se incluye en el objeto de directiva de grupo (GPO) del cliente de DirectAccess. El GPO se aplica a los equipos que forman parte de los grupos de seguridad que especifiques en el Asistente para la instalación del cliente de DirectAccess. Puede especificar grupos de seguridad que se encuentran en cualquier dominio admitido.
  
    Antes de configurar el acceso remoto, deberá crear los grupos de seguridad. Puede agregar equipos al grupo de seguridad después de completar la implementación de acceso remoto. Sin embargo, si agrega los equipos cliente que residen en un dominio diferente del grupo de seguridad, el GPO de cliente no se aplicará a los clientes. Por ejemplo, si creaste SG1 en el dominio A los clientes de DirectAccess, y después agregaste a clientes del dominio B a este grupo, el GPO de cliente no se aplicará a los clientes en el dominio B.  
  
    Para evitar este problema, cree un nuevo grupo de seguridad de cliente para cada dominio que contenga los equipos cliente. Como alternativa, si no desea crear un nuevo grupo de seguridad, ejecute el **Add-DAClient** cmdlet de Windows PowerShell con el nombre del nuevo GPO para el nuevo dominio.  
  
3.  ¿Qué opciones configurarás para el Asistente de conectividad de red de DirectAccess?  
  
    El Asistente de conectividad de red de DirectAccess se ejecuta en los equipos cliente y se proporciona información adicional sobre la conexión de DirectAccess a los usuarios finales. En el Asistente para la instalación del cliente de DirectAccess, puedes configurar lo siguiente:  
  
    -   **Comprobadores de conectividad**  
  
        Se crea una sonda web predeterminada que los clientes usan para validar la conectividad a la red interna. El nombre predeterminado es `https://directaccess-WebProbeHost.<domain_name>`. El nombre debe registrarse manualmente en DNS. Puede crear otros comprobadores de conectividad que usan otras direcciones web a través de HTTP o PING. Debe existir una entrada DNS por cada comprobador de conectividad.  
  
    -   **Ayuda de dirección de correo electrónico del departamento de soporte técnico**  
  
        Si los usuarios finales experimenten problemas de conectividad de DirectAccess, puede enviar un correo electrónico que contiene información de diagnóstico para los administradores de acceso remoto, que pueden solucionar el problema.  
  
    -   **Nombre de la conexión de DirectAccess**  
  
        Puede especificar un nombre de conexión de DirectAccess para ayudar a los usuarios finales a identificar la conexión de DirectAccess en su equipo.  
  
    -   **Permitir a los clientes de DirectAccess usen la resolución local**  
  
        Los clientes requieren un medio de resolución de nombres localmente. Si permites que los clientes de DirectAccess usen resolución de nombres local, los usuarios finales pueden usar servidores de DNS locales para resolver los nombres. Cuando los usuarios finales decide usar servidores DNS locales para la resolución de nombres, DirectAccess no envía las solicitudes de resolución de nombres de una sola etiqueta para el servidor DNS corporativo interno. En su lugar utiliza la resolución local (mediante el uso de la resolución de nombres de multidifusión Local de vínculos (LLMNR) y NetBios a través de protocolos TCP/IP).  
  
## <a name="plan-a-remote-access-server-deployment-strategy"></a>Planear una estrategia de implementación del servidor de acceso remoto  
Las decisiones que deberá realizar cuando planee la implementación del servidor de acceso remoto se incluyen:  
  
-   **Topología de red**  
  
    Hay dos topologías disponibles al implementar un servidor de acceso remoto:  
  
    -   **Dos adaptadores**: Con dos adaptadores de red, acceso remoto puede configurarse con un adaptador de red conectado directamente a Internet y el otro conectado a la red interna. O bien, como alternativa, el servidor se instala detrás de un dispositivo perimetral, como un firewall o un enrutador. En esta red de una configuración adaptador está conectado a la red perimetral y el otro está conectado a la red interna.  
  
    -   **Adaptador de red único**: En esta configuración, el acceso remoto se instala el servidor detrás de un dispositivo perimetral, como un firewall o un enrutador. El adaptador de red se conecta a la red interna.  

-   **Adaptadores de red**  
  
    El Asistente para instalación de servidor de acceso remoto detecta automáticamente los adaptadores de red que están configurados en el servidor de acceso remoto. Debes asegurarte de que están seleccionados los adaptadores correctos.  
  
-   **Certificado IP-HTTPS**  
  
    El Asistente para la instalación del servidor de acceso remoto detecta automáticamente un certificado que es adecuado para la conexión IP-HTTPS. El nombre de sujeto del certificado que selecciones debe coincidir con la dirección ConnectTo. Si utiliza certificados autofirmados, puede elegir usar un certificado que se crea automáticamente el servidor de acceso remoto.  
  
-   **Prefijos IPv6**  
  
    Si el Asistente para la instalación del servidor de acceso remoto detecta que se ha implementado IPv6 en los adaptadores de red, automáticamente rellena los prefijos IPv6 para la red interna, un prefijo IPv6 para asignar a los equipos cliente de DirectAccess y un prefijo IPv6 para asignar a los equipos cliente de VPN. Si los prefijos generados automáticamente no son correctos para su infraestructura ISATAP o IPv6 nativa, debes cambiarlos de forma manual.  
  
-   **Autenticación**  
  
    Puede elegir uno de los métodos siguientes para autenticar los clientes de DirectAccess al servidor de acceso remoto:  
  
    -   **Autenticación de usuario**: Puedes permitir que los usuarios se autentiquen con credenciales de Active Directory o con una autenticación en dos fases.  
  
    -   **Autenticación del equipo**: Puede configurar la autenticación de equipo para usar los certificados. O bien, el servidor de acceso remoto puede actuar como un proxy para la autenticación Kerberos sin necesidad de certificados. 
  
    -   **Los clientes de Windows 7** de forma predeterminada, los equipos cliente que ejecutan Windows 7 no se pueden conectar a una implementación de acceso remoto que ejecuta Windows Server 2012. Si tiene clientes que ejecutan Windows 7 en su organización que necesiten acceso remoto a los recursos internos, puede permitirles conectarse. Los equipos cliente que quieras que tengan acceso a los recursos internos deben ser miembros de un grupo de seguridad que especifiques en el Asistente para la instalación del cliente de DirectAccess.  
  
        > [!NOTE]  
        > Permitir que los clientes que ejecutan Windows 7 para conectarse con DirectAccess, deberá usar la autenticación de certificado de equipo.  
  
-   **Configuración de VPN**  
  
    Antes de configurar el acceso remoto, decida si va a proporcionar acceso a VPN a los clientes remotos. Debe proporcionar acceso a VPN si tiene equipos cliente de su organización que no admiten la conectividad de DirectAccess (por ejemplo, no son administrados o ejecuten un sistema operativo para el que no se admite DirectAccess). El Asistente para instalación de servidor de acceso remoto permite configurar cómo se asignan direcciones IP (mediante DHCP o desde un grupo de direcciones estáticas) y cómo se autentican los clientes VPN (mediante el uso de Active Directory o un servidor RADIUS).  
  
## <a name="plan-the-infrastructure-servers-configurations"></a>Planear las configuraciones de los servidores de infraestructura  
Acceso remoto requiere tres tipos de servidores de infraestructura:  
  
-   **Servidor de ubicación de red**  
  
-   **Servidores DNS** 
  
-   **Servidores de administración** 
  
## <a name="see-also"></a>Vea también  
  
-   [Paso 1: Planificar la infraestructura de acceso remoto](Step-1-Plan-the-Remote-Access-Infrastructure.md)  
  


