---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: Cuándo se debe crear una granja de servidores proxy de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c33475d7420383448439e2b769562e55127c7b0e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190626"
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Cuándo se debe crear una granja de servidores proxy de federación

Considere la posibilidad de instalar servidores proxy de federación adicionales cuando haya un gran Active Directory Federation Services \(AD FS\) implementación y desea proporcionar tolerancia a errores, carga\-equilibrio y la escalabilidad para la implementación de proxy. La acción de creación de proxies de servidor de federación de dos o más en la misma red perimetral y configurar cada uno de ellos para proteger el mismo servicio de federación de AD FS crea una granja de proxy de servidor de federación.  
  
Puede crear una granja de proxy de servidor de federación o instalar a servidores proxy de federación adicional a una granja existente mediante el Asistente de configuración de AD FS Federation Server Proxy. Para obtener más información, consulte [cuándo se debe crear un servidor Proxy de federación](When-to-Create-a-Federation-Server-Proxy.md).  
  
Antes de que todos los servidores proxy de federación pueden funcionar juntos como una granja de servidores, deben agruparlos en clúster en una dirección IP y un sistema de nombres de dominio \(DNS\) nombre de dominio completo \(FQDN\). Puede agrupar los servidores mediante la implementación de equilibrio de carga de red de Microsoft \(NLB\) dentro de la red perimetral. Las tareas en la tabla siguiente requieren NLB para configurarse correctamente para los servidores proxy de federación en la granja de servidores del clúster.  
  
Para obtener más información sobre cómo configurar un FQDN para un clúster con la tecnología NLB de Microsoft, consulte [especificando los parámetros de clúster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configuración de los servidores proxy de federación para una granja  
En la tabla siguiente se describe las tareas que deben completarse para que cada servidor proxy de federación puede participar en una granja de servidores.  
  
|Tarea|Descripción|  
|--------|---------------|  
|Seleccione a todos los servidores proxy en la granja de servidores en el mismo nombre de servicio de federación de AD FS|Al crear la federación de servidores proxy, debe escribir el mismo nombre de servicio de federación en el Asistente de configuración de AD FS Federation Server Proxy para todos los servidores proxy de federación que participarán en la granja de servidores. El servidor proxy de federación usa la dirección URL que constituye este nombre de host DNS para determinar qué instancia de servicio de federación de AD FS, los contactos.<br /><br />Para obtener más información, consulte [configurar un equipo para el rol de servidor Proxy de federación](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Obtener y compartir certificados|Puede obtener un servidor de certificado de autenticación de una entidad de certificación pública \(CA\)— por ejemplo, VeriSign y, a continuación, configure el certificado para que todos los servidores proxy de federación compartan la misma parte de clave privada de la mismo certificado en el sitio Web predeterminado para cada servidor proxy de federación. Para compartir el certificado, debe instalar el mismo certificado de autenticación de servidor en el sitio Web predeterminado para cada servidor proxy de federación. Para obtener más información, consulte [importar un certificado de autenticación de servidor al sitio Web predeterminado](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<br /><br />Para obtener más información, consulte [requisitos de certificados para servidores proxy de federación](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Para obtener más información acerca de cómo agregar nuevos servidores proxy de federación para crear una granja de proxy de servidor de federación, consulte [lista de comprobación: Configurar un servidor Proxy de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
