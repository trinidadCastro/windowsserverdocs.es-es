---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: "Ubicación de un servidor de federación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 376cec7f3a4fb1f988ac5d458b05220c7b9de970
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server"></a>Ubicación de un servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seguridad mejores prácticas, coloca los servidores de federación de los servicios de federación de Active Directory \(AD FS\) delante de un firewall y conectarlos a la red corporativa para evitar la exposición de Internet. Esto es importante porque los servidores de federación tienen autorización completa para conceder los tokens de seguridad. Por lo tanto, deben tener la misma protección que un controlador de dominio. Si un servidor de federación se ve comprometido, un usuario malintencionado tiene la capacidad para emitir tokens de acceso completa a todas las aplicaciones Web y a los servidores de federación que están protegidos por los servicios de federación de Active Directory \(AD FS\) en todas las organizaciones de partner de recursos.  
  
> [!NOTE]  
> Seguridad recomendada, evitar que los servidores de federación accesibles directamente en Internet. Deberías asignarle a los servidores de federación acceso directo a Internet durante la configuración de un entorno de laboratorio de prueba o cuando la organización no tiene una red de perímetro.  
  
Para las redes corporativas típicas, se establece un firewall intranet\ frontal entre la red corporativa y la red perimetral y un firewall Internet\ orientados a menudo se establece entre la red perimetral e Internet. En esta situación, el servidor de federación se ubica en la red corporativa y no es accesible directamente por los clientes de Internet.  
  
> [!NOTE]  
> Los equipos cliente que están conectados a la red corporativa pueden comunicarse directamente con el servidor de federación mediante la autenticación integrada de Windows.  
  
Un proxy de servidor de federación debe colocarse en la red perimetral antes de configurar los servidores de firewall para su uso con AD FS. Para obtener más información, consulta [dónde colocar un Proxy de servidor de federación](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configuración de los servidores de firewall de un servidor de federación  
Para que los servidores de federación puedan comunicarse directamente con los servidores proxy de servidor de federación, el servidor de seguridad de intranet debe configurarse para permitir el tráfico de protocolo seguro de transferencia de hipertexto \(HTTPS\) desde el proxy de servidor de federación al servidor de federación. Este es un requisito porque el servidor de seguridad de intranet debes publicar el servidor de federación mediante el puerto 443 para que el proxy de federación de servidor en la red perimetral puede obtener acceso al servidor de federación.  
  
Además, la orientación de intranet\ firewall servidor, como un servidor que ejecuta Internet Security and Acceleration \(ISA\) Server, utiliza un proceso conocido como publicación del servidor para distribuir las solicitudes de cliente de Internet en los servidores de federación corporativa apropiado. Esto significa que debe crear manualmente una regla de publicación de servidor en el servidor de intranet servidor ISA que publica la URL del servidor de federación clúster, por ejemplo, http:///\/fs.fabrikam.com.  
  
Para obtener más información sobre cómo configurar la publicación de servidores en una red perimetral, consulta [dónde colocar un Proxy de servidor de federación](Where-to-Place-a-Federation-Server-Proxy.md). Para obtener información sobre cómo configurar el servidor ISA para publicar un servidor, consulte [crear una regla de publicación Web segura](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
