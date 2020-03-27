---
title: Administrar el acceso remoto
description: En este tema se proporciona información sobre cómo administrar el acceso remoto en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1459819a-b1b6-4800-8770-4a85d02c7a2b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6437a7aa5a535352ad4f6c6be8fbac2162b6feea
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308429"
---
# <a name="manage-remote-access"></a>Administrar el acceso remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El escenario de implementación de administración de clientes remotos de DirectAccess usa DirectAccess para mantener a los clientes a través de Internet. En esta sección se explica el escenario, incluidos sus roles, fases, características y vínculos a recursos adicionales.  
  
Windows Server 2016 y Windows Server 2012 combinan DirectAccess y la VPN del servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto.   
  
> [!NOTE]  
> Además de este tema, están disponibles los siguientes temas sobre la administración de Acceso remoto.  
>   
> -   [Usar supervisión y cuentas de acceso remoto](monitoring-and-accounting/Use-Remote-Access-Monitoring-and-Accounting.md)  
> -   [Administrar clientes de DirectAccess de forma remota](manage-remote-clients/Manage-DirectAccess-Clients-Remotely.md)  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descripción del escenario  
Los equipos cliente de DirectAccess están conectados a la intranet siempre que están conectados a Internet, sin importar si el usuario ha iniciado sesión en el equipo. Se pueden administrar como recursos de intranet y mantenerse actualizados con los cambios de directiva de grupo, las actualizaciones del sistema operativo, las actualizaciones de antimalware y otros cambios organizativos.  
  
En algunos casos, los servidores o equipos de la intranet deben iniciar conexiones con los clientes de DirectAccess. Por ejemplo, los técnicos del departamento de soporte técnico pueden usar conexiones a Escritorio remoto para conectarse a clientes remotos de DirectAccess y solucionar problemas. Este escenario permite conservar la solución de acceso remoto existente en su lugar para la conectividad del usuario, al mismo tiempo que se usa DirectAccess para administración remota.  
  
DirectAccess proporciona una configuración que admite la administración remota de clientes de DirectAccess. Puede usar la opción del asistente para implementación que limita la creación de directivas a solo aquellas necesarias para la administración remota de equipos cliente.  
  
> [!NOTE]  
> En esta implementación, las opciones de configuración de nivel de usuario, como túnel forzado, integración de la Protección de acceso a redes (NAP), y la autenticación en dos fases no están disponibles.  
  
## <a name="in-this-scenario"></a>En este escenario  
El escenario de implementación de la administración de clientes remotos de DirectAccess incluye los siguientes pasos de planeación y configuración:  
  
### <a name="plan-the-deployment"></a>Planear la implementación  
Solo hay unos pocos requisitos de equipo y de red para planear este escenario: Incluyen:  
  
-   **Topología de red y servidores**: con DirectAccess, se puede colocar el servidor de acceso remoto en el perímetro de la intranet o detrás de un dispositivo o un firewall de traducción de direcciones de red (NAT).  
  
-   **Servidor de ubicación de red de DirectAccess**: Los clientes de DirectAccess usan el servidor de ubicación de red para determinar si están ubicados en la red interna. El servidor de ubicación de red se puede instalar en el servidor de DirectAccess o en otro servidor.  
  
-   **Clientes de DirectAccess**: Decide qué equipos administrados se configurarán como clientes de DirectAccess.  
  
### <a name="configure-the-deployment"></a>Configurar la implementación  
La configuración de la implementación consta de una serie de pasos. Entre ellos se incluyen los siguientes:  
  
1.  **Configurar la infraestructura**: configure DNS, una el servidor y los equipos cliente a un dominio si fuera necesario y configure los grupos de seguridad de Active Directory.  
  
    En este escenario de implementación, Acceso remoto crea objetos de directiva de grupo (GPO) automáticamente. Para ver las opciones avanzadas de GPO de certificado, consulte [implementar acceso remoto avanzado](assetId:///3475e527-541f-4a34-b940-18d481ac59f6).  
  
2.  **Configurar la red y el servidor de Acceso remoto**: configure adaptadores de red, direcciones IP y enrutamiento.  
  
3.  **Configurar los valores de certificado**: en este escenario de implementación, el asistente para introducción crea certificados autofirmados, por lo que no es necesario configurar la infraestructura de certificados más avanzada.  
  
4.  **Configurar el servidor de ubicación de red**:  en este escenario, el servidor de ubicación de red se instala en el servidor de Acceso remoto.  
  
5.  **Planear servidores de administración de DirectAccess**: Los administradores pueden administrar de forma remota equipos cliente de DirectAccess que estén ubicados fuera de la red corporativa mediante Internet. Los servidores de administración incluyen equipos que se usan durante la administración de clientes remotos (como los servidores de actualización).  
  
6.  **Configurar el servidor de acceso remoto**: instale el rol de acceso remoto y ejecute el Asistente para introducción de DirectAccess para configurar DirectAccess.  
  
7.  **Comprobación de la implementación**: pruebe un cliente para asegurarse de que puede conectarse a la red interna y a Internet con DirectAccess.  
  
## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicaciones prácticas  
La implementación de un solo servidor de acceso remoto para administrar los clientes de DirectAccess ofrece lo siguiente:  
  
-   **Facilidad de acceso**: los equipos cliente administrados que ejecutan Windows 8 o Windows 7 pueden configurarse como equipos cliente de DirectAccess. Estos clientes pueden tener acceso a recursos de la red interna a través de DirectAccess en cualquier momento en que estén conectados a Internet sin necesidad de iniciar sesión con una conexión VPN. Los equipos cliente que no ejecuten uno de estos sistemas operativos pueden conectarse a la red interna a través de VPN. DirectAccess y VPN se administran en la misma consola y con el mismo conjunto de asistentes.  
  
-   **Facilidad de administración**: los equipos cliente de DirectAccess conectados a Internet pueden administrarlos de manera remota administradores de acceso remoto con DirectAccess, aun cuando los equipos cliente no estén ubicados en la red corporativa interna. Los equipos cliente que no cumplan los requisitos corporativos pueden ser actualizados automáticamente por servidores de administración. Uno o más servidores de acceso remoto pueden ser administrados desde una sola consola de administración de acceso remoto.  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Roles y características incluidos en este escenario  
En la siguiente tabla, se muestran los roles y características requeridos para el escenario:  
  
|Rol o característica|Compatibilidad con este escenario|  
|----------|-----------------|  
|*Rol de acceso remoto*|Este rol se instala y desinstala con la consola del Administrador del servidor o con Windows PowerShell. Este rol incluye tanto DirectAccess (que antes era una característica de Windows Server 2008 R2) como los servicios de enrutamiento y acceso remoto, que antes eran un servicio de rol de Servicios de acceso y directivas de redes (NPAS). El rol de acceso remoto consta de dos componentes:<br /><br />1. DirectAccess y VPN de servicios de enrutamiento y acceso remoto (RRAS): DirectAccess y VPN se administran en la consola de administración de acceso remoto.<br />2. RRAS: las características se administran en la consola de enrutamiento y acceso remoto.<br /><br />El rol del servidor de Acceso remoto depende de las siguientes características:<br /><br />-Servidor Web (IIS): se requiere para configurar el servidor de ubicación de red y el sondeo Web predeterminado.<br />-Windows Internal Database: se usa para las cuentas locales en el servidor de acceso remoto.|  
|Característica Herramientas de administración de acceso remoto|Esta característica se instala de la siguiente manera:<br /><br />: De forma predeterminada, en un servidor de acceso remoto, cuando se instala el rol de acceso remoto y es compatible con la interfaz de usuario de la consola de administración remota.<br />-Como opción en un servidor que no ejecuta el rol de servidor de acceso remoto. En ese caso, se usa para la administración remota de un servidor de Acceso remoto.<br /><br />Esta característica consiste en lo siguiente:<br /><br />-Herramientas de línea de comandos y GUI de acceso remoto<br />-Módulo de acceso remoto para Windows PowerShell<br /><br />Las dependencias incluyen:<br /><br />-Consola de administración de directivas de grupo<br />-Kit de administración del administrador de conexiones RAS (CMAK)<br />-Windows PowerShell 3,0<br />-Infraestructura y herramientas de administración de gráficos|  
  
## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>Requisitos de hardware  
Los requisitos de hardware para este escenario incluyen los siguientes:  
  
### <a name="server-requirements"></a>Requisitos de servidor  
  
-   Un equipo que cumpla los requisitos de hardware para Windows Server 2016. Para obtener más información, consulte [requisitos del sistema](https://technet.microsoft.com/windows-server-docs/get-started/system-requirements-and-installation)de Windows Server 2016.  
  
-   El servidor debe tener al menos un adaptador de red instalado y habilitado. Debería haber solo un adaptador conectado a la red interna corporativa y solo uno conectado a la red externa (Internet).  
  
-   Si se requiere Teredo como protocolo de transición de IPv6 a IPv4, el adaptador externo del servidor requiere dos direcciones IPv4 públicas consecutivas. Si un solo adaptador de red está disponible, solo se puede usar IP-HTTPS como protocolo de transición.  
  
-   Al menos un controlador de dominio. Los servidores de acceso remoto y los clientes de DirectAccess deben ser miembros del dominio.  
  
-   Se necesita una entidad de certificación en el servidor si no quiere usar certificados autofirmados para IP-HTTPS o el servidor de ubicación de red, o si quiere usar certificados de cliente para la autenticación IPsec de clientes.  
  
### <a name="client-requirements"></a>Requisitos del cliente  
  
-   Un equipo cliente debe ejecutar Windows 10 o Windows 8 o Windows 7.  
  
### <a name="infrastructure-and-management-server-requirements"></a>Requisitos de servidores de infraestructura y administración  
  
-   Durante la administración remota de los equipos cliente de DirectAccess, los clientes inician comunicaciones con servidores de administración tales como controladores de dominio, servidores de System Center Configuration y servidores de la Entidad de registro de mantenimiento (HRA). Estos servidores proporcionan servicios que incluyen actualizaciones de Windows y de antivirus y la conformidad de clientes de Protección de acceso a redes (NAP). Debe implementar los servidores necesarios antes de comenzar la implementación de Acceso remoto.  
  
-   Se requiere un servidor DNS que ejecute Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 con SP2.  
  
## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software  
Los requisitos de software para este escenario son los siguientes:  
  
### <a name="server-requirements"></a>Requisitos de servidor  
  
-   El servidor de acceso remoto debe ser un miembro del dominio. El servidor se puede implementar en el perímetro de la red interna o tras un firewall perimetral u otro dispositivo.  
  
-   Si el servidor de acceso remoto se encuentra tras un firewall perimetral o un dispositivo NAT, el dispositivo debe estar configurado de modo que permita el tráfico desde el servidor de acceso remoto y hacia él.  
  
-   Los administradores que implementen un servidor de Acceso remoto necesitan permisos de administrador local en el servidor y permisos de usuario del dominio. Además, el administrador necesita permisos para los GPO que se usan en la implementación de DirectAccess. Para aprovechar las ventajas de las características que restringen la implementación de DirectAccess solamente en equipos móviles, se necesitan permisos de administrador de dominio en el controlador de dominio para crear un filtro WMI.  
  
-   Si el servidor de ubicación de red no está ubicado en el servidor de acceso remoto, se requiere un servidor independiente para ejecutarlo.  
  
### <a name="remote-access-client-requirements"></a>Requisitos de clientes de acceso remoto  
  
-   Los clientes de DirectAccess deben ser miembros del dominio. Los dominios que contienen clientes pueden pertenecer al mismo bosque que el servidor de acceso remoto, o tener una confianza bidireccional con el bosque o el dominio del servidor de acceso remoto.  
  
-   Se requiere un grupo de seguridad de Active Directory para contener los equipos que se configurarán como clientes de DirectAccess. Los equipos no deben estar incluidos en más de un grupo de seguridad que incluya clientes de DirectAccess. Si los clientes están incluidos en varios grupos, la resolución de nombres para las solicitudes de los clientes no funcionará según lo esperado.  
  

