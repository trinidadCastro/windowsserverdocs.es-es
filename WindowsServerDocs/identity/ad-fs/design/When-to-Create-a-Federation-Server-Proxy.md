---
description: 'Más información acerca de: Cuándo crear un servidor proxy de Federación'
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: Cuándo crear un servidor proxy de federación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4da88560f372f56594d127c1db26f1bf8edd4bea
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049943"
---
# <a name="when-to-create-a-federation-server-proxy"></a>Cuándo crear un servidor proxy de federación

Al crear un servidor proxy de Federación en su organización, se agregan niveles de seguridad adicionales a la implementación de Servicios de federación de Active Directory (AD FS) \( AD FS \) . Considere la posibilidad de implementar un servidor proxy de Federación en la red perimetral de su organización cuando quiera:

-   Impedir que los equipos cliente externos tengan acceso directo a los servidores de Federación. Mediante la implementación de un servidor proxy de Federación en la red perimetral, aíslas de forma eficaz los servidores de Federación para que solo se pueda tener acceso a ellos en los equipos cliente que han iniciado sesión en la red corporativa a través de servidores proxy de Federación, que actúan en nombre de los equipos cliente externos. Los proxies de servidor de federación no tienen acceso a las claves privadas que se utilizan para generar tokens. Para más información, consulte [Ubicación de un servidor proxy de federación](Where-to-Place-a-Federation-Server-Proxy.md).

-   Proporcionar una manera cómoda de diferenciar la experiencia de inicio de sesión \- para los usuarios que provienen de Internet en lugar de los usuarios que proceden de la red corporativa mediante la autenticación integrada de Windows. Un servidor proxy de Federación recopila las credenciales o los detalles de dominio de inicio de los equipos cliente de Internet mediante las páginas de inicio de sesión, cierre de sesión y detección de proveedor de identidades \( homerealmdiscovery. aspx \) que se almacenan en el servidor proxy de Federación.

    En cambio, los equipos cliente que proceden de la red corporativa encuentran una experiencia diferente, según la configuración del servidor de Federación. El servidor de Federación de red corporativa suele estar configurado para la autenticación integrada de Windows, que proporciona una experiencia de inicio de sesión sin problemas \- para los usuarios de la red corporativa.

El rol que desempeña un servidor proxy de Federación en su organización depende de si coloca el servidor proxy de Federación en la organización del asociado de cuenta o en la organización del asociado de recurso. Por ejemplo, cuando se coloca un servidor proxy de Federación en la red perimetral del asociado de cuenta, su función es recopilar la información de credenciales de usuario de los clientes del explorador. Cuando se coloca un servidor proxy de Federación en la red perimetral del asociado de recurso, retransmite las solicitudes de tokens de seguridad a un servidor de Federación de recursos y genera tokens de seguridad de la organización en respuesta a los tokens de seguridad proporcionados por sus asociados de cuenta.

Para obtener más información, consulta [Review the Role of the Federation Server Proxy in the Account Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) y [Review the Role of the Federation Server Proxy in the Resource Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md).

## <a name="how-to-create-a-federation-server-proxy"></a>Cómo crear un servidor proxy de federación
Puede crear un servidor proxy de Federación mediante el Asistente para configuración de servidor proxy de Federación de AD FS o la herramienta de línea de comandos Fsconfig.exe \- . Para obtener instrucciones sobre cómo hacerlo, consulta [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).

Para obtener información general sobre cómo configurar todos los requisitos previos necesarios para implementar un servidor proxy de federación, consulta [Checklist: Setting Up a Federation Server Proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
