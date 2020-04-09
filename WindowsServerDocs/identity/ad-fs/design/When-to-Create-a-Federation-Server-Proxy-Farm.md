---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: Cuándo se debe crear una granja de servidores proxy de federación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d4b2b889159dee9f3b93a54a2b1924be286792f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858498"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Cuándo se debe crear una granja de servidores proxy de federación

Considere la posibilidad de instalar más servidores proxy de Federación cuando tenga un gran Servicios de federación de Active Directory (AD FS) \(AD FS\) implementación y desee proporcionar tolerancia a errores, equilibrio de\-de carga y escalabilidad para la implementación de proxy. La acción de crear dos o más proxies de servidor de Federación en la misma red perimetral y configurar cada uno de ellos para proteger el mismo AD FS Servicio de federación crea una granja de servidores proxy de Federación.  
  
Puede crear una granja de servidores proxy de Federación o instalar más servidores proxy de Federación en una granja existente mediante el Asistente para configuración de servidor proxy de Federación de AD FS. Para obtener más información, consulte [When to Create a Federation Server Proxy](When-to-Create-a-Federation-Server-Proxy.md).  
  
Antes de que todos los servidores proxy de Federación puedan funcionar juntos como una granja, primero debe agruparlos en una dirección IP y un sistema de nombres de dominio \(DNS\) nombre de dominio completo \(FQDN\). Puede agrupar los servidores mediante la implementación del equilibrio de carga de red de Microsoft \(NLB\) dentro de la red perimetral. Las tareas de la tabla siguiente requieren que se configure correctamente NLB para agrupar los proxies de servidor de Federación en la granja.  
  
Para obtener más información sobre cómo configurar un FQDN para un clúster con la tecnología NLB de Microsoft, consulte [especificación de los parámetros de clúster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configuración de los servidores proxy de federación para una granja  
En la tabla siguiente se describen las tareas que deben completarse para que cada servidor proxy de Federación pueda participar en una granja.  
  
|Tarea|Descripción|  
|--------|---------------|  
|Señale todos los servidores proxy de la granja al mismo AD FS nombre Servicio de federación|Al crear los servidores proxy de Federación, debe escribir el mismo nombre Servicio de federación en el Asistente para configuración de servidor proxy de Federación de AD FS para todos los servidores proxy de Federación que participarán en la granja. El servidor proxy de Federación utiliza la dirección URL que constituye este nombre de host DNS para determinar en qué AD FS Servicio de federación instancia se pone en contacto.<p>Para obtener más información, consulte [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Obtener y compartir certificados|Puede obtener un certificado de autenticación de servidor de una entidad de certificación pública \(CA\)(por ejemplo, VeriSign) y, a continuación, configurar el certificado para que todos los servidores proxy de Federación compartan la misma parte de clave privada del mismo certificado en el sitio web predeterminado para cada servidor proxy de Federación. Para compartir el certificado, debe instalar el mismo certificado de autenticación de servidor en el sitio web predeterminado para cada servidor proxy de Federación. Para obtener más información, vea [importar un certificado de autenticación de servidor al sitio web predeterminado](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<p>Para obtener más información, consulte [Certificate Requirements for Federation Server Proxies](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Para obtener más información acerca de cómo agregar nuevos proxies de servidor de Federación para crear una granja de servidores proxy de Federación, vea [lista de comprobación: configurar un servidor proxy de Federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
