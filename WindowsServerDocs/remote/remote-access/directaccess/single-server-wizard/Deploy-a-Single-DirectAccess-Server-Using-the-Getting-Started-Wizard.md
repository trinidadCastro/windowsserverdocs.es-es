---
title: Implementación de un solo servidor de DirectAccess con el Asistente para introducción
description: Este tema forma parte de la guía de implementación de un único servidor de DirectAccess con el Asistente para Introducción para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eb0cf464-0668-40f8-8222-feb6bae6d3d5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d927971094396a8df44eece80732a9d7a301ed33
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388613"
---
# <a name="deploy-a-single-directaccess-server-using-the-getting-started-wizard"></a>Implementación de un solo servidor de DirectAccess con el Asistente para introducción

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El siguiente tema sirve de introducción a un escenario de DirectAccess en el que se usa un solo servidor de DirectAccess y que permite implementar DirectAccess en unos pocos pasos.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Antes de proceder a la implementación, consulte la siguiente lista de configuraciones no compatibles, problemas conocidos y requisitos previos.  
Puede usar los temas siguientes para revisar los requisitos previos y otra información antes de implementar DirectAccess.  
  
-   [Configuraciones no compatibles de DirectAccess](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [Requisitos previos para la implementación de DirectAccess](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
En este escenario, un solo equipo que ejecute Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, se configura como un servidor de DirectAccess con la configuración predeterminada en unos pocos pasos sencillos del asistente, sin necesidad de configurar las opciones de la infraestructura. como una entidad de certificación (CA) o grupos de seguridad de Active Directory.  
  
> [!NOTE]  
> Si desea configurar una implementación avanzada con una configuración personalizada, vea [Deploy a Single DirectAccess Server with Advanced Settings](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
## <a name="in-this-scenario"></a>En este escenario  
Para configurar un servidor de DirectAccess básico, se requieren varios pasos de planeación e implementación.  
  
### <a name="prerequisites"></a>Requisitos previos  
Antes de empezar a implementar este escenario, revise esta lista de requisitos importantes:  
  
-   El firewall de Windows debe estar habilitado en todos los perfiles.  
  
-   Este escenario solo se admite cuando los equipos cliente ejecutan Windows 10, Windows 8.1 o Windows 8.  
  
-   ISATAP no es compatible con la red corporativa. Si utilizas ISATAP, debes eliminarlo y usar IPv6 nativo.  
  
-   No se necesita una infraestructura de clave pública.  
  
-   No se admite para implementar la autenticación en dos fases. Se necesitan credenciales de dominio para la autenticación.  
  
-   Implementa DirectAccess automáticamente en todos los equipos móviles del dominio actual.  
  
-   El tráfico a Internet no pasa por el túnel de DirectAccess. La configuración de túnel forzado no es compatible.  
  
-   El servidor de DirectAccess es el servidor de ubicación de red.  
  
-   La Protección de acceso a redes (NAP) no es compatible.  
  
-   No se admite la opción de cambiar directivas fuera de la consola de administración de DirectAccess o cmdlets de Windows PowerShell.  
  
-   Para implementar multisite, ahora o en el futuro, [implemente primero un solo servidor de DirectAccess con configuración avanzada](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
### <a name="planning-steps"></a>Pasos de planeación  
La planeación se divide en dos fases:  
  
1.  Planeación de la infraestructura de DirectAccess. En esta fase se describe la planificación necesaria para configurar la infraestructura de red antes de comenzar la implementación de DirectAccess. Incluye planear la topología de servidor y red, así como el servidor de ubicación de red de DirectAccess.  
  
2.  Planeación de la implementación de DirectAccess. En esta fase se explican los pasos de planificación necesarios para preparar la implementación de DirectAccess. Engloba planear los equipos cliente de DirectAccess, los requisitos de autenticación de servidor y cliente, la configuración de VPN y los servidores de infraestructura, así como los servidores de administración y aplicaciones.  
  
Para obtener información detallada sobre los pasos de planeación, consulte [planear una implementación de DirectAccess avanzada](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md).  
  
### <a name="deployment-steps"></a>Pasos de implementación  
La implementación se divide en tres fases:  
  
1.  Configuración de la infraestructura de DirectAccess: esta fase incluye la configuración de la red y el enrutamiento, la configuración del firewall, si es necesario, la configuración de certificados, servidores DNS, los valores de Active Directory y GPO, y la ubicación de red de DirectAccess. servidor.  
  
2.  Configuración del servidor de DirectAccess. Esta fase incluye los pasos para configurar los equipos cliente de DirectAccess, el servidor de DirectAccess y los servidores de infraestructura, así como los servidores de administración y aplicaciones.  
  
3.  Comprobando la implementación. Esta fase incluye pasos para comprobar que la implementación funciona según sea necesario.  
  
Para conocer los pasos de implementación, consulte [Install and Configure Basic DirectAccess](../../../remote-access/directaccess/single-server-wizard/Install-and-Configure-Basic-DirectAccess.md).  
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
La implementación de un único servidor de acceso remoto ofrece lo siguiente:  
  
-   Facilidad de acceso. Puede configurar equipos cliente administrados que ejecutan Windows 10, Windows 8.1, Windows 8 o Windows 7, como clientes de DirectAccess. Estos clientes pueden tener acceso a recursos de la red interna a través de DirectAccess en cualquier momento que estén ubicados en Internet sin necesidad de iniciar sesión con una conexión VPN. Los equipos cliente que no ejecuten uno de estos sistemas operativos pueden conectarse a la red interna a través de conexiones VPN tradicionales.  
  
-   Facilidad de administración. Los equipos cliente de DirectAccess ubicados en Internet pueden administrarse de manera remota por administradores de acceso remoto en DirectAccess, aun cuando los equipos cliente no estén ubicados en la red corporativa interna. Los equipos cliente que no cumplan los requisitos corporativos pueden ser actualizados automáticamente por servidores de administración. Tanto DirectAccess como VPN se administran en la misma consola y con el mismo conjunto de asistentes. Además, se pueden administrar uno o más servidores de acceso remoto desde una sola consola de administración de acceso remoto.  
  
## <a name="BKMK_NEW"></a>Roles y características incluidos en este escenario  
En la siguiente tabla, se muestran los roles y características requeridos para el escenario:  
  
|Rol/característica|Compatibilidad con este escenario|  
|---------|-----------------|  
|Rol de acceso remoto|El rol se instala y desinstala con la consola del Administrador del servidor o con Windows PowerShell. Este rol incluye tanto DirectAccess, que antes era una característica de Windows Server 2008 R2, como los servicios de enrutamiento y acceso remoto, que antes eran un servicio de rol bajo el rol del servidor Servicios de acceso y directivas de redes (NPAS). El rol de acceso remoto consta de dos componentes:<br /><br />1.  DirectAccess y VPN de servicios de enrutamiento y acceso remoto (RRAS). DirectAccess y VPN se administran conjuntamente en la consola de administración de acceso remoto.<br />2.  Enrutamiento de RRAS. Las características de enrutamiento RRAS se administran en la consola de enrutamiento y acceso remoto heredada.<br /><br />El rol del servidor de acceso remoto depende de las siguientes características o roles del servidor:<br /><br />-Servidor web-Internet Information Services (IIS): esta característica es necesaria para configurar el servidor de ubicación de red en el servidor de acceso remoto y el sondeo Web predeterminado.<br />-Windows Internal Database. se usa para las cuentas locales en el servidor de acceso remoto.|  
|Característica Herramientas de administración de acceso remoto|Esta característica se instala de la siguiente manera:<br /><br />-Se instala de forma predeterminada en un servidor de acceso remoto cuando se instala el rol de acceso remoto y es compatible con la interfaz de usuario de la consola de administración remota y con los cmdlets de Windows PowerShell.<br />-Se puede instalar opcionalmente en un servidor que no ejecute el rol de servidor de acceso remoto. En este caso, se usa para la administración remota de un equipo de acceso remoto que ejecuta DirectAccess y VPN.<br /><br />La característica de herramientas de administración de acceso remoto consiste de los siguientes elementos:<br /><br />-GUI de acceso remoto<br />-Módulo de acceso remoto para Windows PowerShell<br /><br />Las dependencias incluyen:<br /><br />-Consola de administración de directivas de grupo<br />-Kit de administración del administrador de conexiones RAS (CMAK)<br />-Windows PowerShell 3,0<br />-Infraestructura y herramientas de administración de gráficos|  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
Los requisitos de hardware para este escenario incluyen los siguientes:  
  
-   Requisitos del servidor:  
  
    -   Un equipo que cumpla los requisitos de hardware para Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
    -   El servidor debe tener al menos un adaptador de red instalado, habilitado y conectado a la red interna. Cuando se utilizan dos adaptadores, debería haber uno conectado a la red corporativa interna y otro, a la red externa (Internet o una red privada).  
  
    -   Al menos un controlador de dominio. Tanto el servidor de acceso remoto como los clientes de DirectAccess deben ser miembros del dominio.  
  
-   Requisitos de clientes:  
  
    -   Un equipo cliente debe ejecutar Windows 10, Windows 8.1 o Windows 8.  
  
        > [!IMPORTANT]  
        > Si algunos o todos los equipos cliente ejecutan Windows 7, debe usar el Asistente para configuración avanzada. El Asistente para la instalación de Introducción que se describe en este documento no es compatible con equipos cliente que ejecutan Windows 7. Vea [implementar un único servidor de DirectAccess con configuración avanzada](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) para obtener instrucciones sobre cómo usar clientes de Windows 7 con DirectAccess.  
  
        > [!NOTE]  
        > Solo se pueden usar los siguientes sistemas operativos como clientes de DirectAccess: Windows 10 Enterprise, Windows 8.1 Enterprise, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8 Enterprise, Windows Server 2008 R2, Windows 7 Enterprise y Windows 7 Ultimate.  
  
-   Requisitos de servidores de infraestructura y administración:  
  
    -   Si VPN está habilitada y no se ha configurado un grupo de direcciones IP estáticas, debe implementar un servidor DHCP para asignar direcciones IP automáticamente a los clientes de VPN.  
  
-   Se requiere un servidor DNS que ejecute Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 SP2 o Windows Server 2008 R2.  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Hay varios requisitos para este escenario:  
  
-   Requisitos del servidor:  
  
    -   El servidor de acceso remoto debe ser un miembro del dominio. El servidor se puede implementar en el perímetro de la red interna o tras un firewall perimetral u otro dispositivo.  
  
    -   Si el servidor de acceso remoto se encuentra tras un firewall perimetral o un dispositivo NAT, el dispositivo debe estar configurado de modo que permita el tráfico desde el servidor de acceso remoto y hacia él.  
  
    -   La persona que implemente el acceso remoto en el servidor necesita permisos de administrador local en el servidor y permisos de usuario del dominio. Además, el administrador necesita permisos para los GPO que se usan en la implementación de DirectAccess. Se necesitan permisos para crear un filtro WMI en el controlador de dominio para aprovechar las ventajas de las características que restringen la implementación de DirectAccess solamente a los equipos móviles.  
  
-   Requisitos de clientes de acceso remoto:  
  
    -   Los clientes de DirectAccess deben ser miembros del dominio. Los dominios que contienen clientes pueden pertenecer al mismo bosque que el servidor de acceso remoto, o tener una confianza bidireccional con el bosque del servidor de Acceso remoto.  
  
    -   Se requiere un grupo de seguridad de Active Directory para contener los equipos que se configurarán como clientes de DirectAccess. Si no se ha especificado un grupo de seguridad al establecer la configuración de cliente de DirectAccess, se aplica de forma predeterminada el GPO de cliente a todos los equipos portátiles en el grupo de seguridad Equipos del dominio. Solo se pueden usar los siguientes sistemas operativos como clientes de DirectAccess:  Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 8 Enterprise, Windows 7 Enterprise y Windows 7 Ultimate.  
  
## <a name="BKMK_LINKS"></a>Vea también  
En la siguiente tabla, se proporcionan los vínculos a recursos adicionales:  
  
|Tipo de contenido|Referencias|  
|--------|-------|  
|**Acceso remoto en TechNet**|[TechCenter de acceso remoto](https://technet.microsoft.com/network/bb545655.aspx)|  
|**Herramientas y configuración**|[Cmdlets de acceso remoto de PowerShell](https://technet.microsoft.com/library/hh918399.aspx)|  
|**Recursos de la comunidad**|[Entradas de wiki de DirectAccess](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**Tecnologías relacionadas**|[Cómo funciona IPv6](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  


