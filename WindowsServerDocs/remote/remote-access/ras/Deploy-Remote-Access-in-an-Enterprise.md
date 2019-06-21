---
title: Implementar el acceso directo en una empresa
description: En este tema se proporciona una introducción al escenario de DirectAccess en Windows Server 2016 para la empresa.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4781df0a-158b-4562-b8f5-32b27615a4f8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cbcc844c356978f5bb5f34b66aa36dec9b163c1
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283004"
---
# <a name="deploy-remote-access-in-an-enterprise"></a>Implementar el acceso directo en una empresa

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se proporciona una introducción al escenario de DirectAccess para la empresa.  
  
  
> [!IMPORTANT]  
> Para implementar DirectAccess con esta guía, debe usar un servidor de DirectAccess que ejecuta Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Antes de proceder a la implementación, consulte la siguiente lista de configuraciones no compatibles, problemas conocidos y requisitos previos.  
  
-   [Configuraciones no compatibles de DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-unsupported-configurations)  
  
-   [Problemas conocidos de DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-known-issues)  
  
-   [Requisitos previos de los requisitos previos para la implementación de DirectAccess)](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/prerequisites-for-deploying-directaccess)  
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
El acceso remoto incluye varias características empresariales, como la implementación de varios servidores de Acceso remoto en una carga de clúster equilibrada con el equilibrio de carga de red (NLB) de Windows o un equilibrio de carga externo, y configura una implementación de multisitio con servidores de Acceso remoto situados en ubicaciones geográficas dispersas, e implementa DirectAccess con autenticación de cliente de dos fases mediante una contraseña de un solo uso (OTP).  
  
## <a name="in-this-scenario"></a>En este escenario  
Cada escenario empresarial está descrito en un documento que incluye instrucciones de planeación e implementación. Para obtener más información, vea:  
  
-   [Implementar el acceso remoto en un clúster](cluster/Deploy-Remote-Access-In-Cluster.md)  
  
-   [Implementación de varios servidores de acceso remoto en una implementación multisitio](multisite/Deploy-Multiple-Remote-Access-Servers-in-a-Multisite-Deployment.md)  
  
-   [Implementar el acceso remoto con autenticación OTP](otp/Deploy-RA-OTP.md)  
  
-   [Implementar acceso remoto en un entorno de varios bosques](multi-forest/Deploy-Remote-Access-in-a-Multi-Forest-Environment.md)  
  
## <a name="BKMK_APP"></a>Aplicaciones prácticas  
Los escenarios empresariales de acceso remoto proporcionan lo siguiente:  
  
-   **Mayor disponibilidad**. Implementación de varios servidores de acceso remoto en un clúster proporciona escalabilidad e incrementa la capacidad de rendimiento y la cantidad de usuarios. El equilibrio de carga del clúster proporciona alta disponibilidad. Si se produce un error en un servidor del clúster, los usuarios remotos pueden seguir teniendo acceso a la red corporativa interna a través de un servidor diferente en el clúster. La conmutación por error es transparente en la medida en que los cliente se conectan al clúster con una dirección IP virtual (VIP).  
  
-   **Facilidad de administración**. Una implementación multisitio o clúster se puede configurar y administrar como una sola entidad mediante la consola de administración de acceso remoto que se ejecuta en uno de los servidores del clúster. Además, una implementación de multisitio le permite a los administradores alinear la implementación de acceso remoto con los sitios de Active Directory, y proveer una arquitectura simplificada. La configuración compartida se puede ajustar fácilmente en los servidores de clúster o en todos lo servidores de punto de entrada de multisitio. Se puede administrar la configuración de acceso remoto desde cualquiera de los servidores del clúster o la implementación, o de forma remota con las Herramientas de administración remota del servidor (RSAT). Además, se puede supervisar todo el clúster o la implementación de multisitio desde una sola consola de administración de acceso remoto.  
  
-   **Eficiencia de costo**. Una implementación multisitio de acceso remoto permite que las empresas implementen servidores de acceso remoto en varios sitios correspondientes a las ubicaciones del cliente. Esto proporciona una experiencia de acceso predecible para clientes remotos independientemente de la ubicación, y reduce los costos y el ancho de banda de intranet gracias al enrutamiento del tráfico de clientes en Internet al servidor de acceso remoto más cercano.  
  
-   **Seguridad**. Implementar la autenticación de cliente firme con una contraseña de un solo uso (OTP) en lugar de la contraseña de Active Directory estándar aumenta la seguridad.  
  
## <a name="BKMK_NEW"></a>Roles y características que se incluyen en este escenario  
En la siguiente tabla, se muestran los roles y características que se usan en el escenario empresarial.  
  
|Rol/característica|Compatibilidad con este escenario|  
|---------|-----------------|  
|Rol del servidor de acceso remoto|El rol se instala y desinstala mediante la consola del Administrador del servidor. Este rol incluye tanto DirectAccess, que antes era una característica de Windows Server 2008 R2, como los servicios de enrutamiento y acceso remoto, que antes eran un servicio de rol bajo el rol del servidor Servicios de acceso y directivas de redes (NPAS). El rol de acceso remoto consta de dos componentes:<br /><br />1.  DirectAccess y enrutamiento y servicios de acceso remoto (RRAS) VPN de DirectAccess y VPN se administran conjuntamente en la consola de administración de acceso remoto.<br />2.  Las características de enrutamiento RRAS de enrutamiento RRAS se administran en la consola de enrutamiento y acceso remoto heredada.<br /><br />El rol del servidor de acceso remoto depende de las siguientes características del servidor:<br /><br />-Internet Information Services (IIS): esta característica es necesaria para configurar el sondeo de web de servidor y el valor predeterminado de ubicación de red.<br />-Característica de consola de administración de directivas de grupo - característica es necesaria para que DirectAccess para crear y administrar los objetos de directiva de grupo (GPO) en Active Directory y debe instalarse como una característica necesaria para el rol de servidor.|  
|Característica Herramientas de administración de acceso remoto|Esta característica se instala de la siguiente manera:<br /><br />-Se instala de forma predeterminada en un servidor de acceso remoto cuando se instala el rol de acceso remoto y es compatible con la interfaz de usuario de la consola de administración remota.<br />-Se puede instalar opcionalmente en un servidor que no ejecuta el rol de servidor de acceso remoto. En este caso, se usa para la administración remota de un equipo de acceso remoto que ejecuta DirectAccess y VPN.<br /><br />La característica de herramientas de administración de acceso remoto consiste de los siguientes elementos:<br /><br />1.  Herramientas de la línea de comandos y GUI de acceso remoto<br />2.  Módulo de acceso remoto para Windows PowerShell<br /><br />Las dependencias incluyen:<br /><br />1.  Consola de administración de directivas de grupo<br />2.  Kit de administración de Connection Manager (CMAK) de RAS<br />3.  Windows PowerShell 3.0<br />4.  Infraestructura y herramientas de administración de gráficos|  
|Windows NLB|Esta característica permite el equilibrio de carga de varios servidores de acceso remoto.|  
  

  


