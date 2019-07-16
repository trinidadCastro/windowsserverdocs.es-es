---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: Cuándo crear un servidor proxy de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b41c2194940c85e39e5a3724f747dd12c2544259
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190638"
---
# <a name="when-to-create-a-federation-server-proxy"></a>Cuándo crear un servidor proxy de federación

Creación de un servidor proxy de federación en su organización añade capas de seguridad adicional para los servicios de federación de Active Directory \(AD FS\) implementación. Considere la posibilidad de implementar a un servidor proxy de federación en la red perimetral de su organización cuando desea:  
  
-   Impedir que los equipos cliente externo acceso directo a los servidores de federación. Al implementar a un servidor proxy de federación en la red perimetral, aíslas a los servidores de federación para que sean accesibles solo por los equipos cliente que están conectados a la red corporativa a través de servidores proxy de federación, que actúan en nombre los equipos cliente externo. Los proxies de servidor de federación no tienen acceso a las claves privadas que se utilizan para generar tokens. Para obtener más información, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   Proporcionan una manera cómoda para diferenciar el inicio de sesión\-experiencia para los usuarios que proceden de Internet en contraposición a los usuarios que proceden de la red corporativa mediante la autenticación integrada de Windows. Un servidor proxy de federación recopila credenciales o los detalles del dominio de inicio de los equipos cliente de Internet mediante el uso de la detección de proveedor de inicio de sesión, cierre de sesión y la identidad \(homerealmdiscovery.aspx\) páginas que se almacenan en la federación servidor proxy.  
  
    En cambio, los equipos cliente que proceden de una experiencia diferente, encuentro con la red corporativa se basa en la configuración del servidor de federación. A menudo, el servidor de federación de la red corporativa está configurado para la autenticación integrada de Windows, que proporciona un inicio de sesión sin problemas\-experiencia para los usuarios de la red corporativa.  
  
La función que desempeña un servidor proxy de federación de su organización depende de si se coloca al servidor proxy de federación en la organización del asociado de cuenta o en la organización del asociado de recurso. Por ejemplo, cuando un servidor proxy de federación se coloca en la red perimetral del asociado de cuenta, su función es recopilar la información de credenciales de usuario de los clientes de explorador. Cuando un servidor proxy de federación se coloca en la red perimetral del asociado de recurso, retransmite solicitudes a un servidor de federación de recursos de token de seguridad y genera tokens de seguridad de la organización en respuesta a los tokens de seguridad proporcionadas por su asociados de cuenta.  
  
Para obtener más información, consulte [revisar el rol de servidor Proxy de federación del asociado de cuenta](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) y [revisar el rol de servidor Proxy de federación del asociado de recurso](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>Cómo crear un servidor proxy de federación  
Puede crear un servidor proxy de federación mediante el comando Fsconfig.exe o el Asistente de configuración de AD FS Federation Server Proxy\-herramienta de línea. Para obtener instrucciones sobre cómo hacerlo, consulte [configurar un equipo para el rol de servidor Proxy de federación](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
Para obtener información general acerca de cómo configurar todos los requisitos previos necesarios para implementar un servidor proxy de federación, consulte [lista de comprobación: Configurar un servidor Proxy de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
