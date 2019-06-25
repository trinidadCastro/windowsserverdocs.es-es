---
title: Implementación de un solo servidor de DirectAccess con la configuración avanzada
description: Este tema forma parte de la Guía de implementación de un único servidor de DirectAccess con avanzada configuración para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b211a9ca-1208-4e1f-a0fe-26a610936c30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f17e1c9dd1a4e2d064a4e5980904c524dc62fb72
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283612"
---
# <a name="deploy-a-single-directaccess-server-with-advanced-settings"></a>Implementación de un solo servidor de DirectAccess con la configuración avanzada

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona una introducción al escenario de DirectAccess que usa un único servidor de DirectAccess y le permite implementar DirectAccess con configuración avanzada.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Antes de proceder a la implementación, consulte la siguiente lista de configuraciones no compatibles, problemas conocidos y requisitos previos.  
Puede usar los temas siguientes para revisar los requisitos previos y otra información antes de implementar DirectAccess.  
  
-   [Configuraciones no compatibles de DirectAccess](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [Requisitos previos para la implementación de DirectAccess](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
En este escenario, un equipo que ejecuta Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, se configura como un servidor de DirectAccess con configuración avanzada.  
  
> [!NOTE]  
> Si quieres configurar una implementación básica solo con ajustes básicos, consulta [Deploy a Single DirectAccess Server Using the Getting Started Wizard](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md). En el escenario de configuración sencilla, DirectAccess se configura con opciones de configuración predeterminadas mediante un asistente, sin necesidad de establecer la configuración de infraestructura, como una entidad de certificación (CA) o grupos de seguridad de Active Directory.  
  
## <a name="in-this-scenario"></a>En este escenario  
Para configurar un único servidor de DirectAccess con configuración avanzada, debes realizar varios pasos de planificación e implementación.  
  
### <a name="prerequisites"></a>Requisitos previos  
Revisa los siguientes requisitos antes de empezar.  
  
-   Firewall de Windows debe estar habilitado en todos los perfiles.  
  
-   El servidor de DirectAccess es el servidor de ubicación de red.  
  
-   Conviene que todos los equipos inalámbricos estén en el dominio en el que se instale el servidor de DirectAccess para que puedan estar habilitados para DirectAccess. Cuando DirectAccess se implementa, se habilita automáticamente en todos los equipos móviles del dominio actual.  
  
> [!IMPORTANT]  
> Algunas tecnologías y configuraciones no pueden usarse cuando DirectAccess se implementa.  
>   
> -   El protocolo ISATAP (Intra-Site Automatic Tunnel Addressing Protocol) en la red corporativa no es compatible. Si usas ISATAP, deberás quitarlo y usar IPv6 nativo.  
  
### <a name="planning-steps"></a>Pasos de planeación  
La planeación se divide en dos fases:  
  
1.  **Planificación de la infraestructura de DirectAccess**. En esta fase se describe la planificación necesaria para configurar la infraestructura de red antes de comenzar la implementación de DirectAccess. Engloba planear la topología de servidores y de red y los certificados, DNS, la configuración del objeto de directiva de grupo (GPO) de Active Directory y el servidor de ubicación de red de DirectAccess.  
  
2.  **Planificación de la implementación de DirectAccess**. En esta fase se explican los pasos de planificación necesarios para preparar la implementación de DirectAccess. Engloba planear los equipos cliente de DirectAccess, los requisitos de autenticación de servidor y cliente, la configuración de VPN y los servidores de infraestructura, así como los servidores de administración y aplicaciones.  
  
### <a name="deployment-steps"></a>Pasos de implementación  
La implementación se divide en tres fases:  
  
1.  **Configuración de la infraestructura de DirectAccess**. Esta fase conlleva configurar la red y el enrutamiento, establecer la configuración de firewall (de ser necesario), configurar certificados, servidores DNS, los valores de Active Directory y GPO, y el servidor de ubicación de red de DirectAccess.  
  
2.  **Configuración de las opciones de servidor de DirectAccess**. Esta fase incluye los pasos para configurar los equipos cliente de DirectAccess, el servidor de DirectAccess y los servidores de infraestructura, así como los servidores de administración y aplicaciones.  
  
3.  **Comprobación de la implementación**. Esta fase engloba los pasos necesarios para comprobar la implementación de DirectAccess.  
  
Para obtener los pasos detallados de implementación, consulta [Install and Configure Advanced DirectAccess](../../../remote-access/directaccess/single-server-advanced/Install-and-Configure-Advanced-DirectAccess.md).  
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
La implementación de un único servidor de DirectAccess reporta lo siguiente:  
  
-   **Accesibilidad**. Equipos cliente administrados que ejecutan Windows 10, Windows 8.1, Windows 8 y Windows 7 pueden configurarse como equipos cliente de DirectAccess. Estos clientes pueden tener acceso a recursos de la red interna a través de DirectAccess en cualquier momento que estén ubicados en Internet sin necesidad de iniciar sesión con una conexión VPN. Los equipos cliente que no ejecuten uno de estos sistemas operativos pueden conectarse a la red interna a través de VPN.  
  
-   **Administrabilidad**. Los equipos cliente de DirectAccess ubicados en Internet pueden administrarse de manera remota por administradores de acceso remoto en DirectAccess, aun cuando los equipos cliente no estén ubicados en la red corporativa interna. Los equipos cliente que no cumplan los requisitos corporativos pueden ser actualizados automáticamente por servidores de administración. Tanto DirectAccess como VPN se administran en la misma consola y con el mismo conjunto de asistentes. Además, se pueden administrar uno o más servidores de DirectAccess desde una sola consola de administración de acceso remoto.  
  
## <a name="BKMK_NEW"></a>Roles y características necesarias para este escenario  
En la siguiente tabla se recogen los roles y características necesarios en este escenario:  
  
|Rol/característica|Compatibilidad con este escenario|  
|---------|-----------------|  
|Rol de acceso remoto|El rol se instala y desinstala con la consola del Administrador del servidor o con Windows PowerShell. Este rol engloba tanto DirectAccess como los Servicios de enrutamiento y acceso remoto (RRAS). El rol de acceso remoto consta de dos componentes:<br/><br/>1.  DirectAccess y VPN de RRAS. DirectAccess y VPN se administran conjuntamente en la consola de administración de acceso remoto.<br/>2.  Enrutamiento de RRAS. Las características de enrutamiento RRAS se administran en la consola de enrutamiento y acceso remoto heredada.<br /><br />El rol del servidor de acceso remoto depende de las siguientes características o roles del servidor:<br/><br/> -Internet Information Services (IIS) Web Server: esta característica es necesaria para configurar el servidor de ubicación de red en el servidor de DirectAccess y el sondeo web predeterminado.<br/> -Windows Internal Database. Se utiliza para las cuentas locales del servidor de DirectAccess.|  
|Característica Herramientas de administración de acceso remoto|Esta característica se instala de la siguiente manera:<br /><br />-Se instala de forma predeterminada en un servidor de DirectAccess cuando se instala el rol de acceso remoto y es compatible con la interfaz de usuario de consola de administración remota y los cmdlets de Windows PowerShell.<br />-Se puede instalar opcionalmente en un servidor que no ejecuta el rol de servidor de DirectAccess. En este caso, se usa para la administración remota de un equipo de acceso remoto que ejecuta DirectAccess y VPN.<br /><br />La característica de herramientas de administración de acceso remoto consiste de los siguientes elementos:<br /><br />-La opción remote Access interfaz gráfica de usuario)<br />-Módulo de acceso remoto para Windows PowerShell<br /><br />Las dependencias incluyen:<br /><br />: Consola de administración de directivas de grupo<br />-Kit de administración de Connection Manager (CMAK) de RAS<br />-Windows PowerShell 3.0<br />-Infraestructura y herramientas de administración gráfico|  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
Los requisitos de hardware para este escenario incluyen los siguientes:  
  
-   Requisitos del servidor:  
  
    -   Un equipo que cumpla los requisitos de hardware para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
    -   El servidor debe tener al menos un adaptador de red instalado, habilitado y conectado a la red interna. Cuando se utilizan dos adaptadores, debería haber uno conectado a la red corporativa interna y otro, a la red externa (Internet o una red privada).  
  
    -   Si se requiere Teredo como protocolo de transición de IPv4 a IPv6, el adaptador externo del servidor requiere dos direcciones IPv4 públicas consecutivas. Si solo está disponible una única dirección IP, entonces solo se puede usar IP-HTTPS como protocolo de transición.  
  
    -   Al menos un controlador de dominio. Tanto el servidor de DirectAccess como los clientes de DirectAccess deben ser miembros del dominio.  
  
    -   Necesitarás una entidad de certificación (CA) si no quieres usar certificados autofirmados para IP-HTTPS o el servidor de ubicación de red, o si quieres usar certificados de cliente para la autenticación IPsec de clientes. También puede solicitar certificados a una CA pública.  
  
    -   Si el servidor de ubicación de red no está ubicado en el servidor de DirectAccess, se necesita un servidor web independiente para ejecutarlo.  
  
-   Requisitos de clientes:  
  
    -   Un equipo cliente debe ejecutar Windows 10, Windows 8 o Windows 7.  
  
        > [!NOTE]  
        > Los siguientes sistemas operativos se pueden usar como clientes de DirectAccess: Windows 10, Windows Server 2012 R2, Windows Server 2012, Windows 8 Enterprise, Windows 7 Enterprise o Ultimate de Windows 7.  
  
-   Requisitos de servidores de infraestructura y administración:  
  
    -   Durante la administración remota de los equipos cliente de DirectAccess, los clientes inician comunicaciones con servidores de administración tales como controladores de dominio, servidores de System Center Configuration y servidores de la Entidad de registro de mantenimiento (HRA) para servicios que incluyen Windows y actualizaciones de antivirus y la conformidad de clientes de Protección de acceso a redes (NAP). Los servidores requeridos se deben implementar antes de comenzar la implementación de acceso remoto.  
  
    -   Si el acceso remoto requiere la conformidad NAP de clientes, los servidores NPS y HRS se deben implementar antes de comenzar la implementación de acceso remoto  
  
    -   Si VPN está habilitada, se requiere un servidor DHCP para asignar direcciones IP automáticamente a los clientes de VPN, si no se usa un grupo de direcciones estáticas.  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Hay varios requisitos para este escenario:  
  
-   Requisitos del servidor:  
  
    -   El servidor de DirectAccess debe ser miembro del dominio. El servidor se puede implementar en el perímetro de la red interna o tras un firewall perimetral u otro dispositivo.  
  
    -   Si el servidor de DirectAccess se encuentra detrás de un firewall perimetral o un dispositivo NAT, el dispositivo debe estar configurado de modo que permita el tráfico desde y hacia el servidor de acceso remoto.  
  
    -   La persona que implemente el acceso remoto en el servidor necesita permisos de administrador local en el servidor y permisos de usuario del dominio. Además, el administrador necesita permisos para los GPO que se usan en la implementación de DirectAccess. Se necesitan permisos para crear un filtro WMI en el controlador de dominio para aprovechar las ventajas de las características que restringen la implementación de DirectAccess solamente a los equipos móviles.  
  
-   Requisitos de clientes de acceso remoto:  
  
    -   Los clientes de DirectAccess deben ser miembros del dominio. Los dominios que contienen clientes pueden pertenecer al mismo bosque que el servidor de DirectAccess, o tener una confianza bidireccional con el bosque o el dominio del servidor de DirectAccess.  
  
    -   Se requiere un grupo de seguridad de Active Directory para contener los equipos que se configurarán como clientes de DirectAccess. Si no se ha especificado un grupo de seguridad al establecer la configuración de cliente de DirectAccess, se aplica de forma predeterminada el GPO de cliente a todos los equipos portátiles en el grupo de seguridad Equipos del dominio.  
  
        > [!NOTE]  
        > Se recomienda crear un grupo de seguridad para cada dominio que contenga equipos cliente de DirectAccess.  
  
        > [!IMPORTANT]  
        > Si habilitaste Teredo en la implementación de DirectAccess, y desea proporcionar acceso a los clientes de Windows 7, asegúrese de que los clientes se actualizan a Windows 7 con SP1. Los clientes que usan Windows 7 RTM no podrán conectarse a través de Teredo. pero sí seguir conectándose a la red corporativa mediante IP-HTTPS.  
  
## <a name="BKMK_LINKS"></a>Vea también  
En la siguiente tabla, se proporcionan los vínculos a recursos adicionales:  
  
|Tipo de contenido|Referencias|  
|--------|-------|  
|**Implementación**|[Rutas de acceso de implementación de DirectAccess en Windows Server](../../../remote-access/directaccess/DirectAccess-Deployment-Paths-in-Windows-Server.md)<br /><br />[Implementar un único servidor de DirectAccess mediante el Asistente para introducción](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|  
|**Herramientas y configuración**|[Cmdlets de PowerShell de acceso remoto](https://technet.microsoft.com/library/hh918399.aspx)|  
|**Recursos de la comunidad**|[Guía de supervivencia de DirectAccess](https://social.technet.microsoft.com/wiki/contents/articles/23210.directaccess-survival-guide.aspx)<br /><br />[Entradas Wiki de DirectAccess](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**Tecnologías relacionadas**|[Funcionamiento de IPv6](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  

