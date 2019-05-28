---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: Dónde colocar un servidor de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b883126f60950c0015b3a21e2ca5abc251b25b84
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190457"
---
# <a name="where-to-place-a-federation-server"></a>Dónde colocar un servidor de federación

Como práctica recomendada de seguridad, Active Directory Federation Services lugar \(AD FS\)servidores de federación delante de un firewall y conéctelos a la red corporativa para evitar la exposición de Internet. Esto es importante porque los servidores de federación tienen autorización completa para conceder tokens de seguridad. Por lo tanto, deben tener la misma protección que un controlador de dominio. Si un servidor de federación está en peligro, un usuario malintencionado tiene la capacidad de emitir tokens de acceso completo a todas las aplicaciones Web y servidores de federación que están protegidos por los servicios de federación de Active Directory \(AD FS\) en todos los recursos organizaciones asociadas.  
  
> [!NOTE]  
> Como procedimiento de seguridad recomendado, evite que los servidores de federación directamente accesibles en Internet. Considere la posibilidad de dar acceso directo a Internet de los servidores de federación solo cuando esté configurando un entorno de laboratorio de pruebas o cuando su organización no tiene una red perimetral.  
  
Para las redes corporativas típicas, una intranet\-accesible desde el servidor de seguridad se establece entre la red corporativa y la red perimetral y de Internet\-accesible desde el servidor de seguridad con frecuencia se establece entre la red perimetral y el Internet. En esta situación, el servidor de federación se encuentra dentro de la red corporativa y no es directamente accesible para los clientes de Internet.  
  
> [!NOTE]  
> Los equipos cliente que están conectados a la red corporativa pueden comunicarse directamente con el servidor de federación mediante la autenticación integrada de Windows.  
  
Un servidor proxy de federación debe colocarse en la red perimetral antes de configurar los servidores de firewall para su uso con AD FS. Para obtener más información, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configuración de los servidores de firewall para un servidor de federación  
Para que los servidores de federación pueden comunicarse directamente con servidores proxy de federación, el servidor de seguridad de la intranet debe configurarse para permitir el protocolo seguro de transferencia de hipertexto \(HTTPS\) tráfico desde el servidor proxy de federación a el servidor de federación. Este es un requisito porque el servidor de seguridad de la intranet debe publicar el servidor de federación mediante el puerto 443 para que el servidor proxy de federación en la red perimetral pueda acceder al servidor de federación.  
  
Además, la intranet\-accesible desde el servidor de seguridad, como un servidor que ejecuta Internet Security and Acceleration \(ISA\) servidor, usa un proceso conocido como publicación de servidor para distribuir las solicitudes de cliente de Internet a la servidores de federación corporativa adecuado. Esto significa que debe crear manualmente una regla de publicación de servidor en el servidor de intranet que ejecuta ISA Server que publica la URL del servidor de federación en clúster, por ejemplo, http:\/\/fs.fabrikam.com.  
  
Para obtener más información acerca de cómo configurar la publicación de servidor en una red perimetral, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md). Para obtener información acerca de cómo configurar ISA Server para publicar un servidor, consulte [crear una regla de publicación Web segura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
