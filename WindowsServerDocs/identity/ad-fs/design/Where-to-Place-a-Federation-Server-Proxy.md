---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: "Ubicación de un Proxy de federación de servidor"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bde30f694c6490962edaa0c3fe1543e74ba7fd7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server-proxy"></a>Ubicación de un Proxy de federación de servidor

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puedes colocar a proxies de servidor de federación de los servicios de federación de Active Directory \(AD FS\) en una red perimetral para proporcionar una capa de protección frente a los usuarios malintencionados que pueden provenir de Internet. Proxies de servidor de federación son ideales para el entorno de red del perímetro ya no tienen acceso a las claves privadas que se usan para crear tokens. Sin embargo, proxies de servidor de federación eficaz pueden enrutar solicitudes entrantes a los servidores de federación están autorizados para producir esos tokens.  
  
No es necesario colocar un proxy de servidor de federación dentro de la red corporativa para el asociado de cuenta o el asociado de recurso ya que los equipos cliente que están conectados a la red corporativa pueden comunicarse directamente con el servidor de federación. En este escenario, el servidor de federación también proporciona funcionalidad de proxy del servidor de federación para los equipos cliente que provienen de la red corporativa.  
  
Como es típico con redes perimetrales, un firewall intranet\ frontal se establece entre la red perimetral y la red corporativa y un firewall Internet\ orientados a menudo se establece entre la red perimetral e Internet. En este escenario, el proxy de servidor de federación entre dos de estos servidores de seguridad en la red de perímetro.  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configuración de los servidores de firewall para un proxy de federación de servidor  
Para que el proceso de redireccionamiento de federación servidor proxy se realice correctamente, todos los servidores de firewall deben configurarse para permitir el tráfico de protocolo seguro de transferencia de hipertexto \(HTTPS\). El uso de HTTPS es necesario porque los servidores de firewall deben publicar al proxy de servidor de federación, mediante el puerto 443, para que el proxy de federación de servidor en la red perimetral puede obtener acceso al servidor de federación de la red corporativa.  
  
> [!NOTE]  
> Todas las comunicaciones hacia y desde los equipos cliente también se producen a través de HTTPS.  
  
Además, la orientación de Internet\ firewall servidor, como un equipo que ejecute Microsoft Internet Security and Acceleration \(ISA\) Server, utiliza un proceso conocido como publicación del servidor para distribuir las solicitudes de cliente de Internet al perímetro adecuado y servidores de la red corporativa, como servidores proxy de servidor de federación o servidores de federación.  
  
Reglas de publicación de servidor determinan cómo funciona la publicación de servidor, en esencia, filtrado todas las solicitudes entrantes y salientes en el equipo servidor ISA. Reglas de publicación de servidor asignan solicitudes entrantes del cliente a los servidores adecuados detrás del equipo ISA Server. Para obtener información sobre cómo configurar el servidor ISA para publicar un servidor, consulte [crear una regla de publicación Web seguro](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
En el mundo federado de AD FS, estas solicitudes de cliente normalmente se realizan en una dirección URL específica, por ejemplo, una URL federación servidor identificador como http://fs.fabrikam.com. Dado que estas solicitudes de cliente vienen en desde Internet, el servidor a través de Internet\ debe configurarse para publicar la URL del identificador de servidor de federación para cada proxy de servidor de federación que se implementa en la red de perímetro.  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>Configurar el servidor ISA para permitir que SSL  
Para facilitar las comunicaciones de AD FS seguras, debe configurar el servidor ISA para permitir la comunicación de capa de Sockets seguros \(SSL\) entre las siguientes acciones:  
  
-   **Los servidores de federación y los proxies de servidor de federación.** Un canal SSL es necesario para todas las comunicaciones entre servidores de federación y los proxies de servidor de federación. Por lo tanto, debes configurar el servidor ISA para permitir que una conexión SSL entre la red corporativa y la red de perímetro.  
  
-   **Los equipos cliente, los servidores de federación y los proxies de servidor de federación.** Para que puedan producirse comunicaciones entre los equipos cliente y servidores de federación o entre los equipos cliente y los proxies de servidor de federación, puedes colocar un equipo que ejecute el servidor ISA delante del servidor de federación o proxy del servidor de federación.  
  
    Si tu organización realiza la autenticación de clientes SSL en el servidor de federación o proxy del servidor de federación, cuando se coloca en un equipo que ejecute el servidor ISA delante del servidor de federación o proxy del servidor de federación, el servidor debe configurarse para pass\ a través de la conexión de SSL porque debe finalizar la conexión de SSL en el servidor de federación o proxy del servidor de federación.  
  
    Si la organización no realiza la autenticación de clientes SSL en el servidor de federación o proxy del servidor de federación, es una opción adicional terminar la conexión de SSL en el equipo que ejecuta el servidor ISA y, a continuación, re\-establecer una conexión SSL para el servidor de federación o proxy del servidor de federación.  
  
> [!NOTE]  
> El servidor de federación o proxy del servidor de federación requiere que la conexión se protegen mediante SSL para proteger el contenido del token de seguridad.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
