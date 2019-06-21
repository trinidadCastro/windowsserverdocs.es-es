---
title: Agregar DirectAccess a una implementación de acceso remoto existente (VPN)
description: Este tema forma parte de la Guía de agregar DirectAccess a una implementación de acceso remoto existente (VPN) para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5db01f7-1ae0-46f2-9be7-8d9e121446b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c6bd11855ec9c5387241ba42babf88c0461a9c4
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283585"
---
# <a name="add-directaccess-to-an-existing-remote-access-vpn-deployment"></a>Agregar DirectAccess a una implementación de acceso remoto existente (VPN)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
En este escenario, un equipo que ejecuta Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 se configura como un servidor de DirectAccess con la configuración recomendada después de haber instalado y configurado la VPN. Si desea configurar DirectAccess con características empresariales, como un clúster con equilibrio de carga, una implementación multisitio o autenticación de cliente en dos fases, complete el escenario descrito en este tema para configurar un solo servidor y, a continuación, configure la empresa escenario tal como se describe en [implementar acceso remoto en una empresa](../../ras/Deploy-Remote-Access-in-an-Enterprise.md).  
  
## <a name="in-this-scenario"></a>En este escenario  
Para configurar un solo servidor de acceso remoto, son necesarios varios pasos de planificación e implementación.  
  
### <a name="planning-steps"></a>Pasos de planeación  
La planeación se divide en dos fases:  
  
1.  **Planear la infraestructura de acceso remoto**  
  
    En esta fase se describe la planificación necesaria para configurar la infraestructura de red antes de comenzar la implementación de acceso remoto. Engloba planear la topología de servidores y de red y los certificados, el Sistema de nombres de dominio (DNS), la configuración del objeto de directiva de grupo (GPO) de Active Directory y el servidor de ubicación de red de DirectAccess.  
  
2.  **Planear la implementación de acceso remoto**  
  
    En esta fase se describen los pasos de planificación necesarios para preparar la implementación de acceso remoto. Incluye planear los equipos cliente de acceso remoto, los requisitos de autenticación de servidor y cliente y los servidores de infraestructura.  
  
### <a name="deployment-steps"></a>Pasos de implementación  
La implementación se divide en tres fases:  
  
1.  **Configurar la infraestructura de acceso remoto**  
  
    Esta fase conlleva configurar la red y el enrutamiento, establecer la configuración de firewall (de ser necesario), los certificados, los servidores DNS, los valores de Active Directory y GPO y el servidor de ubicación de red de DirectAccess.  
  
2.  **Configurar el servidor de acceso remoto**  
  
    En esta fase, configuraremos los equipos cliente de acceso remoto, el servidor de acceso remoto y los servidores de la infraestructura.  
  
3.  **Comprobar la implementación**  
  
    En esta fase, comprobaremos que la implementación funciona como debe.  
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
La implementación de un único servidor de acceso remoto ofrece lo siguiente:  
  
-   **facilidad de acceso**  
  
    Administrar los equipos que ejecutan Windows 8 y Windows 7 pueden configurarse como equipos cliente de DirectAccess del cliente. Estos clientes pueden tener acceso a recursos de la red interna a través de DirectAccess en cualquier momento que se encuentren en Internet, y sin necesidad de iniciar sesión con una conexión VPN. Los equipos cliente que no ejecuten uno de estos sistemas operativos pueden conectarse a la red interna a través de VPN. DirectAccess y VPN se administran en la misma consola y con el mismo conjunto de asistentes.  
  
-   **Facilidad de administración**  
  
    Los equipos cliente de DirectAccess con acceso a Internet pueden administrarse de manera remota por administradores de acceso remoto en DirectAccess, aun cuando los equipos cliente no estén ubicados en la red corporativa interna. Los equipos cliente que no cumplan los requisitos corporativos pueden ser actualizados automáticamente por servidores de administración.  
  
## <a name="BKMK_NEW"></a>Roles y características necesarias para este escenario  
En la siguiente tabla se recogen los roles y características necesarios en este escenario:  
  
|Rol/característica|Compatibilidad con este escenario|  
|---------|-----------------|  
|Rol de acceso remoto|El rol se instala y desinstala con la consola del Administrador del servidor o con Windows PowerShell. Este rol incluye tanto DirectAccess (que antes era una característica de Windows Server 2008 R2) como los servicios de enrutamiento y acceso remoto, que antes eran un servicio de rol de Servicios de acceso y directivas de redes (NPAS). El rol de acceso remoto consta de dos componentes:<br /><br />1.  DirectAccess y Servicios de enrutamiento y acceso remoto (RRAS): se administran en la consola de administración de acceso remoto.<br />2.  Enrutamiento de RRAS: se administra en la consola de enrutamiento y acceso remoto.<br /><br />El rol del servidor de acceso remoto depende de las siguientes características del servidor:<br /><br />-Internet Information Services (IIS) servidor Web: necesarios para configurar el servidor de ubicación de red en el servidor de acceso remoto y el sondeo web predeterminado.<br />-Windows Internal Database: se usa para las cuentas locales en el servidor de acceso remoto.|  
|Característica Herramientas de administración de acceso remoto|Esta característica se instala de la siguiente manera:<br /><br />-De forma predeterminada en un servidor de acceso remoto cuando se instala el rol de acceso remoto. Admite la interfaz de usuario de la consola de administración de acceso remoto y cmdlets de Windows PowerShell.<br />-Opcionalmente instalado en un servidor que no ejecuta el rol de servidor de acceso remoto. en cuyo caso se usa para la administración remota de un equipo de acceso remoto que ejecuta DirectAccess y VPN.<br /><br />La característica de herramientas de administración de acceso remoto consiste de los siguientes elementos:<br /><br />-GUI de acceso remoto<br />-Módulo de acceso remoto para Windows PowerShell<br /><br />Las dependencias incluyen:<br /><br />: Consola de administración de directivas de grupo<br />-Kit de administración de Connection Manager (CMAK) de RAS<br />-Windows PowerShell 3.0<br />-Infraestructura y herramientas de administración gráfico|  
  
## <a name="BKMK_HARD"></a>Requisitos de hardware  
Los requisitos de hardware para este escenario incluyen los siguientes:  
  
**Requisitos del servidor**  
  
-   Un equipo que cumpla los requisitos de hardware para Windows Server 2012.  
  
-   El servidor debe tener al menos un adaptador de red instalado, habilitado y unido a la red interna. Cuando se usan dos adaptadores, debe haber uno conectado a la red corporativa interna y otro, a la red externa (Internet).  
  
-   Si se requiere Teredo como protocolo de transición de IPv4 a IPv6, el adaptador externo del servidor requiere dos direcciones IPv4 públicas consecutivas. El Asistente para habilitar DirectAccess no habilita Teredo, aun cuando existan dos direcciones IP consecutivas. Si solo hay disponible una dirección IP, únicamente se puede usar IP-HTTPS como protocolo de transición.  
  
-   Al menos un controlador de dominio. Tanto el servidor de acceso remoto como los clientes de DirectAccess deben ser miembros del dominio.  
  
-   El Asistente para habilitar DirectAccess necesita certificados para IP-HTTPS y el servidor de ubicación de red. Si la VPN SSTP ya usa un certificado, este se reutilizará para IP-HTTPS. Si la VPN SSTP no está configurada, puedes configurar un certificado para IP-HTTPS o usar un certificado autofirmado creado automáticamente. Para el servidor de ubicación de red, puedes configurar un certificado o usar un certificado autofirmado creado automáticamente.  
  
**Requisitos del cliente**  
  
-   Un equipo cliente debe ejecutar Windows 8 o Windows 7.  
  
    > [!NOTE]  
    > Solo se pueden usar los siguientes sistemas operativos como clientes de DirectAccess: Windows Server 2012, Windows Server 2008 R2, Windows 8 Enterprise, Windows 7 Enterprise y Ultimate de Windows 7.  
  
**Requisitos de servidor de infraestructura y administración**  
  
-   Durante la administración remota de los equipos cliente de DirectAccess, los clientes inician la comunicación con servidores de administración —como controladores de dominio, servidores de System Center Configuration y servidores de la Autoridad de registro de mantenimiento (HRA)— para obtener servicios que incluyen Windows y actualizaciones de antivirus y la conformidad de clientes de Protección de acceso a redes (NAP). Los servidores necesarios se deben implementar antes de comenzar la implementación de acceso remoto.  
  
-   Si el acceso remoto requiere la conformidad NAP de los clientes, el Servidor de directivas de red (NPS) y HRA se deben implementar antes de comenzar la implementación de acceso remoto  
  
-   Se requiere un servidor DNS que ejecuta Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 con SP2.  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Los requisitos de software para este escenario son los siguientes:  
  
**Requisitos del servidor**  
  
-   El servidor de acceso remoto debe ser un miembro del dominio. El servidor se puede implementar en el perímetro de la red interna o tras un firewall perimetral u otro dispositivo.  
  
-   Si el servidor de acceso remoto se encuentra detrás de un firewall perimetral o un dispositivo de traducción de direcciones de red (NAT), el dispositivo debe estar configurado de modo que permita el tráfico desde y hacia el servidor de acceso remoto.  
  
-   La persona que implemente el acceso remoto en el servidor necesita permisos de administrador local en el servidor y permisos de usuario del dominio. Además, el administrador necesita permisos para los GPO que se usan en la implementación de DirectAccess. Se necesitan permisos para crear un filtro WMI en el controlador de dominio para aprovechar las ventajas de las características que restringen la implementación de DirectAccess solamente a los equipos móviles.  
  
**Requisitos del cliente de acceso remoto**  
  
-   Los clientes de DirectAccess deben ser miembros del dominio. Los dominios que contienen clientes pueden pertenecer al mismo bosque que el servidor de acceso remoto, o tener una confianza bidireccional con el bosque o el dominio del servidor de acceso remoto.  
  
-   Se requiere un grupo de seguridad de Active Directory para contener los equipos que se configurarán como clientes de DirectAccess. Si no se ha especificado un grupo de seguridad al establecer la configuración de cliente de DirectAccess, se aplica de forma predeterminada el GPO de cliente en todos los equipos portátiles (con capacidad para DirectAccess) en el grupo de seguridad Equipos del dominio. Solo se pueden usar los siguientes sistemas operativos como clientes de DirectAccess:  Windows Server 2012, Windows Server 2008 R2, Windows 8 Enterprise, Windows 7 Enterprise y Ultimate de Windows 7.  
  
    > [!NOTE]  
    > Recomendamos crear un grupo de seguridad por cada dominio que contenga equipos que se vayan a configurar como clientes de DirectAccess.  
  

  

