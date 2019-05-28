---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: Dónde colocar a un servidor proxy de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9cc40920d366c973ace06a0b6d438a1c2d84b03e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190511"
---
# <a name="where-to-place-a-federation-server-proxy"></a>Dónde colocar a un servidor proxy de federación

Puede colocar los servicios de federación de Active Directory \(AD FS\)proxies de servidor de federación en una red perimetral para proporcionar un nivel de protección contra usuarios malintencionados que procedan de Internet. Los servidores proxy de federación son ideales para el entorno de red perimetral porque no tienen acceso a las claves privadas que se usan para crear tokens. Sin embargo, los servidores proxy de federación eficazmente pueden enrutar las solicitudes entrantes a servidores de federación que están autorizados para generar esos tokens.  
  
No es necesaria para colocar a un servidor proxy de federación dentro de la red corporativa para el asociado de cuenta o el asociado de recurso porque los equipos cliente que están conectados a la red corporativa pueden comunicarse directamente con el servidor de federación. En este escenario, el servidor de federación también proporciona funcionalidad de proxy de servidor de federación para los equipos cliente que provienen de la red corporativa.  
  
Como suele suceder con redes perimetrales, una intranet\-accesible desde el servidor de seguridad se establece entre la red perimetral y la red corporativa y de Internet\-accesible desde el servidor de seguridad con frecuencia se establece entre la red perimetral y el Internet. En este escenario, el servidor proxy de federación se ubica entre estos firewalls en la red perimetral.  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configuración de los servidores de firewall para un servidor proxy de federación  
Para que el proceso de redirección de federation server proxy se realice correctamente, todos los servidores de firewall deben estar configurados para permitir el protocolo seguro de transferencia de hipertexto \(HTTPS\) tráfico. El uso de HTTPS es necesario porque los servidores de firewall deben publicar al servidor proxy de federación, mediante el puerto 443, por lo que el servidor proxy de federación en la red perimetral pueda acceder al servidor de federación en la red corporativa.  
  
> [!NOTE]  
> Todas las comunicaciones a y desde los equipos cliente también ocurren a través de HTTPS.  
  
Además, Internet\-accesible desde el servidor de seguridad, como un equipo que ejecuta Microsoft Internet Security and Acceleration \(ISA\) servidor, usa un proceso conocido como publicación de servidor para distribuir el cliente de Internet solicitudes para el perímetro adecuado y los servidores de la red corporativa, como servidores proxy de federación o servidores de federación.  
  
Las reglas de publicación de servidor determinan el funcionamiento de la publicación de servidor, básicamente, filtrando todas las solicitudes entrantes y salientes a través del equipo que ejecuta ISA Server. Las reglas de publicación de servidor asignan las solicitudes de cliente entrantes a los servidores adecuados que hay detrás del equipo que ejecuta ISA Server. Para obtener información acerca de cómo configurar ISA Server para publicar un servidor, consulte [crear una regla de publicación Web segura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
En el mundo federado de AD FS, estas solicitudes de cliente suelen realizarse en una dirección URL específica, por ejemplo, una dirección de URL de identificador de servidor de federación como http://fs.fabrikam.com. Dado que estas solicitudes de cliente provienen de desde Internet, Internet\-accesible desde el servidor de seguridad debe estar configurado para publicar la URL de identificador del servidor de federación para cada servidor proxy de federación que se implementa en la red perimetral.  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>Configuración de ISA Server para permitir SSL  
Para facilitar las comunicaciones seguras de AD FS, debe configurar ISA Server para permitir que la capa de Sockets seguros \(SSL\) las comunicaciones entre los elementos siguientes:  
  
-   **Los servidores de federación y servidores proxy de federación.** Un canal SSL es necesario para todas las comunicaciones entre los servidores de federación y servidores proxy de federación. Por lo tanto, debe configurar ISA Server para que permita una conexión SSL entre la red corporativa y la red perimetral.  
  
-   **Los equipos cliente, servidores de federación y servidores proxy de federación.** Para que puedan realizarse las comunicaciones entre los equipos cliente y servidores de federación o entre equipos cliente y servidores proxy de federación, puede colocar un equipo que ejecuta ISA Server delante del servidor de federación o servidor proxy de federación.  
  
    Si su organización realiza la autenticación de cliente SSL en el servidor de federación o servidor proxy de federación, cuando se coloca en un equipo que ejecuta ISA Server delante del servidor de federación o servidor proxy de federación, el servidor debe configurarse para paso\-a través de la conexión SSL porque la conexión SSL debe terminar en el servidor de federación o servidor proxy de federación.  
  
    Si su organización no realiza la autenticación de cliente SSL en el servidor de federación o servidor proxy de federación, es una opción adicional terminar la conexión SSL en el equipo que ejecuta ISA Server y, a continuación, re\-establecer una conexión SSL con el servidor de federación o servidor proxy de federación.  
  
> [!NOTE]  
> El servidor de federación o servidor proxy de federación requiere que la conexión sea segura mediante SSL para proteger el contenido del token de seguridad.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
