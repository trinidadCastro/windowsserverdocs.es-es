---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: Dónde colocar un servidor de federación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 503b1f206786d36364f9f0033ec0ea88330c444f
ms.sourcegitcommit: 2cc251eb5bc3069bf09bc08e06c3478fcbe1f321
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2020
ms.locfileid: "84333866"
---
# <a name="where-to-place-a-federation-server"></a>Dónde colocar un servidor de federación

Como práctica recomendada de seguridad, coloque Servicios de federación de Active Directory (AD FS) \( AD FS \) servidores de Federación detrás de un firewall y conéctelos a la red corporativa para evitar la exposición de Internet. Esto es importante, ya que los servidores de Federación tienen autorización completa para conceder tokens de seguridad. Por lo tanto, deben tener la misma protección que un controlador de dominio. Si un servidor de Federación está en peligro, un usuario malintencionado tiene la capacidad de emitir tokens de acceso completo a todas las aplicaciones web y a los servidores de Federación protegidos por Servicios de federación de Active Directory (AD FS) \( AD FS \) en todas las organizaciones de asociados de recurso.  
  
> [!NOTE]  
> Como procedimiento de seguridad recomendado, evite que sus servidores de Federación estén directamente accesibles en Internet. Considere la posibilidad de asignar a los servidores de Federación acceso directo a Internet solo cuando esté configurando un entorno de laboratorio de pruebas o cuando su organización no disponga de una red perimetral.  
  
En el caso de las redes corporativas típicas, \- se establece un firewall orientado a la intranet entre la red corporativa y la red perimetral, y \- a menudo se establece un firewall con conexión a Internet entre la red perimetral e Internet. En esta situación, el servidor de Federación se encuentra dentro de la red corporativa y no es accesible directamente para los clientes de Internet.  
  
> [!NOTE]  
> Los equipos cliente que están conectados a la red corporativa pueden comunicarse directamente con el servidor de Federación a través de la autenticación integrada de Windows.  
  
Se debe colocar un servidor proxy de Federación en la red perimetral antes de configurar los servidores de Firewall para su uso con AD FS. Para más información, consulte [Ubicación de un servidor proxy de federación](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configuración de los servidores de firewall para un servidor de federación  
Para que los servidores de Federación puedan comunicarse directamente con servidores proxy de Federación, el servidor de Firewall de la intranet debe configurarse para \( permitir \) el tráfico HTTPS del protocolo seguro de transferencia de hipertexto desde el servidor proxy de Federación al servidor de Federación. Este es un requisito porque el servidor de Firewall de la intranet debe publicar el servidor de Federación mediante el puerto 443 para que el servidor proxy de Federación de la red perimetral pueda obtener acceso al servidor de Federación.  
  
Además, el servidor de \- firewall con conexión a la intranet, como un servidor que ejecuta Internet Security and Acceleration \( ISA \) Server, usa un proceso conocido como publicación de servidor para distribuir las solicitudes de cliente de Internet a los servidores de Federación corporativos adecuados. Esto significa que debe crear manualmente una regla de publicación de servidor en el servidor de intranet que ejecuta ISA Server y que publica la dirección URL del servidor de Federación en clúster, por ejemplo, http: \/ \/ FS.fabrikam.com.  
  
Para obtener más información acerca de cómo configurar la publicación de servidor en una red perimetral, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md). Para obtener información sobre cómo configurar ISA Server para publicar un servidor, vea [crear una regla de publicación web segura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
