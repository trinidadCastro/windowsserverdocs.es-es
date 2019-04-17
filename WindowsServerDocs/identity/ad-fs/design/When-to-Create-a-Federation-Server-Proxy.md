---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: "Cuándo se debe crear a un Proxy de federación de servidor"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1f0253dfb5a690371dae1a2bfcb6b7520077d473
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server-proxy"></a>Cuándo se debe crear a un Proxy de federación de servidor

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crear a un proxy de servidor de federación de la organización agrega capas de seguridad adicional para la implementación de \(AD FS\) de los servicios de federación de Active Directory. Considera la posibilidad de implementar a un proxy de federación de servidor de red del perímetro de la organización cuando quieras:  
  
-   Impedir que los equipos cliente externo acceder directamente a los servidores de federación. Al implementar a un proxy de servidor de federación de la red perimetral, eficazmente aísla a los servidores de federación para que solo pueden acceder los equipos cliente que están conectados a la red corporativa a través de proxies de servidor de federación que actúe en nombre de los equipos cliente externo. Proxies de servidor de federación no tienen acceso a las claves privadas que se usan para producir tokens. Para obtener más información, consulta [dónde colocar un Proxy de servidor de federación](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   Proporcionan una forma cómoda para diferenciar la experiencia de sign\ para los usuarios que proceden de Internet en contraposición a los usuarios que proceden de la red corporativa mediante la autenticación integrada de Windows. Un proxy de servidor de federación recopila credenciales o detalles de dominio de inicio de los equipos de cliente de Internet mediante el inicio de sesión, cierre de sesión y páginas \(homerealmdiscovery.aspx\) de detección de proveedor de identidad que se almacenan en el proxy de servidor de federación.  
  
    En cambio, los equipos cliente que provienen de la encuentro de la red corporativa una experiencia diferente, según la configuración del servidor de federación. El servidor de federación de la red corporativa a menudo está configurado para la autenticación integrada de Windows, que proporciona una experiencia de sign\ sin problemas para usuarios de la red corporativa.  
  
El papel que desempeña un proxy de servidor de federación de la organización depende de si colocas al proxy de servidor de federación de la organización de partner de la cuenta o en la organización de partner de recurso. Por ejemplo, cuando un proxy de federación de servidor se coloca en la red perimetral de asociado de cuenta, su función es recopilar la información de credenciales de usuario de los clientes del explorador. Cuando un proxy de federación de servidor se coloca en la red perimetral de asociado de recurso, pasa las solicitudes de token de seguridad a un servidor de federación de recursos y genera tokens de seguridad de la organización en respuesta a los tokens de seguridad que se proporcionan con sus asociados de cuenta.  
  
Para obtener más información, consulta [revisar el rol del servidor Proxy Server federación del asociado de cuenta](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) y [revisar el rol del servidor Proxy de servidor de federación en el Partner de recursos](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>Cómo crear a un proxy de federación de servidor  
Puedes crear a un proxy de servidor de federación mediante el Asistente para configuración de Proxy de federación de servidor de AD FS o la herramienta de línea de Web\ Fsconfig.exe. Para obtener instrucciones sobre cómo hacerlo, consulta [configurar un equipo para el rol de Proxy del servidor de federación](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
Para obtener información general sobre cómo configurar todos los requisitos previos necesarios para implementar un proxy de federación de servidor, consulta [lista de comprobación: configuración de un Proxy de servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
